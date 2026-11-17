# MiniMax H3 Image API: cURL quickstart

```bash
export POYO_API_KEY="your-api-key"
export POYO_BASE_URL="https://api.poyo.ai"
```

## Text-to-image with on-image text

```bash
curl -X POST "$POYO_BASE_URL/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "model": "minimax-h3-image",
  "input": {
    "prompt": "A vintage travel poster for Kyoto, headline text '\''KYOTO'\'' in large bold serif letters and subtitle '\''City of a Thousand Gates'\'' below, red torii gates receding up a mossy hill, autumn maple leaves, 1960s screen-print style",
    "size": "16:9",
    "resolution": "2K"
  }
}'
```

## Custom size

```bash
curl -X POST "$POYO_BASE_URL/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d @payloads/minimax-h3-image-custom-size.json
```

A custom `WIDTHxHEIGHT` returns exactly that size. Up to 4,500,000 pixels uses the 1K/2K price.

## 4K

```bash
curl -X POST "$POYO_BASE_URL/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d @payloads/minimax-h3-image-4k.json
```

## Edit with reference images

```bash
curl -X POST "$POYO_BASE_URL/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d @payloads/minimax-h3-image-edit.json
```

The first URL is `<Picture 1>`, the second `<Picture 2>`. Each reference image adds 0.5 credits.

## Check status

Save `data.task_id` from the submit response, then poll:

```bash
curl "$POYO_BASE_URL/api/generate/status/<task_id>" \
  -H "Authorization: Bearer $POYO_API_KEY"
```

For production, add a public HTTPS `callback_url` to the top-level request and make the receiver idempotent.

See the [full README](../README.md) and [production notes](../docs/production-notes.md).
