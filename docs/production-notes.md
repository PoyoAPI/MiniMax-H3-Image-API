# MiniMax H3 Image API production notes

## Confirmed contract

- Endpoint: `POST /api/generate/submit`
- Model ID: `minimax-h3-image`
- Authentication: `Authorization: Bearer <POYO_API_KEY>`
- Status: `GET /api/generate/status/{task_id}`

## Input limits

| Field | Requirement | Constraint |
| --- | --- | --- |
| prompt | required | 1–5,000 characters; non-whitespace text required. |
| image_urls | optional | 1–16 public JPEG, PNG, or WebP URLs, up to 20 MB each, 128–8192 px per side; array even for one image. |
| size | optional | 1:1, 5:4, 4:5, 4:3, 3:4, 3:2, 2:3, 16:9, 9:16, 21:9, auto (editing), or WIDTHxHEIGHT (655,360–20,000,000 px, ratio 1:3 to 3:1). |
| resolution | optional | 1K, 2K (default), or 4K. |
| enable_prompt_expansion | optional | Default true; text-to-image only. Editing always expands. |
| output_format | optional | png (default) or jpeg. |

## Operational checklist

- Keep API keys in a server-side secret manager.
- Validate user input before calling the API.
- Persist `data.task_id` immediately after submission.
- Use five-second polling only for development; prefer `callback_url` in production.
- Make webhook processing idempotent and reconcile status before fulfilling an order.
- Copy output files to durable storage after the task finishes.
- Recheck the live model page before hard-coding prices.

## Failure handling

Treat non-2xx HTTP responses and non-success API `code` values as errors. Preserve the HTTP status, API code, and a redacted response body for debugging.

Schema: [PoYo API documentation](https://docs.poyo.ai/api-manual/image-series/minimax-h3-image?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api)
