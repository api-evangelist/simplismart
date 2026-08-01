---
name: Run an LLM chat completion on Simplismart
description: Call a Simplismart-hosted open LLM (Llama, Qwen, Gemma, Mixtral, DeepSeek) with an OpenAI-compatible chat request and read its usage metrics.
api: openapi/simplismart-llama-3.3-70b-openapi.yml
operations: [createChatCompletion70B, getChatMetrics]
---

# Run an LLM chat completion

Simplismart serves open LLMs behind an OpenAI-compatible `/chat/completions`
surface. Base URL for shared inference is `https://api.simplismart.live`.

## Steps

1. **Authenticate.** Send `Authorization: Bearer <SIMPLISMART_API_KEY>` on every
   request. Keys are generated under Settings -> API Keys and may carry an expiry
   (a 401 means the key is missing, wrong, or expired).
2. **Create the completion** (`createChatCompletion70B`). POST to
   `/chat/completions` with `model`, `messages` (role/content), and optional
   streaming. The request/response shape matches the OpenAI chat API, so an
   OpenAI SDK pointed at the Simplismart base URL works unchanged.
3. **Read metrics** (`getChatMetrics`). GET `/get/metrics/{request_id}` to fetch
   token counts, processing time, and performance statistics for a completion.

## Rules

- No idempotency key is supported — do not assume safe retries create duplicates
  server-side; retry only on 5xx with backoff.
- Errors return a plain JSON `{ "detail": "..." }` envelope; the Python SDK
  raises `SimplismartError` with `status_code` and `payload`.
- Concurrency is bounded by your organisation's GPU quota, not a request-rate cap.
