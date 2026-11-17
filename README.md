# MiniMax H3 Image API: cURL, Python, and Node.js Examples

[![MiniMax H3 Image on PoYo](https://img.shields.io/badge/Try%20on%20PoYo-minimax--h3--image-84cc16)](https://poyo.ai/models/minimax-h3-image?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api) [![API docs](https://img.shields.io/badge/API-docs-22d3ee)](https://docs.poyo.ai/api-manual/image-series/minimax-h3-image?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api) [![Check](https://github.com/PoyoAPI/MiniMax-H3-Image-API/actions/workflows/check.yml/badge.svg)](https://github.com/PoyoAPI/MiniMax-H3-Image-API/actions/workflows/check.yml)

Runnable examples for **MiniMax H3 Image** (MiniMax-H3-Image) through PoYo's async image API: text-to-image and editing with up to 16 reference images, 1K, 2K, or 4K output, exact custom sizes up to 20 megapixels, and on-image text.

**Answer:** To use the MiniMax H3 Image API, send a server-side request to `POST https://api.poyo.ai/api/generate/submit` with model `minimax-h3-image` and a PoYo API key, then poll `GET https://api.poyo.ai/api/generate/status/{task_id}`. The examples in this repository run with Python 3.9+, Node.js 22+, or cURL.

Last verified against the live PoYo schema and pricing on **November 17, 2026**.

## MiniMax H3 Image at a glance

- **Model ID:** `minimax-h3-image` (still images). For MiniMax H3 video, use model ID `hailuo-03` in [MiniMax-H3-API](https://github.com/PoyoAPI/MiniMax-H3-API).
- **Released:** November 10, 2026 by MiniMax, as an open-weight model
- **Modes:** text-to-image, or editing with 1–16 reference images in the same model ID
- **Output:** 10 aspect ratios at 1K, 2K (default), or 4K (up to 6272×2688 at 21:9), or a custom `WIDTHxHEIGHT` from 655,360 to 20,000,000 pixels; PNG or JPEG
- **Text:** put exact on-image text in quotes and say where it goes
- **Price:** from 3.2 credits ($0.016) per image at 1K or 2K, 11.2 credits ($0.056) at 4K, plus 0.5 credits ($0.0025) per reference image

## Quickstart

1. Create a key in the [PoYo dashboard](https://poyo.ai/dashboard/api-key?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api).
2. Copy `.env.example` to `.env` and replace the placeholder.
3. Run one of the examples. Each submits the task and polls until `finished` or `failed`.

```dotenv
POYO_API_KEY=YOUR_POYO_API_KEY_HERE
POYO_BASE_URL=https://api.poyo.ai
```

### Python

```bash
python python/generate.py
python python/generate.py payloads/minimax-h3-image-edit.json
```

[`python/generate.py`](python/generate.py) uses only the Python standard library.

### Node.js

```bash
node node/index.mjs
node node/index.mjs payloads/minimax-h3-image-custom-size.json
```

### cURL

```bash
curl -X POST "https://api.poyo.ai/api/generate/submit" \
  -H "Authorization: Bearer $POYO_API_KEY" \
  -H "Content-Type: application/json" \
  -d @payloads/minimax-h3-image-text-to-image.json
```

Status polling and the other modes are in [curl/generate.md](curl/generate.md).

## Example requests

Every file in [`payloads/`](payloads) is a complete request body with public input media, so it runs as-is.

| File | Mode | Settings | Credits |
| --- | --- | --- | --- |
| [`minimax-h3-image-text-to-image.json`](payloads/minimax-h3-image-text-to-image.json) | Poster with on-image text | 16:9, 2K (2720×1536) | 3.2 ($0.016) |
| [`minimax-h3-image-custom-size.json`](payloads/minimax-h3-image-custom-size.json) | Custom size, no prompt expansion | 1920×1080, JPEG | 3.2 ($0.016) |
| [`minimax-h3-image-4k.json`](payloads/minimax-h3-image-4k.json) | 4K text-to-image | 1:1, 4K (4096×4096) | 11.2 ($0.056) |
| [`minimax-h3-image-edit.json`](payloads/minimax-h3-image-edit.json) | Edit with `<Picture 1>` + `<Picture 2>` | 2K, `size` auto | 4.2 ($0.021) |

## Input modes

- **Text-to-image:** omit `image_urls`. `size` defaults to `1:1`, and `enable_prompt_expansion` (default `true`) rewrites a short prompt into a detailed scene description.
- **Image editing:** pass 1–16 URLs in `image_urls`. The first URL is `<Picture 1>`, the second `<Picture 2>`, and so on; say which image to change and which images to take content from. `size` defaults to `auto`, which follows the aspect ratio of `<Picture 1>`. Editing always expands the prompt.

## Request fields

| Field | Requirement | Meaning |
| --- | --- | --- |
| prompt | required | 1–5,000 characters with non-whitespace text. Quote exact on-image text and say where it goes. |
| image_urls | optional | 1–16 public JPEG, PNG, or WebP URLs, up to 20 MB each, 128–8192 pixels per side. Selects editing. Always an array. |
| size | optional | `1:1`, `5:4`, `4:5`, `4:3`, `3:4`, `3:2`, `2:3`, `16:9`, `9:16`, `21:9`, `auto` (editing), or custom `WIDTHxHEIGHT`. |
| resolution | optional | `1K`, `2K` (default), or `4K`, for aspect-ratio sizes and `auto`. |
| enable_prompt_expansion | optional | Default `true`; applies to text-to-image. |
| output_format | optional | `png` (default) or `jpeg`. |

A custom `WIDTHxHEIGHT` uses integers with 655,360 to 20,000,000 total pixels and an aspect ratio between 1:3 and 3:1; the output has exactly that size.

## Pricing

Billing is per image. 1 credit = $0.005.

| Output | Price |
| --- | --- |
| 1K or 2K, or custom size up to 4,500,000 pixels | 3.2 credits ($0.016) per image |
| 4K, or custom size above 4,500,000 pixels | 11.2 credits ($0.056) per image |
| Each image in `image_urls` | 0.5 credits ($0.0025) |

A 2K edit with three reference images costs 3.2 + 3 × 0.5 = 4.7 credits. Credits are charged only when the image is generated. Confirm current prices on the [model page](https://poyo.ai/models/minimax-h3-image?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api).

## Writing MiniMax H3 Image prompts

- **On-image text:** quote every word, say where it goes and how lines break, and add "no other text" when the rest of the design should stay clean.
- **Reference roles:** use `<Picture N>` tags and give each image one job, for example "put the sofa from `<Picture 2>` into the room in `<Picture 1>`".
- **Long prompts:** set `enable_prompt_expansion` to `false` when a carefully written text-to-image prompt should be followed as written.
- **Exact pixels:** use a custom `WIDTHxHEIGHT` for banners and slides; stay at or below 4,500,000 pixels for the lower price.

## Async production flow

```text
submit -> persist data.task_id -> poll during development -> receive callback_url webhook in production
```

Treat `finished` and `failed` as terminal states. Copy output images to your own storage after the task finishes.

## FAQ

**How much does the MiniMax H3 Image API cost?** 3.2 credits ($0.016) per image at 1K or 2K and 11.2 credits ($0.056) at 4K, plus 0.5 credits per reference image.

**Is MiniMax H3 Image the same as MiniMax H3?** No. MiniMax H3 is MiniMax's video model (`hailuo-03` on PoYo); MiniMax H3 Image is the separate still-image model (`minimax-h3-image`).

**How many reference images can I use?** 1 to 16, referenced in the prompt as `<Picture 1>`, `<Picture 2>`, and so on.

**Does it support 4K and custom sizes?** Yes. Set `resolution` to `4K`, or pass an exact `WIDTHxHEIGHT` from 655,360 to 20,000,000 pixels.

**How long does a request take?** In PoYo tests in November 2026, a 1K or 2K image took about 12 seconds, a 4K image about 22 to 25 seconds, and an edit with three reference images about 44 seconds. Times vary with load and settings.

## Repository map

- [`payloads/`](payloads) — complete request bodies for each mode
- [`python/generate.py`](python/generate.py) — standard-library Python example
- [`node/index.mjs`](node/index.mjs) — dependency-free Node.js example
- [`curl/generate.md`](curl/generate.md) — copy-paste cURL requests
- [`docs/production-notes.md`](docs/production-notes.md) — limits and rollout checklist

## Related repositories

- [MiniMax-H3-API](https://github.com/PoyoAPI/MiniMax-H3-API) — MiniMax H3 (Hailuo 03) video
- [Seedream-5.5-API](https://github.com/PoyoAPI/Seedream-5.5-API) — Seedream 5.5 with transparent PNGs and layers
- [awesome-poyo-api](https://github.com/PoyoAPI/awesome-poyo-api) — every PoYo model repository in one list
- [poyo-devtools](https://github.com/PoyoAPI/poyo-devtools) — PoYo CLI, MCP bridge, and Agent Skill

## Links

- [MiniMax H3 Image API on PoYo](https://poyo.ai/models/minimax-h3-image?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api)
- [MiniMax H3 Image API reference](https://docs.poyo.ai/api-manual/image-series/minimax-h3-image?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api)
- [PoYo model catalog](https://poyo.ai/models?utm_source=github&utm_medium=readme&utm_campaign=minimax-h3-image-api)

## License

MIT
