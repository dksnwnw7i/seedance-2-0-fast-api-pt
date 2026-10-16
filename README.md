# Seedance 2.0 Fast API (`doubao-seedance-2.0-fast`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0213 per second** on the APIMart route (billed by resolution tier) — um único endpoint compatível com OpenAI em `https://api.apimart.ai/v1`, sem plano mensal. *(observado em 2026-10-12)*

**[Obter uma chave de API](https://go.apimart.ai/k-c747fe)** · **[Preços ao vivo](https://go.apimart.ai/k-aad987)** · **[Página do modelo](https://go.apimart.ai/k-d1432a)** · [⚡ Início rápido em 60 segundos](#quickstart)

**Por que os times usam Seedance 2.0 Fast (`doubao-seedance-2.0-fast`) via APIMart**

- **Uma chave, o catálogo inteiro.** A mesma base URL `https://api.apimart.ai/v1` e o mesmo cabeçalho `Authorization` alcançam Seedance 2.0 Fast (`doubao-seedance-2.0-fast`) e mais de 300 modelos de imagem, vídeo e linguagem — troque o campo `model`, não o seu cliente.
- **Mínimo de $1, pague conforme o uso.** Sem assinatura e sem plano pré-pago para dimensionar antes: recarregue a partir de $1 e gaste em chamadas.
- **A cobrança volta na resposta.** Toda tarefa concluída informa o valor cobrado (`cost` / `credits_cost`); uma tarefa marcada como `failed` é reembolsada.
- **Assíncrono por design.** Envie, pegue o `task_id`, consulte `GET /v1/tasks/{id}` — batching e retries são trabalho comum de fila, não uma integração sob medida.

<!-- /conv-kit:v1 -->

**Também conhecido como:** Dreamina

Seedance 2.0 Fast is ByteDance's faster Seedance 2.0 route on APIMart: text-to-video and image-to-video with the same framing controls, billed per generated second.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `doubao-seedance-2.0-fast` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-d1432a) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-aad987) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-d1432a) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"doubao-seedance-2.0-fast","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "doubao-seedance-2.0-fast", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Qual é o id do modelo Seedance 2.0 Fast?**

`doubao-seedance-2.0-fast`. Veja a tabela de rotas acima.

**Como é a cobrança?**

**from $0.0213 per second** on the APIMart route (billed by resolution tier). A tarefa concluída informa o valor exato no campo `cost`; uma tarefa com falha é reembolsada.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-c747fe) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [seedance-2-0-fast-prompts-pt](https://github.com/{{OWNER}}/seedance-2-0-fast-prompts-pt)
- **Same model, other languages** — 10 more language builds: English `seedance-2-0-fast-api` · 中文 [seedance-2-0-fast-api-cn](https://github.com/aryvk/seedance-2-0-fast-api-cn) · Deutsch [seedance-2-0-fast-api-de](https://github.com/m7sqz596a/seedance-2-0-fast-api-de) · Español [seedance-2-0-fast-api-es](https://github.com/qaukkgecogrt/seedance-2-0-fast-api-es) · Français [seedance-2-0-fast-api-fr](https://github.com/j-az78q90oa/seedance-2-0-fast-api-fr) · Relay/gateway [seedance-2-0-fast-api-gateway](https://github.com/nmx58j60dc8c8/seedance-2-0-fast-api-gateway) · Bahasa [seedance-2-0-fast-api-id](https://github.com/ulazbd7hzzt/seedance-2-0-fast-api-id) · 日本語 [seedance-2-0-fast-api-ja](https://github.com/sxb6d8v/seedance-2-0-fast-api-ja) · 한국어 [seedance-2-0-fast-api-ko](https://github.com/k9fc4/seedance-2-0-fast-api-ko) · Русский [seedance-2-0-fast-api-ru](https://github.com/wlpm01/seedance-2-0-fast-api-ru)
- **More models in this language** — [midjourney-api-pt](https://github.com/l-ca0/midjourney-api-pt) · [gemini-2-5-flash-image-preview-api-pt](https://github.com/ycx8l0/gemini-2-5-flash-image-preview-api-pt) · [nano-banana-api-pt](https://github.com/erujt71i1lx/nano-banana-api-pt) · [gemini-3-1-flash-image-preview-api-pt](https://github.com/hutjwux0nt9h/gemini-3-1-flash-image-preview-api-pt) · [nano-banana-2-api-pt](https://github.com/ydkd8kdkl8n/nano-banana-2-api-pt) · [gemini-3-pro-image-preview-api-pt](https://github.com/qss2d-66qb2n7/gemini-3-pro-image-preview-api-pt)
- **Topic hubs** — every build of this model, all languages: [seedance-2-0-fast](https://github.com/topics/seedance-2-0-fast) · [seedance-2-0-fast-api](https://github.com/topics/seedance-2-0-fast-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-d1432a> | `apimart.ai/pt/model/doubao-seedance-2-0-fast` |
| Live pricing | <https://go.apimart.ai/k-aad987> | `apimart.ai/pt/pricing` |
| Get an API key | <https://go.apimart.ai/k-c747fe> | `apimart.ai/pt/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedance 2.0 Fast is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
