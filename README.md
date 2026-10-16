# Seedream 4 API (`seedream-4-0`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0195 per image** on the APIMart route — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-58cd19)** · **[Live pricing](https://go.apimart.ai/k-88f8f9)** · **[Model page](https://go.apimart.ai/k-909eb6)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Seedream 4 (`seedream-4-0`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Seedream 4 (`seedream-4-0`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Seedream 4.0 API is ByteDance's unified text-to-image and image editing model. Generate 4K images with industry-leading text rendering, batch processing, and multi-reference support.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `seedream-4-0` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-909eb6) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.024375 | 0.0195 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-88f8f9) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-909eb6) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"seedream-4-0","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "seedream-4-0", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Seedream 4 model id?**

`seedream-4-0`. See the route table above.

**How is it billed?**

**from $0.0195 per image** on the APIMart route. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-58cd19) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, other languages** — 9 more language builds: 中文 [seedream-4-api-cn](https://github.com/n7q72nsidq42w/seedream-4-api-cn) · Deutsch [seedream-4-api-de](https://github.com/yhkrslhp2s019k/seedream-4-api-de) · Español [seedream-4-api-es](https://github.com/yft1f3cb8wp/seedream-4-api-es) · Français [seedream-4-api-fr](https://github.com/t90p6sruvq/seedream-4-api-fr) · Bahasa [seedream-4-api-id](https://github.com/h2zpbl/seedream-4-api-id) · 日本語 `seedream-4-api-ja` · 한국어 [seedream-4-api-ko](https://github.com/ry93x5wv1d7e/seedream-4-api-ko) · Português [seedream-4-api-pt](https://github.com/sr35u/seedream-4-api-pt) · Русский [seedream-4-api-ru](https://github.com/f4oaqsu7whztd/seedream-4-api-ru)
- **More models in this language** — [midjourney-api-gateway](https://github.com/apimartxc3iskw/midjourney-api-gateway) · [gemini-2-5-flash-image-preview-api-gateway](https://github.com/mcqdlebg/gemini-2-5-flash-image-preview-api-gateway) · [nano-banana-api-gateway](https://github.com/gix2kust/nano-banana-api-gateway) · [gemini-3-1-flash-image-preview-api-gateway](https://github.com/nn6jtthao8/gemini-3-1-flash-image-preview-api-gateway) · [nano-banana-2-api-gateway](https://github.com/u9mdw3pt594q2r/nano-banana-2-api-gateway) · [gemini-3-pro-image-preview-api-gateway](https://github.com/uqu9q/gemini-3-pro-image-preview-api-gateway)
- **Topic hubs** — every build of this model, all languages: [seedream-4](https://github.com/topics/seedream-4) · [seedream-4-api](https://github.com/topics/seedream-4-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-909eb6> | `apimart.ai/model/seedream-4` |
| Live pricing | <https://go.apimart.ai/k-88f8f9> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-58cd19> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedream 4 is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
