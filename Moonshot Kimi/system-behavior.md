# Moonshot Kimi System Behavior

**Models:** K2, K2.5, K2-thinking  
**Context Window:** 256,000 tokens  
**API:** OpenAI-compatible (`https://api.moonshot.cn/v1`)

## Model Variants

| Model | Strengths | Best For |
|-------|-----------|----------|
| `kimi-k2.5` | Fast, balanced | General tasks, coding |
| `kimi-k2` | Cost-effective | Simple queries, routing |
| `kimi-k2-thinking` | Deep reasoning | Complex analysis, math |

## System Behavior Patterns

### Tool Calling
Kimi uses OpenAI-compatible function calling with specific characteristics:

- **Parallel tool calls**: Supported
- **Multi-step reasoning**: Automatically chains tool calls
- **Error handling**: Graceful fallback on tool failure
- **Streaming**: Full streaming support for responses

### Response Patterns

**Non-thinking models (k2, k2.5):**
- Direct, concise responses
- Code blocks with proper formatting
- Tool results integrated naturally

**Thinking model (k2-thinking):**
- Shows reasoning process
- Self-correction during generation
- Better for complex multi-step problems

### Context Handling
- 256k context window
- Efficient token usage
- Maintains context across long conversations
- Good at referencing earlier parts of conversation

## API Behavior

### Temperature Handling
- Default: 0.3 (more deterministic)
- Range: 0.0 - 1.0
- Lower values = more focused responses

### Rate Limits
- Varies by tier
- Generally permissive for API usage
- No aggressive throttling observed

## Comparison with Other Models

| Aspect | Kimi K2.5 | Claude 3.5 | GPT-4 |
|--------|-----------|------------|-------|
| Tool Use | Native | Native | Native |
| Context | 256k | 200k | 128k |
| Speed | Fast | Medium | Medium |
| Cost | Lower | Higher | Higher |
| Reasoning | Good | Excellent | Excellent |

## Notes
- Fully compatible with OpenAI SDK
- Supports custom headers for routing
- Good at following system instructions
- Handles multi-language prompts well
