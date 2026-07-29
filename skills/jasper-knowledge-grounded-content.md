---
name: Generate content grounded in Jasper Knowledge
description: Upload workspace knowledge, retrieve relevant context, and generate on-brand content with it.
api: https://api.jasper.ai/v1
source: https://developers.jasper.ai/reference
operations: [uploadKnowledgeDocument, searchKnowledge, getAllTones, getAllAudiences, command]
---

# Generate content grounded in Jasper Knowledge

Ground Jasper generations in your own workspace facts so output stays accurate and on-brand.

## Auth
- `X-API-KEY: <workspace token>` or OAuth 2.0 bearer. Business plan required.
- Base URL: `https://api.jasper.ai/v1`.

## Steps
1. **Persist knowledge.** `uploadKnowledgeDocument` (`POST /v1/knowledge`) stores durable
   workspace content (unlike temporary attachments). Manage it with `getKnowledgeDocuments`,
   `updateKnowledgeDocument`, `deleteKnowledgeDocument`.
2. **Retrieve.** `searchKnowledge` returns the most relevant knowledge chunks for a query — use
   this to build the context you pass into generation.
3. **Set brand context.** Fetch a brand `tone` with `getAllTones` and an `audience` with
   `getAllAudiences` so the output matches your brand voice and target reader.
4. **Generate.** Call the content `command` endpoint with the retrieved knowledge, tone, and
   audience as context. Optionally augment with the real-time Retrieval Add-Ons.
5. **Errors.** Handle `400/401/429/500` per `errors/jasper-problem-types.yml`; the content POST
   group is limited to 105 rpm and knowledge search shares that group.

## Notes
- Prefer Knowledge (durable) over ad-hoc attachments for anything reused across runs.
- No idempotency key is documented — treat generation calls as non-idempotent and de-dupe
  client-side (`conventions/jasper-conventions.yml`).
