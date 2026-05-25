# Configuration API

## LLM Configuration Schema

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent Configuration API
  version: 2.0.0
```

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `model` | `str` | ✅ | Model name (e.g., `'qwen3-max'`, `'qwen3-vl-plus'`) |
| `model_type` | `str` | ✅ | Provider identifier |
| `model_server` | `str` | ⚠️ | API endpoint for OpenAI-compatible services |
| `api_key` | `str` | ❌ | Authentication key (falls back to env vars) |
| `generate_cfg` | `dict` | ❌ | Generation behavior control |

### Provider Types

| Value | Capability | Input → Output |
|-------|------------|----------------|
| `qwen_dashscope` | LLM | Text → Text |
| `qwenvl_dashscope` | VLM | Text/Image/Video → Text |
| `qwenaudio_dashscope` | Omni | Text/Image/Video/Audio → Text |
| `oai` | LLM | OpenAI-compatible |
| `qwenvl_oai` | VLM | OpenAI-compatible VLM |
| `qwenaudio_oai` | Omni | OpenAI-compatible Omni |

### generate_cfg Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `max_input_tokens` | `int` | 90000 | Context length limit before truncation |
| `use_raw_api` | `bool` | `False` | Use model server's native tool-call parsing |
| `enable_thinking` | `bool` | — | Enable reasoning mode if supported |
| `top_p`, `temperature` | `float` | — | Passed directly to model API |

## Examples

### DashScope API

```python
llm_cfg = {
    'model': 'qwen3-max-preview',
    'model_type': 'qwen_dashscope',
    'generate_cfg': {
        'enable_thinking': True,
        'use_raw_api': True,
        'top_p': 0.8,
    }
}
```

### Local vLLM/SGLang

```python
llm_cfg = {
    'model': 'Qwen3-8B',
    'model_server': 'http://localhost:8000/v1',
    'api_key': 'EMPTY',
    'generate_cfg': {
        'top_p': 0.85,
        'extra_body': {'chat_template_kwargs': {'enable_thinking': True}},
    }
}
```

## Tool Configuration

### function_list Formats

| Type | Format | Description |
|------|--------|-------------|
| `str` | `"code_interpreter"` | Pre-registered tool name |
| `dict` | `{"name": "weather", "api_key": "..."}` | Tool configuration |
| `dict` (MCP) | `{"mcpServers": {...}}` | MCP server configuration |
| `BaseTool` | `CustomTool()` | Direct tool instance |

### MCP Configuration Example

```python
{
    "mcpServers": {
        "time": {
            "command": "uvx",
            "args": ["mcp-server-time", "--local-timezone=Asia/Shanghai"]
        },
        "fetch": {
            "command": "uvx",
            "args": ["mcp-server-fetch"]
        }
    }
}
```

### Full Example

```python
tools = [
    "code_interpreter",
    {"name": "weather", "api_key": "your_key"},
    {
        "mcpServers": {
            "time": {"command": "uvx", "args": ["mcp-server-time"]},
            "file": {"command": "uvx", "args": ["mcp-server-filesystem"]}
        }
    }
]

bot = Assistant(llm=llm_cfg, function_list=tools)
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| `ValueError: Tool xxx is not registered` | Unknown tool name | Use registered name or MCP/BaseTool |
| MCP server fails | Incorrect command/args | Verify terminal execution; install via `uvx` |
