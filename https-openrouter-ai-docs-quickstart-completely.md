# OpenRouter Quickstart — Complete Deep-Dive Reference

> **Research conducted:** 2026-05-31 | **Sources:** 9+ pages from openrouter.ai/docs, live API, GitHub repos  
> **Official docs index:** https://openrouter.ai/docs/llms.txt  
> **OpenAPI spec:** https://openrouter.ai/openapi.yaml  

---

## Executive Summary

OpenRouter is a **unified LLM gateway** that provides access to 300+ AI models from dozens of providers through a single OpenAI-compatible endpoint (`https://openrouter.ai/api/v1`). It automatically handles provider fallbacks, load balancing, and cost optimization. Integration is available via three approaches: raw HTTP API, official Client SDKs (`@openrouter/sdk` / `pip install openrouter`), and an Agent SDK (`@openrouter/agent`) for multi-turn agentic workflows. The platform uses a credit-based billing system, supports BYOK (Bring Your Own Key), and is a GitHub secret scanning partner for leaked key detection. Free model variants (`:free` suffix) are available for zero-cost experimentation.

---

## Table of Contents

1. [Getting Started — 3 Ways to Integrate](#1-getting-started)
2. [Authentication & API Keys](#2-authentication--api-keys)
3. [OAuth PKCE Flow](#3-oauth-pkce-flow)
4. [Management API Keys](#4-management-api-keys)
5. [Bring Your Own Key (BYOK)](#5-bring-your-own-key-byok)
6. [Complete Request Schema](#6-complete-request-schema)
7. [Complete Response Schema](#7-complete-response-schema)
8. [Sampling Parameters Reference](#8-sampling-parameters-reference)
9. [Streaming](#9-streaming)
10. [Error Handling](#10-error-handling)
11. [Rate Limits](#11-rate-limits)
12. [Provider Routing](#12-provider-routing)
13. [Model Variants & Slug Conventions](#13-model-variants--slug-conventions)
14. [Latest Model Resolution (`~`)](#14-latest-model-resolution-)
15. [Model Fallbacks](#15-model-fallbacks)
16. [Auto Router](#16-auto-router)
17. [Free Models](#17-free-models)
18. [Multimodal — Images](#18-multimodal--images)
19. [Multimodal — PDFs](#19-multimodal--pdfs)
20. [Tool Calling & Agent SDK](#20-tool-calling--agent-sdk)
21. [Web Search Server Tool](#21-web-search-server-tool)
22. [Plugins](#22-plugins)
23. [App Attribution](#23-app-attribution)
24. [Credits & Billing](#24-credits--billing)
25. [Privacy & Data Logging](#25-privacy--data-logging)
26. [Key Repositories & Resources](#26-key-repositories--resources)
27. [Architecture Diagram](#27-architecture-diagram)
28. [Confidence Assessment](#28-confidence-assessment)
29. [Footnotes](#footnotes)

---

## 1. Getting Started

OpenRouter offers **three integration approaches**[^1]:

| Approach | Best for |
|---|---|
| **API** | Full control, any language, no dependencies |
| **Client SDKs** | Type-safe model calls with minimal overhead |
| **Agent SDK** | Building agents with tool use, loops, and state |

The base endpoint is `https://openrouter.ai/api/v1` — compatible with the OpenAI API spec.[^2]

---

### Approach 1 — Raw HTTP API

No dependencies; any language. Use the interactive [Request Builder](https://openrouter.ai/request-builder) to generate code in your language.

#### Python (requests)[^1]

```python
import requests
import json

response = requests.post(
  url="https://openrouter.ai/api/v1/chat/completions",
  headers={
    "Authorization": "Bearer <OPENROUTER_API_KEY>",
    "HTTP-Referer": "<YOUR_SITE_URL>",      # Optional. Site URL for rankings on openrouter.ai.
    "X-OpenRouter-Title": "<YOUR_SITE_NAME>", # Optional. Site title for rankings on openrouter.ai.
  },
  data=json.dumps({
    "model": "~openai/gpt-latest",
    "messages": [
      {"role": "user", "content": "What is the meaning of life?"}
    ]
  })
)
```

#### TypeScript (fetch)[^1]

```typescript
fetch('https://openrouter.ai/api/v1/chat/completions', {
  method: 'POST',
  headers: {
    Authorization: 'Bearer <OPENROUTER_API_KEY>',
    'HTTP-Referer': '<YOUR_SITE_URL>',         // Optional
    'X-OpenRouter-Title': '<YOUR_SITE_NAME>',  // Optional
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    model: '~openai/gpt-latest',
    messages: [
      { role: 'user', content: 'What is the meaning of life?' },
    ],
  }),
});
```

#### Shell (cURL)[^1]

```bash
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -d '{
  "model": "~openai/gpt-latest",
  "messages": [
    {"role": "user", "content": "What is the meaning of life?"}
  ]
}'
```

---

### Approach 2 — Client SDKs (`@openrouter/sdk` / `openrouter`)

Auto-generated from the OpenAPI spec via Speakeasy. Full type safety, zero boilerplate.[^1]

#### Installation

```bash
npm install @openrouter/sdk   # TypeScript / Node.js
pip install openrouter         # Python
```

#### TypeScript (Client SDK)[^1]

```typescript
import { OpenRouter } from '@openrouter/sdk';

const client = new OpenRouter({
  apiKey: '<OPENROUTER_API_KEY>',
  httpReferer: '<YOUR_SITE_URL>',  // Optional
  appTitle: '<YOUR_SITE_NAME>',    // Optional
});

const completion = await client.chat.send({
  model: '~openai/gpt-latest',
  messages: [
    { role: 'user', content: 'What is the meaning of life?' },
  ],
});

console.log(completion.choices[0].message.content);
```

#### Python (Client SDK)[^1]

```python
from openrouter import OpenRouter
import os

with OpenRouter(api_key=os.getenv("OPENROUTER_API_KEY")) as client:
    response = client.chat.send(
        model="~openai/gpt-latest",
        messages=[
            {"role": "user", "content": "What is the meaning of life?"}
        ],
    )
    print(response.choices[0].message.content)
```

---

### Approach 3 — OpenAI SDK as Drop-in Replacement

For existing codebases using the OpenAI SDK — no structural changes required.[^1]

#### TypeScript (OpenAI SDK)[^1]

```typescript
import OpenAI from 'openai';

const openai = new OpenAI({
  baseURL: 'https://openrouter.ai/api/v1',
  apiKey: '<OPENROUTER_API_KEY>',
  defaultHeaders: {
    'HTTP-Referer': '<YOUR_SITE_URL>',
    'X-OpenRouter-Title': '<YOUR_SITE_NAME>',
  },
});

async function main() {
  const completion = await openai.chat.completions.create({
    model: '~openai/gpt-latest',
    messages: [{ role: 'user', content: 'What is the meaning of life?' }],
  });
  console.log(completion.choices[0].message);
}
main();
```

#### Python (OpenAI SDK)[^1]

```python
from openai import OpenAI

client = OpenAI(
  base_url="https://openrouter.ai/api/v1",
  api_key="<OPENROUTER_API_KEY>",
)

completion = client.chat.completions.create(
  extra_headers={
    "HTTP-Referer": "<YOUR_SITE_URL>",
    "X-OpenRouter-Title": "<YOUR_SITE_NAME>",
  },
  model="~openai/gpt-latest",
  messages=[{"role": "user", "content": "What is the meaning of life?"}]
)
print(completion.choices[0].message.content)
```

---

## 2. Authentication & API Keys

OpenRouter uses **three** authentication methods[^3]:

1. **Cookie-based authentication** — web interface and chatroom
2. **API keys (Bearer tokens)** — completions API and core endpoints  
3. **Management API keys** — programmatic key management via `/api/v1/keys`

### Creating an API Key

> Visit https://openrouter.ai/keys — give it a name and optionally set a credit limit.[^3]

### Required Header[^3]

```
Authorization: Bearer <OPENROUTER_API_KEY>
```

### Attribution Headers (Optional)[^3]

| Header | Required | Description |
|---|---|---|
| `Authorization` | ✅ Yes | `Bearer <OPENROUTER_API_KEY>` |
| `Content-Type` | ✅ For POST | `application/json` |
| `HTTP-Referer` | Optional | Site URL — required for leaderboard rankings |
| `X-OpenRouter-Title` | Optional | App name for rankings (`X-Title` also accepted) |
| `X-OpenRouter-Categories` | Optional | Marketplace categories (comma-separated) |

### Security Best Practices[^3]

> OpenRouter is a **GitHub secret scanning partner**. If a key is compromised, you'll receive an email notification. Immediately visit https://openrouter.ai/settings/keys to delete and recreate.
>
> Never commit keys to public repositories. Use environment variables.

---

## 3. OAuth PKCE Flow

Users can connect to OpenRouter in one click using [PKCE](https://oauth.net/2/pkce/).[^4]

### Step 1: Redirect User to OpenRouter[^4]

```
https://openrouter.ai/auth?callback_url=<YOUR_SITE_URL>&code_challenge=<CODE_CHALLENGE>&code_challenge_method=S256
```

**Generate a Code Challenge (TypeScript):**[^4]

```typescript
import { Buffer } from 'buffer';

async function createSHA256CodeChallenge(input: string) {
  const encoder = new TextEncoder();
  const data = encoder.encode(input);
  const hash = await crypto.subtle.digest('SHA-256', data);
  return Buffer.from(hash).toString('base64url');
}

const codeVerifier = 'your-random-string';
const codeChallenge = await createSHA256CodeChallenge(codeVerifier);
```

### Step 2: Exchange Code for API Key[^4]

```typescript
// Extract code from query params
const code = new URLSearchParams(window.location.search).get('code');

// Exchange for key
const response = await fetch('https://openrouter.ai/api/v1/auth/keys', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    code,
    code_verifier: '<CODE_VERIFIER>',
    code_challenge_method: 'S256',
  }),
});

const { key } = await response.json();
```

### Step 3: Use the Key[^4]

```typescript
import { OpenRouter } from '@openrouter/sdk';

const client = new OpenRouter({ apiKey: key });
const completion = await client.chat.send({
  model: 'openai/gpt-5.2',
  messages: [{ role: 'user', content: 'Hello!' }],
  stream: false,
});
```

### OAuth Error Codes[^4]

| Code | Meaning |
|---|---|
| `400 Invalid code_challenge_method` | Use same method in steps 1 & 2 |
| `403 Invalid code or code_verifier` | Ensure user is logged in; verify code_verifier |
| `405 Method Not Allowed` | Use `POST` with `HTTPS` |

---

## 4. Management API Keys

For programmatic key creation and rotation in SaaS applications.[^4]

> Create a Management key at https://openrouter.ai/settings/management-keys — these **cannot** make LLM API calls; only administrative operations.

### All Management Endpoints (`/api/v1/keys`)[^4]

```typescript
import { OpenRouter } from '@openrouter/sdk';

const mgmt = new OpenRouter({ apiKey: 'your-management-key' });

// List keys (paginated, 100 per page)
const keys = await mgmt.apiKeys.list();
const page2 = await mgmt.apiKeys.list({ offset: 100 });

// Create a key
const newKey = await mgmt.apiKeys.create({
  name: 'Customer Instance Key',
  limit: 1000, // Optional credit limit in USD
});

// Get a specific key by hash
const key = await mgmt.apiKeys.get('<YOUR_KEY_HASH>');

// Update key (name, disabled status, limit reset)
const updated = await mgmt.apiKeys.update('<HASH>', {
  name: 'Updated Key Name',
  disabled: true,
  includeByokInLimit: false,
  limitReset: 'daily',   // 'daily' | 'weekly' | 'monthly'
});

// Delete a key
await mgmt.apiKeys.delete('<HASH>');
```

### Key Response Object[^4]

```json
{
  "data": [{
    "hash": "<KEY_HASH>",
    "label": "sk-or-v1-abc...123",
    "name": "Customer Key",
    "disabled": false,
    "limit": 10,
    "limit_remaining": 10,
    "limit_reset": null,
    "usage": 0,
    "usage_daily": 0,
    "usage_weekly": 0,
    "usage_monthly": 0
  }]
}
```

---

## 5. Bring Your Own Key (BYOK)

Use your own provider keys with OpenRouter. Keys are securely encrypted.[^4]

> Manage keys at https://openrouter.ai/workspaces/default/byok  
> Cost: a **percentage of normal OpenRouter price**, deducted from your credits. First N BYOK requests per month are free.

### Supported Providers

**AWS Bedrock:**[^4]
```json
{
  "accessKeyId": "your-aws-access-key-id",
  "secretAccessKey": "your-aws-secret-access-key",
  "region": "your-aws-region"
}
```

Required IAM permissions: `bedrock:InvokeModel` and `bedrock:InvokeModelWithResponseStream`.

**Azure AI Foundry:**[^4]
```json
[{
  "api_key": "your-azure-api-key",
  "resource_name": "your-resource-name",
  "resource_type": "ai_foundry"
}]
```

**Google Vertex AI:**[^4]
```json
{
  "type": "service_account",
  "project_id": "your-project-id",
  "private_key": "-----BEGIN PRIVATE KEY-----\n...",
  "client_email": "your-service-account@your-project.iam.gserviceaccount.com",
  "region": "global"
}
```

---

## 6. Complete Request Schema

`POST https://openrouter.ai/api/v1/chat/completions`[^2]

```typescript
type Request = {
  // Either "messages" or "prompt" is required
  messages?: Message[];
  prompt?: string;

  // Model selection — if unspecified, uses the user's default
  model?: string;

  // Structured output format
  response_format?: ResponseFormat;

  stop?: string | string[];
  stream?: boolean;

  // Plugins (PDF parsing, web search, response healing)
  plugins?: Plugin[];

  // Sampling parameters
  max_tokens?: number;          // Range: [1, context_length)
  temperature?: number;         // Range: [0, 2]
  top_p?: number;               // Range: (0, 1]
  top_k?: number;               // Range: [1, Infinity)
  frequency_penalty?: number;   // Range: [-2, 2]
  presence_penalty?: number;    // Range: [-2, 2]
  repetition_penalty?: number;  // Range: (0, 2]
  logit_bias?: { [key: number]: number };
  top_logprobs?: number;        // Integer only
  min_p?: number;               // Range: [0, 1]
  top_a?: number;               // Range: [0, 1]
  seed?: number;
  
  // Tool calling (passed to providers that support it)
  tools?: Tool[];
  tool_choice?: ToolChoice;     // 'none' | 'auto' | 'required' | { type, function }
  parallel_tool_calls?: boolean; // Default: true

  // Reasoning / thinking tokens
  reasoning?: object;
  reasoning_effort?: 'xhigh' | 'high' | 'medium' | 'low' | 'minimal' | 'none';
  include_reasoning?: boolean;  // Deprecated alias

  // Predicted output (latency optimization)
  prediction?: { type: 'content'; content: string };

  // OpenRouter-specific parameters
  models?: string[];            // Fallback models array
  route?: 'fallback';
  provider?: ProviderPreferences;
  user?: string;                // Stable identifier for end-users

  // Debug (streaming only)
  debug?: { echo_upstream_body?: boolean };
};
```

### Message Types[^2]

```typescript
type Message =
  | {
      role: 'user' | 'assistant' | 'system';
      content: string | ContentPart[];  // ContentParts only for 'user' role
      name?: string;  // Prepended as "{name}: {content}" for non-OpenAI models
    }
  | {
      role: 'tool';
      content: string;
      tool_call_id: string;
      name?: string;
    };

type TextContent = { type: 'text'; text: string };
type ImageContentPart = {
  type: 'image_url';
  image_url: { url: string; detail?: string };  // URL or base64
};
type ContentPart = TextContent | ImageContentPart;
```

### Tool Definition[^2]

```typescript
type Tool = {
  type: 'function';
  function: {
    description?: string;
    name: string;
    parameters: object; // JSON Schema
  };
};
```

### Response Format for Structured Outputs[^2]

```typescript
type ResponseFormat =
  | { type: 'json_object' }
  | {
      type: 'json_schema';
      json_schema: {
        name: string;
        strict?: boolean;
        schema: object; // JSON Schema
      };
    };
```

### Plugin Configuration[^2]

```typescript
type Plugin = {
  id: 'web' | 'file-parser' | 'response-healing' | 'context-compression';
  enabled?: boolean;
  [key: string]: unknown; // Plugin-specific options
};
```

---

## 7. Complete Response Schema

```typescript
type Response = {
  id: string;                    // e.g. "gen-xxxxxxxxxxxxxx"
  choices: (NonStreamingChoice | StreamingChoice | NonChatChoice)[];
  created: number;               // Unix timestamp
  model: string;                 // The actual model that served the request
  object: 'chat.completion' | 'chat.completion.chunk';
  system_fingerprint?: string;
  usage?: ResponseUsage;
};

type ResponseUsage = {
  prompt_tokens: number;
  completion_tokens: number;
  total_tokens: number;
  prompt_tokens_details?: {
    cached_tokens: number;
    cache_write_tokens?: number;
    audio_tokens?: number;
    video_tokens?: number;
  };
  completion_tokens_details?: {
    reasoning_tokens?: number;
    audio_tokens?: number;
    image_tokens?: number;
  };
  cost?: number;              // Cost in credits (USD)
  is_byok?: boolean;
  server_tool_use?: { web_search_requests?: number };
};

type NonStreamingChoice = {
  finish_reason: 'tool_calls' | 'stop' | 'length' | 'content_filter' | 'error' | null;
  native_finish_reason: string | null;  // Raw provider finish reason
  message: {
    content: string | null;
    role: string;
    tool_calls?: ToolCall[];
  };
  error?: ErrorResponse;
};

type StreamingChoice = {
  finish_reason: string | null;
  native_finish_reason: string | null;
  delta: { content: string | null; role?: string; tool_calls?: ToolCall[] };
  error?: ErrorResponse;
};
```

### Example Response JSON[^2]

```json
{
  "id": "gen-xxxxxxxxxxxxxx",
  "choices": [{
    "finish_reason": "stop",
    "native_finish_reason": "stop",
    "message": {
      "role": "assistant",
      "content": "Hello there!"
    }
  }],
  "usage": {
    "prompt_tokens": 10,
    "completion_tokens": 4,
    "total_tokens": 14,
    "prompt_tokens_details": { "cached_tokens": 0 },
    "completion_tokens_details": { "reasoning_tokens": 0 },
    "cost": 0.00014
  },
  "model": "openai/gpt-4o"
}
```

> **Generation ID:** The `X-Generation-Id` response header is returned for all endpoints and can be used for debugging. Retrieve per-generation stats via `GET /api/v1/generation?id=$GENERATION_ID`.[^2]

---

## 8. Sampling Parameters Reference

| Parameter | Type | Range | Default | Notes |
|---|---|---|---|---|
| `temperature` | float | [0, 2] | 1.0 | Lower = more predictable |
| `top_p` | float | (0, 1] | 1.0 | Nucleus sampling |
| `top_k` | integer | [1, ∞) | 0 (disabled) | Not available for OpenAI models |
| `frequency_penalty` | float | [-2, 2] | 0.0 | Scales with token frequency |
| `presence_penalty` | float | [-2, 2] | 0.0 | Does NOT scale with frequency |
| `repetition_penalty` | float | [0, 2] | 1.0 | Alternative repeat penalty |
| `min_p` | float | [0, 1] | 0.0 | Min probability relative to top token |
| `top_a` | float | [0, 1] | 0.0 | Dynamic top filter |
| `seed` | integer | — | — | Deterministic sampling (not guaranteed all models) |
| `max_tokens` | integer | [1, ∞) | — | Max output tokens (`max_completion_tokens` alias) |
| `logit_bias` | map | tokens → [-100, 100] | — | Token probability adjustments |
| `logprobs` | boolean | — | — | Return log probabilities |
| `top_logprobs` | integer | [0, 20] | — | Requires `logprobs: true` |
| `stop` | string\|string[] | — | — | Stop sequences |
| `reasoning_effort` | enum | — | — | `xhigh`/`high`/`medium`/`low`/`minimal`/`none` |
| `verbosity` | enum | — | `medium` | For Anthropic: maps to `output_config.effort` |

[^2]

---

## 9. Streaming

Set `stream: true` to receive Server-Sent Events (SSE).[^5]

### TypeScript SDK (Streaming)[^5]

```typescript
import { OpenRouter } from '@openrouter/sdk';

const openRouter = new OpenRouter({ apiKey: 'YOUR_KEY' });

const stream = await openRouter.chat.send({
  model: 'openai/gpt-4o',
  messages: [{ role: 'user', content: 'How would you build the tallest building ever?' }],
  stream: true,
});

for await (const chunk of stream) {
  const content = chunk.choices?.[0]?.delta?.content;
  if (content) process.stdout.write(content);

  // Usage stats arrive in the final chunk
  if (chunk.usage) console.log('Usage:', chunk.usage);
}
```

### Python (Streaming with SSE parsing)[^5]

```python
import requests, json

url = "https://openrouter.ai/api/v1/chat/completions"
headers = {
  "Authorization": "Bearer <KEY>",
  "Content-Type": "application/json"
}
payload = {
  "model": "openai/gpt-4o",
  "messages": [{"role": "user", "content": "Write a story"}],
  "stream": True
}

buffer = ""
with requests.post(url, headers=headers, json=payload, stream=True) as r:
  for chunk in r.iter_content(chunk_size=1024, decode_unicode=True):
    buffer += chunk
    while True:
      line_end = buffer.find('\n')
      if line_end == -1: break
      line = buffer[:line_end].strip()
      buffer = buffer[line_end + 1:]
      if line.startswith('data: '):
        data = line[6:]
        if data == '[DONE]': break
        try:
          obj = json.loads(data)
          content = obj["choices"][0]["delta"].get("content")
          if content: print(content, end="", flush=True)
        except json.JSONDecodeError: pass
```

### TypeScript (Raw fetch Streaming)[^5]

```typescript
const response = await fetch('https://openrouter.ai/api/v1/chat/completions', {
  method: 'POST',
  headers: { Authorization: `Bearer ${KEY}`, 'Content-Type': 'application/json' },
  body: JSON.stringify({ model: 'openai/gpt-4o', messages: [...], stream: true }),
});

const reader = response.body?.getReader();
const decoder = new TextDecoder();
let buffer = '';

while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  buffer += decoder.decode(value, { stream: true });
  while (true) {
    const lineEnd = buffer.indexOf('\n');
    if (lineEnd === -1) break;
    const line = buffer.slice(0, lineEnd).trim();
    buffer = buffer.slice(lineEnd + 1);
    if (line.startsWith('data: ')) {
      const data = line.slice(6);
      if (data === '[DONE]') break;
      const parsed = JSON.parse(data);
      const content = parsed.choices[0].delta.content;
      if (content) console.log(content);
    }
  }
}
```

### SSE Keep-Alive Comments[^5]

OpenRouter occasionally sends SSE comments to prevent timeouts:
```
: OPENROUTER PROCESSING
```
These are safe to ignore per the SSE spec.

### Stream Cancellation[^5]

```typescript
const controller = new AbortController();
const stream = await openRouter.chat.send(
  { model: '...', messages: [...], stream: true },
  { signal: controller.signal }
);
// Cancel:
controller.abort();
```

**Supported providers for cancellation:** OpenAI, Azure, Anthropic, Fireworks, DeepSeek, Cloudflare, and many more.  
**Not supported:** AWS Bedrock, Groq, Google, Mistral, HuggingFace, Replicate, Perplexity, and others.[^5]

---

## 10. Error Handling

### Pre-Stream Errors (HTTP status codes)[^5]

```json
{ "error": { "code": 400, "message": "Invalid model specified" } }
```

| HTTP Code | Meaning |
|---|---|
| `400` | Bad Request — invalid parameters |
| `401` | Unauthorized — invalid API key |
| `402` | Payment Required — insufficient credits |
| `429` | Too Many Requests — rate limited |
| `502` | Bad Gateway — provider error |
| `503` | Service Unavailable — no available providers |

### Mid-Stream Errors (SSE payload, HTTP still 200)[^5]

```
data: {"id":"cmpl-abc123","object":"chat.completion.chunk","created":1234567890,"model":"openai/gpt-4o","error":{"code":"server_error","message":"Provider disconnected unexpectedly"},"choices":[{"index":0,"delta":{"content":""},"finish_reason":"error"}]}
```

Key: `finish_reason: "error"` terminates the stream.

### Error Handling Pattern[^5]

```typescript
const stream = await openRouter.chat.send({ ..., stream: true });

for await (const chunk of stream) {
  if ('error' in chunk) {
    console.error(`Stream error: ${chunk.error.message}`);
    if (chunk.choices?.[0]?.finish_reason === 'error') {
      console.log('Stream terminated');
    }
    return;
  }
  const content = chunk.choices?.[0]?.delta?.content;
  if (content) process.stdout.write(content);
}
```

---

## 11. Rate Limits

Check rate limits and credits for an API key:[^5]

```
GET https://openrouter.ai/api/v1/key
Authorization: Bearer <OPENROUTER_API_KEY>
```

**Response schema:**

```typescript
type Key = {
  data: {
    label: string;
    limit: number | null;              // Credit limit, null = unlimited
    limit_reset: string | null;        // Reset interval
    limit_remaining: number | null;    // Credits remaining
    include_byok_in_limit: boolean;
    usage: number;                     // All-time usage
    usage_daily: number;               // Current UTC day
    usage_weekly: number;              // Current UTC week (Mon start)
    usage_monthly: number;             // Current UTC month
    is_free_tier: boolean;             // Has user purchased credits?
  };
};
```

**Free Model Rate Limits:**[^5]

1. **Requests per minute:** Dynamically determined (template placeholder in docs — not publicly fixed)
2. **Per-day limits:**
   - Without purchased credits: lower daily `:free` model request limit
   - With credits purchased above threshold: higher daily `:free` model request limit
3. **DDoS protection:** Cloudflare blocks dramatically excessive usage

> Creating additional accounts or API keys does NOT affect rate limits — capacity is governed globally.[^5]

> If your account has a **negative credit balance**, you'll see `402 Payment Required` errors even for free models.[^5]

---

## 12. Provider Routing

The `provider` object in the request body controls how OpenRouter routes requests.[^6]

### Provider Object Schema[^6]

| Field | Type | Default | Description |
|---|---|---|---|
| `order` | string[] | — | Provider slugs to try in priority order |
| `allow_fallbacks` | boolean | `true` | Allow fallback to other providers on failure |
| `require_parameters` | boolean | `false` | Only route to providers supporting ALL request params |
| `data_collection` | `"allow"\|"deny"` | `"allow"` | Control routing to data-logging providers |
| `zdr` | boolean | — | Only use Zero Data Retention endpoints |
| `only` | string[] | — | Whitelist specific providers for this request |
| `ignore` | string[] | — | Skip specific providers |
| `quantizations` | string[] | — | Filter by quantization: `int4`, `int8`, `fp8`, `fp16`, `bf16`, etc. |
| `sort` | string\|object | — | Sort by `"price"`, `"throughput"`, or `"latency"` |
| `preferred_min_throughput` | number\|object | — | Minimum tokens/sec preference (p50, p75, p90, p99) |
| `preferred_max_latency` | number\|object | — | Maximum latency preference in seconds |
| `max_price` | object | — | Hard price cap: `{ "prompt": 1, "completion": 2 }` (per million tokens) |

### Default Load Balancing Strategy[^6]

1. Prioritize providers without recent outages (last 30 seconds)
2. Weight by inverse square of price among stable providers
3. Use remaining as fallbacks

### Examples[^6]

```json
// Sort by throughput (fastest providers)
{ "provider": { "sort": "throughput" } }

// Specific provider order with fallback
{ "provider": { "order": ["openai", "together"] } }

// Only Azure, no fallbacks
{ "provider": { "only": ["azure"], "allow_fallbacks": false } }

// Skip DeepInfra
{ "provider": { "ignore": ["deepinfra"] } }

// Privacy: no data-logging providers
{ "provider": { "data_collection": "deny" } }

// ZDR (Zero Data Retention)
{ "provider": { "zdr": true } }

// Hard price cap: ≤$1/M prompt, ≤$2/M completion
{ "provider": { "max_price": { "prompt": 1, "completion": 2 } } }

// Performance thresholds (preferred, not guaranteed)
{
  "provider": {
    "preferred_max_latency": { "p50": 1, "p90": 3 },
    "preferred_min_throughput": { "p50": 100 }
  }
}
```

### Targeting Specific Provider Endpoints[^6]

| Slug | What It Matches |
|---|---|
| `"google-vertex"` | All Google Vertex regions |
| `"google-vertex/us-east5"` | Only us-east5 region |
| `"deepinfra"` | All DeepInfra endpoints |
| `"deepinfra/turbo"` | Only DeepInfra turbo endpoint |

### Anthropic Beta Features (via `x-anthropic-beta` header)[^6]

| Feature | Header Value |
|---|---|
| Fine-Grained Tool Streaming | `fine-grained-tool-streaming-2025-05-14` |
| Interleaved Thinking | `interleaved-thinking-2025-05-14` |
| Structured Outputs | `structured-outputs-2025-11-13` |

---

## 13. Model Variants & Slug Conventions

### Static Variants (model-specific)[^1][^6]

| Suffix | Description |
|---|---|
| `:free` | Free, no-cost access — low rate limits |
| `:extended` | Extended context window |
| `:thinking` | Enables extended reasoning / chain-of-thought |

### Dynamic Variants (any model)[^1][^6]

| Suffix | Equivalent To | Description |
|---|---|---|
| `:nitro` | `provider.sort = "throughput"` | Route to fastest provider |
| `:floor` | `provider.sort = "price"` | Route to cheapest provider |
| `:exacto` | Quality-first tool-calling sort | Best for agentic/tool-use workflows |
| `:online` | `plugins: [{ id: "web" }]` | **DEPRECATED** — use `openrouter:web_search` tool instead |

### Model Slug Formats[^1]

```
provider/model-name                     Standard: "anthropic/claude-sonnet-4"
provider/model-name:free               Free variant: "meta-llama/llama-3.2-3b-instruct:free"
provider/model-name:nitro              Throughput: "meta-llama/llama-3.3-70b-instruct:nitro"
provider/model-name:thinking           Reasoning: "deepseek/deepseek-r1:thinking"
~provider/family-latest                Latest alias: "~anthropic/claude-sonnet-latest"
openrouter/free                        Random free model
openrouter/auto                        Auto-routing
```

---

## 14. Latest Model Resolution (`~`)

`~author/family-latest` slugs **always resolve to the newest concrete model** in a family.[^6]

```json
{ "model": "~anthropic/claude-opus-latest" }
```

**Response shows the concrete model:**[^6]
```json
{
  "model": "anthropic/claude-opus-4.8",
  "choices": [...]
}
```

**How it works:**[^6]
1. OpenRouter sees the `~` prefix and identifies the target family
2. Selects the newest visible model in that family
3. Routes and forwards as if you'd called the concrete slug
4. Response `model` field reports the concrete model

**Limitations:**[^6]
- Versions can change at any time — use concrete slugs for reproducibility
- Never resolves to other aliases or hidden models
- No way to pin to "second newest" via the alias

---

## 15. Model Fallbacks

Use the `models` array to automatically try fallback models.[^6]

```json
{
  "models": ["~anthropic/claude-sonnet-latest", "gryphe/mythomax-l2-13b"],
  "messages": [...]
}
```

Any error can trigger fallback: context length, moderation, rate limiting, downtime.[^6]

**With OpenAI SDK (via `extra_body`):**[^6]
```python
completion = openai_client.chat.completions.create(
    model="~openai/gpt-latest",
    extra_body={
        "models": ["~anthropic/claude-sonnet-latest", "gryphe/mythomax-l2-13b"],
    },
    messages=[...]
)
```

---

## 16. Auto Router

`openrouter/auto` — powered by NotDiamond — automatically selects the best model for your prompt.[^6]

```json
{ "model": "openrouter/auto" }
```

The response `model` field shows which model was actually chosen.

---

## 17. Free Models

### Free Models Router[^7]

Use `openrouter/free` to randomly select from all available free models. Intelligently filters for feature support (images, tools, structured outputs).[^7]

```bash
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "openrouter/free", "messages": [{"role": "user", "content": "Hello!"}]}'
```

Browse all free models: https://openrouter.ai/models?max_price=0

### Selected Free Models (from Live API, May 2026)[^7]

| Model ID | Name | Context | Modality |
|---|---|---|---|
| `openrouter/free` | Free Models Router | — | Auto-selected |
| `meta-llama/llama-3.2-3b-instruct:free` | Meta Llama 3.2 3B (free) | 131,072 | text→text |
| `meta-llama/llama-3.3-70b-instruct:free` | Meta Llama 3.3 70B (free) | 131,072 | text→text |
| `deepseek/deepseek-v4-flash:free` | DeepSeek V4 Flash (free) | 1,048,576 | text→text |
| `openai/gpt-oss-120b:free` | OpenAI gpt-oss-120b (free) | 131,072 | text→text |
| `google/gemma-4-31b-it:free` | Google Gemma 4 31B (free) | 262,144 | text+image+video→text |
| `qwen/qwen3-coder:free` | Qwen3 Coder 480B A35B (free) | 1,048,576 | text→text |
| `moonshotai/kimi-k2.6:free` | MoonshotAI Kimi K2.6 (free) | 262,144 | text+image→text |
| `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free` | NVIDIA Nemotron 3 Nano Omni (free) | 256,000 | text+image+audio+video→text |

> Free models have low rate limits and are **not suitable for production use**.[^7]

---

## 18. Multimodal — Images

Supported content types: `image/png`, `image/jpeg`, `image/webp`, `image/gif`.[^8]

> **Tip:** Send text prompt first, then images. If images must come first, use system prompt.

### Image via URL[^8]

```python
messages = [{
    "role": "user",
    "content": [
        {"type": "text", "text": "What's in this image?"},
        {"type": "image_url", "image_url": {"url": "https://example.com/image.jpg"}}
    ]
}]
```

### Image via Base64[^8]

```python
import base64

def encode_image_to_base64(image_path):
    with open(image_path, "rb") as f:
        return base64.b64encode(f.read()).decode('utf-8')

base64_image = encode_image_to_base64("path/to/image.jpg")
data_url = f"data:image/jpeg;base64,{base64_image}"

messages = [{
    "role": "user",
    "content": [
        {"type": "text", "text": "What's in this image?"},
        {"type": "image_url", "image_url": {"url": data_url}}
    ]
}]
```

---

## 19. Multimodal — PDFs

PDFs work on **any model** on OpenRouter — even models that don't natively support files.[^8]

> When a model supports file input natively, the PDF is passed directly. Otherwise, OpenRouter parses and passes the text.

### PDF Engines[^8]

| Engine | Description | Cost |
|---|---|---|
| `"native"` | Model's built-in file support | Charged as input tokens |
| `"cloudflare-ai"` | Converts PDF to markdown | Free |
| `"mistral-ocr"` | Best for scanned PDFs/images | Per 1,000 pages |

OCR: max 8 images per PDF extracted; all text preserved.

### PDF via URL[^8]

```python
messages = [{
    "role": "user",
    "content": [
        {"type": "text", "text": "What are the main points in this document?"},
        {"type": "file", "file": {
            "filename": "document.pdf",
            "file_data": "https://bitcoin.org/bitcoin.pdf"
        }}
    ]
}]

plugins = [{"id": "file-parser", "pdf": {"engine": "mistral-ocr"}}]
```

### PDF via Base64[^8]

```python
import base64

with open("document.pdf", "rb") as f:
    base64_pdf = base64.b64encode(f.read()).decode('utf-8')

data_url = f"data:application/pdf;base64,{base64_pdf}"

messages = [{
    "role": "user",
    "content": [
        {"type": "text", "text": "Summarize this document."},
        {"type": "file", "file": {
            "filename": "document.pdf",
            "file_data": data_url
        }}
    ]
}]
```

---

## 20. Tool Calling & Agent SDK

### Raw Tool Calling[^2]

```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get weather for a location",
        "parameters": {
            "type": "object",
            "properties": {"location": {"type": "string"}},
            "required": ["location"]
        }
    }
}]

response = requests.post(
    "https://openrouter.ai/api/v1/chat/completions",
    headers={"Authorization": "Bearer <KEY>"},
    json={"model": "openai/gpt-4o", "messages": [...], "tools": tools}
)
```

### Agent SDK (`@openrouter/agent`)[^1]

Installation:
```bash
npm install @openrouter/agent
# pnpm add @openrouter/agent
# yarn add @openrouter/agent
```

**Example with tool use:**[^1]

```typescript
import { callModel, tool } from '@openrouter/agent';
import { z } from 'zod';

const weatherTool = tool({
  name: 'get_weather',
  description: 'Get the current weather for a location',
  inputSchema: z.object({
    location: z.string().describe('City name'),
  }),
  execute: async ({ location }) => {
    return { temperature: 72, condition: 'sunny', location };
  },
});

const result = await callModel({
  model: '~anthropic/claude-sonnet-latest',
  messages: [
    { role: 'user', content: 'What is the weather in San Francisco?' },
  ],
  tools: [weatherTool],
});

const text = await result.getText();
console.log(text);
```

`callModel` automatically: sends prompt → receives tool call → executes tool → feeds result back → returns final response.[^1]

---

## 21. Web Search Server Tool

The `openrouter:web_search` tool gives **any model** access to real-time web results.[^5]

> Replaces the deprecated `:online` suffix. Preferred approach as of 2025.

### How It Works[^5]

1. Include `{ "type": "openrouter:web_search" }` in `tools`
2. Model decides if search is needed and generates a query
3. OpenRouter executes search (defaults to `auto` — native provider search or [Exa](https://exa.ai) fallback)
4. Results (URLs, titles, snippets) returned to model
5. Model synthesizes a cited response (may search multiple times)

```typescript
const response = await fetch('https://openrouter.ai/api/v1/chat/completions', {
  method: 'POST',
  headers: { Authorization: 'Bearer <KEY>', 'Content-Type': 'application/json' },
  body: JSON.stringify({
    model: 'openai/gpt-4o',
    messages: [{ role: 'user', content: "What's the latest news about AI?" }],
    tools: [{ type: 'openrouter:web_search' }],
  }),
});
```

---

## 22. Plugins

Configure via the `plugins` array in the request:[^2]

| Plugin ID | Description |
|---|---|
| `web` | Real-time web search (legacy — use `openrouter:web_search` tool instead) |
| `file-parser` | PDF processing (configure `pdf.engine`: `native`, `cloudflare-ai`, `mistral-ocr`) |
| `response-healing` | Automatic JSON repair for malformed outputs |
| `context-compression` | Middle-out prompt compression to reduce costs |

```json
{
  "plugins": [
    { "id": "response-healing" },
    { "id": "context-compression" }
  ]
}
```

---

## 23. App Attribution

Include attribution headers to appear on OpenRouter's public leaderboards and analytics.[^1]

### Attribution Headers[^1]

| Header | Role |
|---|---|
| `HTTP-Referer: <url>` | **Required** to create an app page |
| `X-OpenRouter-Title: <name>` | Sets display name (must pair with HTTP-Referer) |
| `X-OpenRouter-Categories: <cats>` | Comma-separated marketplace categories |

### App Categories[^1]

**Coding:** `cli-agent`, `ide-extension`, `cloud-agent`, `programming-app`, `native-app-builder`  
**Creative:** `creative-writing`, `video-gen`, `image-gen`  
**Productivity:** `writing-assistant`, `general-chat`, `personal-agent`  
**Entertainment:** `roleplay`, `game`

### Full Attribution Example[^1]

```typescript
const client = new OpenRouter({
  apiKey: '<KEY>',
  httpReferer: 'https://myapp.com',
  appTitle: 'My AI Assistant',
  appCategories: 'cli-agent,cloud-agent',
});
```

---

## 24. Credits & Billing

### Getting Started with Credits[^1]

1. Create account at https://openrouter.ai
2. Add credits at https://openrouter.ai/settings/credits
3. Create API keys at https://openrouter.ai/keys
4. Start making API calls

### Credit System[^1]

- Base currency: **US dollars**
- Pricing: OpenRouter **passes through provider pricing without markup** (fee charged on credit purchase only)
- Auto top-up available when balance drops below threshold
- Credits expire after 1 year of purchase

### Check Credits via API[^7]

```
GET https://openrouter.ai/api/v1/credits
Authorization: Bearer <MANAGEMENT_KEY>
```

Response:
```json
{
  "data": {
    "total_credits": 50.00,
    "total_usage": 12.35
  }
}
```

Remaining = `total_credits` − `total_usage`

### Accepted Payment Methods[^1]

- All major credit cards
- AliPay
- Cryptocurrency (USDC) — **non-refundable**

### Refund Policy[^1]

- Within 24 hours of purchase: refund available via https://openrouter.ai/settings/credits
- Platform fees are non-refundable
- Cryptocurrency payments: never refundable

### Sample Paid Model Pricing (May 2026, per million tokens)[^7]

| Model | Prompt | Completion |
|---|---|---|
| DeepSeek V4 Flash (paid) | $0.000098/M | $0.000197/M |
| Google Gemini 3.5 Flash | $1.50/M | $9.00/M |
| Anthropic Claude Opus 4.8 | $5.00/M | $25.00/M |
| OpenAI GPT-5.5 Pro | $30.00/M | $180.00/M |

---

## 25. Privacy & Data Logging

- **Default:** Zero logging of prompts/completions — even on errors[^1]
- **Opt-in:** Enable prompt/completion logging in settings for a 1% usage discount
- **Chatroom:** Conversations stored locally on device only — no cross-device sync
- **Provider logging:** Providers that log data or train on prompts are NOT routed to unless "model training" toggle is ON in privacy settings
- **OpenRouter is a GitHub secret scanning partner** — leaked keys trigger email notifications[^3]

---

## 26. Key Repositories & Resources

| Resource | URL / Description |
|---|---|
| **TypeScript Client SDK** | [OpenRouterTeam/typescript-sdk](https://github.com/OpenRouterTeam/typescript-sdk) — `npm install @openrouter/sdk` |
| **TypeScript Agent SDK** | [OpenRouterTeam/typescript-agent](https://github.com/OpenRouterTeam/typescript-agent) — `npm install @openrouter/agent` |
| **Python Client SDK** | `pip install openrouter` (auto-generated via Speakeasy) |
| **Official LangGraph Quickstart** | [Motivus-AI/quickstart-openrouter](https://github.com/Motivus-AI/quickstart-openrouter) |
| **OpenAPI Spec (YAML)** | https://openrouter.ai/openapi.yaml |
| **OpenAPI Spec (JSON)** | https://openrouter.ai/openapi.json |
| **Docs Index** | https://openrouter.ai/docs/llms.txt |
| **Request Builder** | https://openrouter.ai/request-builder |
| **Models Browser** | https://openrouter.ai/models |
| **Free Models** | https://openrouter.ai/models?max_price=0 |
| **Activity Dashboard** | https://openrouter.ai/activity |
| **Discord Community** | https://discord.gg/openrouter |
| **MCP Server** | https://openrouter.ai/docs/_mcp/server |

### API Endpoints Summary

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/chat/completions` | Chat completions (primary) |
| `GET` | `/api/v1/models` | List all available models |
| `GET` | `/api/v1/key` | Current API key info & rate limits |
| `GET` | `/api/v1/credits` | Account credit balance (management key) |
| `GET` | `/api/v1/generation?id=$ID` | Per-generation stats |
| `GET/POST/PATCH/DELETE` | `/api/v1/keys/*` | Key management (management key) |
| `POST` | `/api/v1/auth/keys` | Exchange OAuth code for API key |

---

## 27. Architecture Diagram

```mermaid
flowchart TD
    A["Your Application"] -->|HTTP POST| B["OpenRouter API\nhttps://openrouter.ai/api/v1"]
    
    B --> C{Routing Engine}
    
    C -->|"model: ~provider/latest\n~ = latest alias"| D[Latest Resolution]
    C -->|"model: openrouter/auto\nNotDiamond"| E[Auto Router]
    C -->|"models: [...]\nFallbacks"| F[Model Fallback Chain]
    C -->|"Standard model slug"| G[Provider Load Balancer]
    
    D --> G
    E --> G
    F --> G
    
    G -->|"provider.sort = price"| H[Price-Weighted Balancer]
    G -->|"provider.sort = throughput / :nitro"| I[Throughput-First Sort]
    G -->|"provider.sort = latency"| J[Latency-First Sort]
    G -->|"provider.order = [...]"| K[Explicit Order]
    G -->|":exacto"| L[Quality-First Sort]
    
    H & I & J & K & L --> M{Provider Selection}
    
    M --> N1["OpenAI / Azure"]
    M --> N2["Anthropic / AWS Bedrock"]
    M --> N3["Google / Vertex AI"]
    M --> N4["DeepSeek, Groq, etc."]
    M --> N5["300+ Other Models"]
    
    N1 & N2 & N3 & N4 & N5 -->|Response| O[OpenRouter Response]
    
    O -->|"stream: false"| P[Complete JSON Response]
    O -->|"stream: true"| Q[SSE Stream\ndata: {...}\n: OPENROUTER PROCESSING]
    
    P & Q --> A

    subgraph "Plugins (optional)"
        R["file-parser (PDF)"]
        S["web search"]
        T["response-healing"]
        U["context-compression"]
    end
    
    B -.-> R & S & T & U

    subgraph "Auth Methods"
        V["Bearer Token (API Key)"]
        W["OAuth PKCE Flow"]
        X["Management API Key"]
        Y["BYOK (Provider Keys)"]
    end
    
    A -.->|"Authorization: Bearer"| V & W & X & Y
```

---

## 28. Confidence Assessment

### High Confidence (directly fetched from official sources)
- All code examples — fetched verbatim from `openrouter.ai/docs/quickstart.md`[^1]
- Authentication flows — from `openrouter.ai/docs/api/reference/authentication.md`[^3]
- OAuth PKCE — from `openrouter.ai/docs/guides/overview/auth/oauth.md`[^4]
- Request/response schemas — from `openrouter.ai/docs/api/reference/parameters.md`[^2]
- Provider routing — from `openrouter.ai/docs/guides/routing/provider-selection.md`[^6]
- Streaming — from `openrouter.ai/docs/api/reference/streaming.md`[^5]
- Free model list — from live `openrouter.ai/api/v1/models` API[^7]
- Multimodal/PDF docs — from official docs pages[^8]

### Medium Confidence (dynamic content or template placeholders)
- **Rate limit numbers:** The exact RPM/RPD values for free models are rendered dynamically on the live page and returned as `{FREE_MODEL_RATE_LIMIT_RPM}` placeholders in the markdown — they are NOT fixed in docs
- **Exact credit purchase threshold:** Also a dynamic placeholder
- **Exact BYOK fee percentage:** Described as "a percentage" without a fixed number

### Assumptions Made
- Model slugs in the "Selected Free Models" section reflect the live API state at research time (May 2026) and will change as new models are added
- Pricing examples in Section 24 are approximate from live API data and change regularly

---

## Footnotes

[^1]: [https://openrouter.ai/docs/quickstart.md](https://openrouter.ai/docs/quickstart)  
[^2]: [https://openrouter.ai/docs/api/reference/parameters.md](https://openrouter.ai/docs/api/reference/parameters) (also covers request/response schema)  
[^3]: [https://openrouter.ai/docs/api/reference/authentication.md](https://openrouter.ai/docs/api/reference/authentication)  
[^4]: [https://openrouter.ai/docs/guides/overview/auth/oauth.md](https://openrouter.ai/docs/guides/overview/auth/oauth) — Management keys from `/guides/overview/auth/management-api-keys.md`; BYOK from `/guides/overview/auth/byok.md`  
[^5]: [https://openrouter.ai/docs/api/reference/streaming.md](https://openrouter.ai/docs/api/reference/streaming) — Errors from streaming doc; limits from `/api/reference/limits.md`; web search from `/guides/features/server-tools/web-search.md`  
[^6]: [https://openrouter.ai/docs/guides/routing/provider-selection.md](https://openrouter.ai/docs/guides/routing/provider-selection) — Model variants from `/routing/model-variants/*`; latest-resolution from `/routing/routers/latest-resolution.md`; fallbacks from `/routing/model-fallbacks.md`; auto router from `/routing/routers/auto-router.md`  
[^7]: Live API: [https://openrouter.ai/api/v1/models](https://openrouter.ai/api/v1/models) — Free models docs from `/guides/routing/model-variants/free.md` and `/cookbook/get-started/free-models-router-playground.md`; credits API from `/api/api-reference/credits/get-credits.md`  
[^8]: [https://openrouter.ai/docs/guides/overview/multimodal/image-understanding.md](https://openrouter.ai/docs/guides/overview/multimodal/image-understanding) — PDFs from `/multimodal/pdfs.md`; FAQ from `/docs/faq.md`; app attribution from `/docs/app-attribution.md`  
