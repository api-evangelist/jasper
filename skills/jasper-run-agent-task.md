---
name: Run a Jasper marketing Agent Task
description: Discover a Jasper Agent Task, attach ad-hoc context, and run it to generate on-brand marketing output.
api: https://api.jasper.ai/v1
source: https://developers.jasper.ai/reference
operations: [listTasks, getTaskById, createAttachment, runTask, runTaskStream]
---

# Run a Jasper marketing Agent Task

Use Jasper Agent Tasks to offload a marketing job (blog draft, ad copy, repurposing) to a
Jasper agent that already knows your brand voice, audiences, and style guides.

## Auth
- Send `X-API-KEY: <workspace token>` (Admin/Developer role manages tokens at
  https://app.jasper.ai/settings/dev-tools/tokens), **or** an OAuth 2.0 bearer token.
- API access requires the Jasper **Business** plan.
- Base URL: `https://api.jasper.ai/v1`.

## Steps
1. **Find the task.** `listTasks` (`GET /v1/tasks`) returns public and custom workspace agent
   tasks. Pick the task `id` you want to run, or fetch one with `getTaskById`
   (`GET /v1/tasks/{id}`).
2. **(Optional) Attach context.** For ad-hoc context, call `createAttachment` as
   `multipart/form-data` — the `type` field discriminates the `value` (FILE bytes, or TEXT/URL
   value). Keep the returned attachment `id`; it is only accepted by `runTask` via `attachmentIds`.
   For durable context, upload to the Knowledge API instead.
3. **Run it.** `runTask` (`POST /v1/tasks/{id}/run`) with the required context items,
   configuration, and any `attachmentIds`. Use `runTaskStream` for a Server-Sent Events stream.
4. **Handle results/errors.** `200` returns the generated output. Handle `400` (bad body),
   `404` (task not found), `429` (workspace rate limit — content POST group is 105 rpm), and
   `500` (run failed) per `errors/jasper-problem-types.yml`.

## Notes
- Attachments are short-lived; do not rely on them for durable storage.
- Rate limits are per workspace — see `rate-limits/jasper-rate-limits.yml`.
