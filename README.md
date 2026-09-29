# Invoice PDF Generator

A free, dependency-free [Cloudflare Worker](https://workers.cloudflare.com/) that turns invoice JSON into a professional PDF invoice - no templates, no PDF libraries, ~1 KB of hand-rolled PDF writing.

Live API: [Invoice PDF Generator on RapidAPI](https://rapidapi.com/MorrowgridStudio/api/invoice-pdf-generator4) (free tier available)

## Usage

`POST /v1/invoice` with a JSON body:

```json
{
  "invoice_number": "INV-1001",
  "seller": {"name": "Your Company", "email": "billing@yourco.com"},
  "buyer": {"name": "Client Co"},
  "items": [{"description": "Consulting", "qty": 2, "unit_price": 150}],
  "tax_rate": 0.08,
  "currency": "USD"
}
```

The response is the PDF itself (`application/pdf`).

| Field | Required | Notes |
| --- | --- | --- |
| `invoice_number` | no | Shown on the document |
| `seller`, `buyer` | no | `name` plus optional `email` / `address` lines |
| `items` | yes | Array of `{description, qty, unit_price}` |
| `tax_rate` | no | e.g. `0.08` for 8% |
| `currency` | no | Defaults to `USD` |

`GET /health` returns `{"ok": true}`.

If the `PROXY_SECRET` env var is set, requests must carry a matching `X-RapidAPI-Proxy-Secret` header (used to lock the Worker to RapidAPI-originated traffic).

## Deploy your own

1. Create a Cloudflare Worker.
2. Paste `worker.js` as the script - no build step, no dependencies.
3. (Optional) set a `PROXY_SECRET` secret to gate access.

## Sponsor

Maintained by [Morrowgrid Studio](https://github.com/morrowgrid). If this saves you an afternoon, sponsorship keeps the free tier alive.
