# Moonshot Kimi API Integration

## Base URL

```
https://api.moonshot.cn/v1
```

## Authentication

Header: `Authorization: Bearer YOUR_API_KEY`

## OpenAI SDK Compatibility

Kimi is fully compatible with OpenAI's Python SDK:

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_KIMI_API_KEY",
    base_url="https://api.moonshot.cn/v1"
)

response = client.chat.completions.create(
    model="kimi-k2.5",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

## Available Models

| Model Name | Description |
|------------|-------------|
| `kimi-k2.5` | Latest general-purpose model |
| `kimi-k2` | Fast, cost-effective |
| `kimi-k2-thinking` | Enhanced reasoning |

## Request Format

```json
{
  "model": "kimi-k2.5",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Explain quantum computing"}
  ],
  "temperature": 0.3,
  "max_tokens": 4096,
  "stream": false
}
```

## Response Format

Standard OpenAI-compatible response:

```json
{
  "id": "chatcmpl-xxx",
  "object": "chat.completion",
  "created": 1234567890,
  "model": "kimi-k2.5",
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "Quantum computing uses quantum bits..."
    },
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 15,
    "completion_tokens": 150,
    "total_tokens": 165
  }
}
```

## Streaming Example

```python
stream = client.chat.completions.create(
    model="kimi-k2.5",
    messages=[{"role": "user", "content": "Write a poem"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

## Tool Calling Example

```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_current_time",
        "description": "Get current time",
        "parameters": {
            "type": "object",
            "properties": {},
            "required": []
        }
    }
}]

response = client.chat.completions.create(
    model="kimi-k2.5",
    messages=messages,
    tools=tools
)

# Check for tool calls
if response.choices[0].message.tool_calls:
    tool_call = response.choices[0].message.tool_calls[0]
    function_name = tool_call.function.name
    arguments = json.loads(tool_call.function.arguments)
```

## Error Handling

Common errors:

| Status | Meaning |
|--------|---------|
| 401 | Invalid API key |
| 429 | Rate limit exceeded |
| 500 | Server error |

## Cost Tracking

Use the `usage` field in responses:

```python
response = client.chat.completions.create(...)
tokens_used = response.usage.total_tokens
```

## Custom Headers

Optional headers for routing:

```python
client = OpenAI(
    api_key="YOUR_KEY",
    base_url="https://api.moonshot.cn/v1",
    default_headers={
        "X-Custom-Header": "value"
    }
)
```
