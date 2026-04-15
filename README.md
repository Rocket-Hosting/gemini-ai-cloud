# Gemini AI Cloud

A hosted OpenAI-compatible API for Google's Gemini abd Gemma AI models.

## 🚀 API Access

### Free API Key
- **API Key:** `sk-8xMIqGFuWezqJyEWFWG4Ow`
- **Base URL:** `http://server2.api.nikkcocompany.store:4000/v1/chat/completions`
- Currently has access to **all models** (same as paid keys)
- ⚠️ Model availability for free keys may change at any time, with or without notice
- Updates will **always** be shared first in [Discord](https://discord.gg/F6mBjR8Jnt), and later posted on GitHub

👉 **Join the [Discord server](https://discord.gg/F6mBjR8Jnt)** to stay updated!

### Paid API Keys – Pricing

Contact us on [Discord](https://discord.gg/F6mBjR8Jnt) to purchase a paid API key.

#### Gemini Models

| Model | Input (per 1M) | Output (per 1M) |
|-------|----------------|-----------------|
| gemini-3.1-pro-preview | $0.20 | $0.90 |
| gemini-3.1-pro-preview:search | $0.22 | $0.99 |
| gemini-3-flash-preview | $0.025 | $0.15 |
| gemini-3-flash-preview:search | $0.0275 | $0.165 |
| gemini-3.1-flash-lite-preview | $0.0125 | $0.075 |
| gemini-3.1-flash-lite-preview:search | $0.01375 | $0.0825 |
| gemini-2.5-flash | $0.015 | $0.125 |
| gemini-2.5-flash:search | $0.0165 | $0.1375 |
| gemini-2.5-flash-lite | $0.005 | $0.02 |
| gemini-2.5-flash-lite:search | $0.0055 | $0.022 |
| gemini-2.5-pro | $0.125 | $0.75 |
| gemini-2.5-pro:search | $0.1375 | $0.825 |
| gemini-robotics-er-1.5-preview | $0.015 | $0.125 |
| gemini-robotics-er-1.5-preview:search | $0.0165 | $0.1375 |

#### Gemma Models

| Model | Input (per 1M) | Output (per 1M) |
|-------|----------------|-----------------|
| gemma-4-26b-a4b-it | $0.0065 | $0.02 |
| gemma-4-26b-a4b-it:search | $0.00715 | $0.022 |
| gemma-4-31b-it | $0.007 | $0.02 |
| gemma-4-31b-it:search | $0.0077 | $0.022 |
| gemma-3-27b-it | $0.0015 | $0.0055 |
| gemma-3-12b-it | $0.002 | $0.0065 |
| gemma-3-4b-it | $0.00085 | $0.0034 |
| gemma-3-1b-it | $0.00075 | $0.002 |

> Models with `:search` suffix enable automatic Google Search grounding.
>
> **Support `:search`:** all Gemini models, `gemma-4-26b-a4b-it`, `gemma-4-31b-it`
> **Do NOT support `:search`:** `gemma-3-27b-it`, `gemma-3-12b-it`, `gemma-3-4b-it`, `gemma-3-1b-it`

## Features

- 🤖 **OpenAI Compatible** – Use with any OpenAI SDK or tool
- 👁️ **Vision Support** – Process images with Gemini Vision models
- 🛠️ **Tool Calling** – Full function calling support
- 🌐 **Web Search** – Models with `:search` suffix enable automatic Google Search grounding
- 🔑 **Simple Auth** – Just use your API key

## Quick Start

### Using the API

Set your API key and base URL in your OpenAI SDK:

**Python:**
```python
from openai import OpenAI

client = OpenAI(
    base_url="http://server2.api.nikkcocompany.store:4000/v1",
    api_key="sk-8xMIqGFuWezqJyEWFWG4Ow"
)

response = client.chat.completions.create(
    model="gemini-3.1-pro-preview",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

**JavaScript:**
```javascript
import OpenAI from 'openai';

const openai = new OpenAI({
    baseURL: 'http://server2.api.nikkcocompany.store:4000/v1',
    apiKey: 'sk-8xMIqGFuWezqJyEWFWG4Ow'
});

const completion = await openai.chat.completions.create({
    model: 'gemini-3.1-pro-preview',
    messages: [{ role: 'user', content: 'Hello!' }]
});
console.log(completion.choices[0].message.content);
```

**cURL:**
```bash
curl http://server2.api.nikkcocompany.store:4000/v1/chat/completions \
  -H "Authorization: Bearer sk-8xMIqGFuWezqJyEWFWG4Ow" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemini-3.1-pro-preview",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

## API Usage

### Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /v1/models` | List all available models |
| `POST /v1/chat/completions` | Create a chat completion |

### Chat Completions

**Example with vision:**
```json
{
  "model": "gemini-3.1-pro-preview",
  "messages": [
    {
      "role": "user",
      "content": [
        {"type": "text", "text": "What's in this image?"},
        {
          "type": "image_url",
          "image_url": {
            "url": "data:image/jpeg;base64,/9j/4AAQSkZJRg..."
          }
        }
      ]
    }
  ]
}
```

**Example with tool calling:**
```json
{
  "model": "gemini-3.1-pro-preview",
  "messages": [{"role": "user", "content": "What's the weather in London?"}],
  "tools": [{
    "type": "function",
    "function": {
      "name": "get_weather",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {"type": "string"}
        }
      }
    }
  }]
}
```

**Example with web search:**
```json
{
  "model": "gemini-3.1-pro-preview:search",
  "messages": [{"role": "user", "content": "Latest news about AI"}]
}
```

### Response Format

Standard OpenAI chat completion response:
```json
{
  "id": "chatcmpl-123",
  "object": "chat.completion",
  "created": 1677652288,
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "AI response here"
    },
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 10,
    "completion_tokens": 20,
    "total_tokens": 30
  }
}
```

### Deprecated Models

`gemini-3-pro-preview` (with and without `:search`) was deprecated by Google. Use `gemini-3.1-pro-preview` instead.

## Support

For issues or questions, updates join our [Discord server](https://discord.gg/F6mBjR8Jnt).
