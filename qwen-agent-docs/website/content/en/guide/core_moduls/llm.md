# LLM API

## Overview

Unified interface for LLM access with function calling support.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent LLM API
  version: 2.0.0
```

## Interface

### Factory Function

```python
def get_chat_model(cfg: Optional[Dict] = None) -> BaseChatModel:
    """Get LLM instance from configuration"""
```

### Configuration Schema

```json
{
  "type": "object",
  "required": ["model"],
  "properties": {
    "model": {"type": "string", "description": "Model name"},
    "model_type": {"type": "string", "description": "Provider identifier"},
    "model_server": {"type": "string", "description": "API endpoint URL"},
    "api_key": {"type": "string", "description": "Authentication key"},
    "generate_cfg": {"type": "object", "description": "Generation parameters"}
  }
}
```

### Chat Method

```python
def chat(messages: List[Message], functions: Optional[List[Dict]] = None, stream: bool = True) -> Iterator[List[Message]]:
    """Generate responses with optional function calling"""
```

## Usage Example

```python
from qwen_agent.llm import get_chat_model

llm_cfg = {
    'model': 'qwen-max',
    'model_server': 'dashscope',
    'generate_cfg': {'top_p': 0.8}
}

llm = get_chat_model(llm_cfg)

functions = [{
    'name': 'get_current_weather',
    'description': 'Get weather for location',
    'parameters': {
        'type': 'object',
        'properties': {
            'location': {'type': 'string'}
        },
        'required': ['location']
    }
}]

for responses in llm.chat(
    messages=[{'role': 'user', 'content': "Weather in San Francisco?"}],
    functions=functions,
    stream=True
):
    print(responses)
```

## Supported Providers

| Provider | Model Type | Capabilities |
|----------|-----------|--------------|
| `qwen_dashscope` | LLM | Text → Text |
| `qwenvl_dashscope` | VLM | Text/Image/Video → Text |
| `qwenaudio_dashscope` | Omni | Text/Image/Video/Audio → Text |
| `oai` | LLM | OpenAI-compatible |
| `qwenvl_oai` | VLM | OpenAI-compatible VLM |
| `qwenaudio_oai` | Omni | OpenAI-compatible Omni |

## Custom LLM Registration

New LLMs must implement:
1. Non-streaming generation
2. Streaming generation
3. Function call interface

Inherit from `BaseFnCallModel` for automatic function calling support.
