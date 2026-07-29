---
name: Edit product images with the Jasper Image API
description: Clean up, remove/replace backgrounds, uncrop, and upscale marketing imagery at scale.
api: https://api.jasper.ai/v1
source: https://developers.jasper.ai/docs/using-images
operations: [removeBackground, replaceBackground, cleanup, removeText, uncrop, upscale, packshotCompositing, altText]
---

# Edit product images with the Jasper Image API

Produce professional marketing imagery — no design tooling required — via Jasper's image
endpoints.

## Auth
- `X-API-KEY: <workspace token>` or OAuth 2.0 bearer. Business plan required.
- Base URL: `https://api.jasper.ai/v1`. Image endpoints accept `multipart/form-data`.

## Common flows
- **Isolate a product:** `removeBackground`, then `replaceBackground` (or `packshotCompositing`
  to place the product on a generated scene).
- **Repair/clean:** `cleanup` (remove artifacts), `removeText` (strip baked-in text).
- **Reframe:** `uncrop` (extend the canvas / outpaint), `upscale` (increase resolution).
- **Accessibility/SEO:** `altText` to generate alt text for an image.

## Guardrails
- Image POST endpoints are rate-limited to **30 rpm per workspace**
  (`rate-limits/jasper-rate-limits.yml`) — throttle batch jobs accordingly.
- Handle `400` (invalid/missing image), `429` (rate limit), and `500` per
  `errors/jasper-problem-types.yml`.
