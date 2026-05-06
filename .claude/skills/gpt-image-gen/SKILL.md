---
name: gpt-image-gen
description: Generate a still image via the OpenAI Images API (model `gpt-image-2`). Pure technical wrapper — accepts a prompt + output path, returns a PNG file on disk. Use this skill whenever a sub-agent needs to produce an image; do not write your own curl logic.
---

# gpt-image-gen — OpenAI Images API wrapper

## When to invoke

Any sub-agent that needs to render a still image from a text prompt. The caller is responsible for **what** the prompt says (creative direction, style, references). This skill is responsible only for **how** the API is called and how the bytes land on disk.

The canonical caller in this project is the `sami-image-gen` agent (see `.claude/agents/sami-image-gen.md`). Other agents may call this skill directly when image generation is incidental and Sami's curation is not needed.

## Inputs

| Field | Required | Default | Notes |
|---|---|---|---|
| `prompt` | yes | — | Free-text prompt sent to the model. Caller must finalize it before invoking. |
| `output_path` | yes | — | Absolute or repo-relative path to the destination `.png`. Parent directory must exist. |
| `size` | no | `1024x1024` | Allowed values per OpenAI docs (`1024x1024`, `1024x1536`, `1536x1024`, `auto`). |
| `quality` | no | `medium` | `low`, `medium`, `high`, `auto`. |
| `model` | no | `gpt-image-2` | See **Notes** below if `model_not_found` returned. |

The skill **assumes `OPENAI_API_KEY` is exported** in the shell environment (loaded from `.env`). If the caller cannot rely on that, it should source `.env` before invoking.

## How to call

### Step 1 — Verify the key is present

```bash
test -n "$OPENAI_API_KEY" || { echo "OPENAI_API_KEY not set" >&2; exit 1; }
```

If the variable is empty, source `.env`:

```bash
set -a; source .env; set +a
```

### Step 2 — POST to the API and capture the JSON

Save the raw response to a temp file so we can both decode it and inspect it on error:

```bash
RESPONSE_FILE="$(mktemp -t gpt-image-gen.XXXXXX.json)"

curl -sS -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d @- > "$RESPONSE_FILE" <<EOF
{
  "model": "gpt-image-2",
  "prompt": $(printf '%s' "$PROMPT" | python3 -c 'import json,sys; print(json.dumps(sys.stdin.read()))'),
  "size": "1024x1024",
  "quality": "medium",
  "output_format": "png"
}
EOF
```

The `python3 -c '... json.dumps ...'` step JSON-escapes the prompt safely (newlines, quotes, unicode). Do **not** interpolate `$PROMPT` directly into the JSON body — it breaks on any quote in the prompt.

### Step 3 — Decode `b64_json` to PNG (jq primary, python fallback)

The API returns `{"data":[{"b64_json":"..."}]}`. Decode the first element. Try `jq` first (fast, common); fall back to Python (always available where Python is installed, including Git Bash on Windows).

```bash
OUTPUT_PATH="$1"   # the caller-provided path

if command -v jq >/dev/null 2>&1; then
  jq -r '.data[0].b64_json' "$RESPONSE_FILE" | base64 --decode > "$OUTPUT_PATH"
else
  python3 - "$RESPONSE_FILE" "$OUTPUT_PATH" <<'PY'
import json, base64, sys
with open(sys.argv[1]) as f:
    data = json.load(f)
if "data" not in data:
    sys.stderr.write(json.dumps(data, ensure_ascii=False) + "\n")
    sys.exit(2)
b64 = data["data"][0]["b64_json"]
with open(sys.argv[2], "wb") as out:
    out.write(base64.b64decode(b64))
PY
fi
```

The Python fallback also handles the case where the API returned an error object (no `data` field) — it prints the error JSON to stderr and exits non-zero, so the caller sees what went wrong.

### Step 4 — Verify the file is non-empty

```bash
test -s "$OUTPUT_PATH" || { echo "Output file is empty or missing: $OUTPUT_PATH" >&2; exit 3; }
SIZE_BYTES=$(wc -c < "$OUTPUT_PATH" | tr -d ' ')
echo "wrote $SIZE_BYTES bytes to $OUTPUT_PATH"
```

`test -s` is the **gate** for reporting success. Never report "image generated" before this check passes.

### Step 5 — Clean up the temp response file

```bash
rm -f "$RESPONSE_FILE"
```

(Skip this if you want to keep the raw response for debugging during development.)

## Output contract

On success, the caller receives:

- The file at `output_path` is a valid PNG, size > 0 bytes.
- stdout: `wrote <N> bytes to <output_path>`.
- exit code: `0`.

On failure:

- exit code `1` — `OPENAI_API_KEY` not set.
- exit code `2` — API returned an error object (no `data` field). The error JSON is on stderr.
- exit code `3` — file written but empty or missing.

## Notes

### If `gpt-image-2` returns `model_not_found`

OpenAI's currently-published image model (as of late 2025) is `gpt-image-1`. The project explicitly chose `gpt-image-2` as the literal value. If the API returns:

```json
{"error": {"code": "model_not_found", "message": "..."}}
```

the agreed fallback is to switch the `"model"` field to `"gpt-image-1"`. Update this skill in one place; do not paper over it in the caller.

### Why JSON-escape the prompt with Python

A prompt like `It's a "test"` will break a naïve `-d '{"prompt":"$PROMPT"}'` heredoc — both the apostrophe and the embedded quote corrupt the JSON. `python3 -c 'json.dumps(...)'` is the smallest reliable escape pass that's available wherever Python 3 is installed (which, in this project, is everywhere).

### Why jq + python fallback

`jq` is faster and cleaner for the happy path. But `jq` is not in the default Git Bash on Windows, and we don't want this skill to silently fail there. Python 3 is a hard dependency of the broader stack already, so it's a safe fallback.

### Cost awareness

Each call costs real money on the OpenAI side. The skill makes no attempt to cache or deduplicate. Callers that iterate on prompts should keep the `.txt` sidecar (the `sami-image-gen` agent does this) so they don't regenerate images they've already produced.
