# Qwen-Agent API Reference

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent Core API
  version: 2.0.0
  description: |
    Framework for LLM application development with:
    - Instruction following
    - Tool usage
    - Planning
    - Memory capabilities
```

## Quick Start

```python
from qwen_agent.agents import Assistant
from qwen_agent.gui import WebUI

# Configuration
llm_cfg = {'model': 'qwen3-max', 'model_type': 'qwen_dashscope'}

# Tools specification
tools = [
    {'mcpServers': {'time': {'command': 'uvx', 'args': ['mcp-server-time']}}},
    'image_gen'
]

# Agent instantiation
bot = Assistant(
    llm=llm_cfg,
    function_list=tools,
    name='Qwen3 Tool-calling Demo'
)

# Execution
messages = [{'role': 'user', 'content': 'draw a cute dog'}]
for response in bot.run(messages=messages):
    print(response)
```

## Related Endpoints

- [`/guide/get_started/install`](./get_started/install.md) - Installation procedures
- [`/guide/get_started/quickstart`](./get_started/quickstart.md) - Quick start guide
- [`/guide/get_started/features`](./get_started/features.md) - Feature enumeration
- [`/guide/get_started/configuration`](./get_started/configuration.md) - Configuration schema
