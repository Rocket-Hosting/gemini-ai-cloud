```markdown
# Gemini AI Cloud

A lightweight OpenAI-compatible API proxy for Google's Gemini AI models with time-based access control.

## Overview

Gemini AI Cloud provides a simple HTTP interface to Google's Gemini AI models that's fully compatible with OpenAI's API format. Access is controlled via access keys with time-based expiration. Perfect for sharing AI access with time-limited permissions.

## Features

- 🤖 **OpenAI Compatible** – Use with any OpenAI SDK or tool (endpoints are direct, no `/v1` prefix)
- 👁️ **Vision Support** – Process images with Gemini Vision models
- 🛠️ **Tool Calling** – Full function calling support
- 🌐 **Web Search** – Models with `:search` suffix enable automatic Google Search grounding
- ⏱️ **Time-based Access** – Control access duration (1 day, 2 days, etc.)
- 🔑 **Simple Auth** – No API key required, only an access key

## Quick Start

### 1. Download on linux https://github.com/Rocket-Hosting/gemini-ai-cloud/releases/latest/download/validator

```

### 2. Set the access key (required)

The server requires an access key to validate requests.
A free testing key `hbhbbhbh` is available for evaluation. To use it, set:

```bash
export ACCESS_KEY=hbhbbhbh
```

You may also use your own custom access key if you have one.

fix permissions (only required for initial run)
```bash
chmod +x validator
```
### 3. Run the server

```bash
./validator --port 8080 --host 0.0.0.0
```

Or with default settings:
```bash
./validator
```

## Configuration

### Command Line Options

| Option | Description | Default |
|--------|-------------|---------|
| `--port <number>` | Port to listen on | 8080 |
| `--host <string>` | Host to bind to | 0.0.0.0 |
| `--help` | Show help message | – |

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `ACCESS_KEY` | Secret key for API access | **Yes** |

The server will not start unless `ACCESS_KEY` is set.

## API Usage

All endpoints are served directly (no `/v1` prefix).

### Authentication

All requests must include the access key in the `Authorization` header:

```bash
curl -H "Authorization: Bearer not-required" \
-H "Content-Type: application/json" \
-d '{"messages": [{"role": "user", "content": "Hello!"}]}' \
http://your-server:8080/chat/completions
```

### Available Models

```
GET /models
```

**Example response:**
```json
{
"object": "list",
"data": [
{ "id": "gemini-3-pro-preview", "object": "model", "created": 1772277201285 },
{ "id": "gemini-3-pro-preview:search", "object": "model", "created": 1772277201285 },
{ "id": "gemini-3-flash-preview", "object": "model", "created": 1772277201285 },
{ "id": "gemini-3-flash-preview:search", "object": "model", "created": 1772277201285 },
{ "id": "gemini-2.5-flash", "object": "model", "created": 1772277201285 },
{ "id": "gemini-2.5-flash:search", "object": "model", "created": 1772277201285 },
{ "id": "gemini-2.5-flash-lite", "object": "model", "created": 1772277201285 },
{ "id": "gemini-2.5-flash-lite:search", "object": "model", "created": 1772277201285 },
{ "id": "gemini-2.5-pro", "object": "model", "created": 1772277201285 },
{ "id": "gemini-2.5-pro:search", "object": "model", "created": 1772277201285 }
]
}
```

Models with the `:search` suffix automatically enable Google Search grounding.

### Endpoints

#### POST /chat/completions
OpenAI-compatible chat completions.

**Example with vision:**
```json
{
"model": "gemini-3-pro-preview",
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
"model": "gemini-3-pro-preview",
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

**Example with web search (using `:search` model):**
```json
{
"model": "gemini-3-pro-preview:search",
"messages": [{"role": "user", "content": "Latest news about AI"}]
}
```

### Response Format

The API returns standard OpenAI chat completion responses:

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

## Access Control

Access is **time‑based**, not token‑based. Each access key has an expiration time that determines how long it can be used.

- **Free testing key:** `hbhbbhbh` – available for evaluation, but may expire or be revoked at any time without notice
- Paid access keys: For production use or guaranteed availability, you can obtain a paid access key.

Pricing: 1$ per day of access.

Custom keys with specific durations are available – please contact us on Discord to purchase.

⚠️ **Important Notes:**
- The access key **must** be set via the `ACCESS_KEY` environment variable – there is no default.
- free key can expire or be changed without notice.
- For production use, obtain a dedicated access key with a guaranteed validity period.

## Deployment Examples

### Run in background
```bash
nohup ./validator --port 3000 > gemini.log 2>&1 &
```

### Using systemd (Linux)
Create `/etc/systemd/system/gemini-ai-cloud.service`:
```ini
[Unit]
Description=Gemini AI Cloud
After=network.target

[Service]
Type=simple
User=ubuntu123
WorkingDirectory=/home/ubuntu123/gemini
Environment="ACCESS_KEY=your_key_here"
ExecStart=/home/ubuntu123/gemini/validator --port 8080
Restart=always

[Install]
WantedBy=multi-user.target
```

### Using Docker
```dockerfile
FROM ubuntu:latest
COPY validator /app/
EXPOSE 8080
ENV ACCESS_KEY=hbhbbhbh
CMD ["/app/validator", "--port", "8080"]
```

## SDK Examples

### Python (OpenAI SDK)
```python
from openai import OpenAI

client = OpenAI(
base_url="http://your-server:8080", # no /v1
api_key="YOUR_ACCESS_KEY" # replace with your access key
)

response = client.chat.completions.create(
model="gemini-3-pro-preview",
messages=[{"role": "user", "content": "Hello!"}]
)
```

### JavaScript
```javascript
import OpenAI from 'openai';

const openai = new OpenAI({
baseURL: 'http://your-server:8080', // no /v1
apiKey: 'YOUR_ACCESS_KEY' // replace with your access key
});

const completion = await openai.chat.completions.create({
model: 'gemini-3-pro-preview',
messages: [{ role: 'user', content: 'Hello!' }]
});
```

### cURL
```bash
curl http://your-server:8080/chat/completions \
-H "Authorization: Bearer YOUR_ACCESS_KEY" \
-H "Content-Type: application/json" \
-d '{
"model": "gemini-3-pro-preview",
"messages": [{"role": "user", "content": "Hello!"}]
}'
```

## Security Considerations

1. Always use HTTPS in production (consider a reverse proxy like Nginx or Caddy).
2. Rotate access keys regularly.
3. Monitor access logs for unusual activity.
4. Set reasonable time limits for access keys.

## License

MIT License – see the [LICENSE](LICENSE) file for details.

## Support

For issues or questions, please open an issue on GitHub:
discord @rocket.hosting
```
