# Wan 2.7 Image Pro API (`wan2.7-image-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0544 per image** on the APIMart route — 一个 OpenAI 兼容端点 `https://api.apimart.ai/v1`，无需包月套餐。*(数据观测于 2026-10-12)*

**[获取 API Key](https://go.apimart.ai/k-52132f)** · **[实时价格](https://go.apimart.ai/k-67f1e1)** · **[模型页面](https://go.apimart.ai/k-bfe163)** · [⚡ 60 秒快速上手](#quickstart)

**为什么团队通过 APIMart 调用 Wan 2.7 Image Pro（`wan2.7-image-pro`）**

- **一个 Key，整个目录。**同一个 `https://api.apimart.ai/v1` base URL 和 `Authorization` 请求头即可访问 Wan 2.7 Image Pro（`wan2.7-image-pro`）以及图像、视频、语言等 300+ 模型——只需切换 `model` 字段，客户端无需改动。
- **$1 起充，按量付费。**没有订阅、也没有需要预先估算用量的预付费套餐：最低充值 $1，调用多少扣多少。
- **费用随响应一并返回。**每个完成的任务都会报告实际计费金额（`cost` / `credits_cost`）；状态为 `failed` 的任务会自动退款。
- **原生异步设计。**提交任务、拿到 `task_id`、轮询 `GET /v1/tasks/{id}`——批处理和重试都是普通的队列工作，不需要定制集成。

<!-- /conv-kit:v1 -->

**中文环境也叫:** 通义万相

Wan 2.7 Image Pro (wan2.7-image-pro) is Wan's flagship text-to-visual model in Alibaba's Wan lineup. Create stunning outputs with a top-tier Wan engine. The Wan API supports fast Wan 2.7 Image Pro generation, and pricing keeps Wan 2.7 Image Pro generation workflows affordable for creators across the Wan community.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `wan2.7-image-pro` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-bfe163) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.068 | 0.0544 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-67f1e1) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

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

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-bfe163) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"wan2.7-image-pro","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "wan2.7-image-pro", "prompt": "A bamboo forest at sunrise, slow push-in",
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

**Wan 2.7 Image Pro 的模型 id 是什么？**

`wan2.7-image-pro`。见上方路由表。

**如何计费？**

**from $0.0544 per image** on the APIMart route。完成的任务会在 `cost` 字段中给出准确金额；失败的任务会自动退款。

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-52132f) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [wan-2-7-image-pro-prompts-cn](https://github.com/rg1qf80d5au9h/wan-2-7-image-pro-prompts-cn)
- **Same model, other languages** — 10 more language builds: English [wan-2-7-image-pro-api](https://github.com/u3cdzay/wan-2-7-image-pro-api) · Deutsch `wan-2-7-image-pro-api-de` · Español [wan-2-7-image-pro-api-es](https://github.com/pht10ui5daic/wan-2-7-image-pro-api-es) · Français `wan-2-7-image-pro-api-fr` · Relay/gateway `wan-2-7-image-pro-api-gateway` · Bahasa [wan-2-7-image-pro-api-id](https://github.com/g8aowr0uot/wan-2-7-image-pro-api-id) · 日本語 [wan-2-7-image-pro-api-ja](https://github.com/jhz35cfz/wan-2-7-image-pro-api-ja) · 한국어 `wan-2-7-image-pro-api-ko` · Português [wan-2-7-image-pro-api-pt](https://github.com/gvq5ox1pycj/wan-2-7-image-pro-api-pt) · Русский [wan-2-7-image-pro-api-ru](https://github.com/jsgotg/wan-2-7-image-pro-api-ru)
- **More models in this language** — [midjourney-api-cn](https://github.com/apimartycl6k972t/midjourney-api-cn) · [gemini-2-5-flash-image-preview-api-cn](https://github.com/hm-5j/gemini-2-5-flash-image-preview-api-cn) · [nano-banana-api-cn](https://github.com/mpflv/nano-banana-api-cn) · [gemini-3-1-flash-image-preview-api-cn](https://github.com/utp00/gemini-3-1-flash-image-preview-api-cn) · [nano-banana-2-api-cn](https://github.com/i3hxnu6/nano-banana-2-api-cn) · [gemini-3-pro-image-preview-api-cn](https://github.com/vd0tv42d/gemini-3-pro-image-preview-api-cn)
- **Topic hubs** — every build of this model, all languages: [wan-2-7-image-pro](https://github.com/topics/wan-2-7-image-pro) · [wan-2-7-image-pro-api](https://github.com/topics/wan-2-7-image-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-bfe163> | `apimart.ai/zh/model/wan-2-7-image-pro` |
| Live pricing | <https://go.apimart.ai/k-67f1e1> | `apimart.ai/zh/pricing` |
| Get an API key | <https://go.apimart.ai/k-52132f> | `apimart.ai/zh/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Wan 2.7 Image Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
