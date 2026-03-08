# Moonshot Kimi Tool Use Patterns

## Basic Tool Calling

Kimi follows OpenAI's function calling specification:

```json
{
  "model": "kimi-k2.5",
  "messages": [
    {"role": "system", "content": "You have access to tools."},
    {"role": "user", "content": "What's the weather in Dubai?"}
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "Get weather for a location",
        "parameters": {
          "type": "object",
          "properties": {
            "location": {"type": "string"},
            "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
          },
          "required": ["location"]
        }
      }
    }
  ]
}
```

## Multi-Tool Orchestration

Kimi automatically chains multiple tool calls:

**Example: Research task**
1. Search for information
2. Fetch specific article
3. Summarize findings

Kimi will call tools in sequence, waiting for results before proceeding.

## Error Handling Patterns

### Invalid Tool Response
When a tool returns an error, Kimi:
1. Acknowledges the error
2. Attempts alternative approach
3. Falls back to general knowledge if needed

### Missing Parameters
Kimi will ask for required parameters if not provided:

```
User: "Check weather"
Kimi: "I'd be happy to check the weather. Which city would you like me to look up?"
```

## Streaming Tool Calls

Kimi supports streaming with tool calls:

```python
# Streaming response includes:
# - tool_calls delta (function name, arguments)
# - content delta (response text)
# - finish_reason (stop/tool_calls)
```

## Complex Multi-Step Examples

### Example 1: Data Analysis Pipeline
```
User: "Analyze our sales data and email the report"

Step 1: Tool call - fetch_sales_data()
Step 2: Tool call - analyze_data(results_from_step_1)
Step 3: Tool call - generate_email_report(analysis_results)
Step 4: Final response with summary
```

### Example 2: Research and Summarize
```
User: "Research the latest Middle East conflicts and create a briefing"

Step 1: Tool call - search_news("Middle East conflicts 2026")
Step 2: Tool call - fetch_articles([url1, url2, url3])
Step 3: Tool call - summarize_findings(articles)
Step 4: Tool call - create_briefing_doc(summary)
```

## Best Practices

1. **Clear descriptions**: Kimi relies on function descriptions
2. **Specific parameters**: Well-defined schemas improve accuracy
3. **Error handling**: Always provide meaningful error messages
4. **Parallel calls**: Group independent calls together

## Common Patterns

| Pattern | Behavior |
|---------|----------|
| Single tool | Direct execution |
| Sequential | Waits for each result |
| Parallel | Calls independent tools together |
| Conditional | Decides based on tool results |
| Looping | Repeats until condition met |
