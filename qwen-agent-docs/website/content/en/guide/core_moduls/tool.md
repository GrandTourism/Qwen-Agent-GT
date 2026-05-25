# Tool API

## Overview

Standardized interface for tool execution and registration.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent Tool API
  version: 2.0.0
```

## Interface Specification

### Base Class

```python
class BaseTool:
    name: str
    description: str
    parameters: dict
    
    def call(self, params: str, **kwargs) -> Union[str, list, dict]:
        """Execute tool with given parameters"""
```

### Registration Decorator

```python
def register_tool(name: str):
    """Register tool in TOOL_REGISTRY"""
```

## Usage

### Direct Invocation

```python
from qwen_agent.tools import ImageGen

tool = ImageGen()
result = tool.call(params={'prompt': 'a cute cat'})
print(result)  # Returns: str | list | dict
```

### Agent Integration

Tools passed via `function_list` parameter:

| Input Type | Example | Description |
|------------|---------|-------------|
| `str` | `'code_interpreter'` | Pre-registered tool name |
| `dict` | `{'name': 'weather', 'api_key': '...'}` | Tool configuration |
| `BaseTool` | `CodeInterpreter()` | Tool instance |

## Custom Tool Development

### Using Decorator

```python
from qwen_agent.tools.base import BaseTool, register_tool

@register_tool('my_image_gen')
class MyImageGen(BaseTool):
    description = 'AI image generation service'
    parameters = {
        'type': 'object',
        'properties': {
            'prompt': {
                'type': 'string',
                'description': 'Image description in English'
            }
        },
        'required': ['prompt']
    }
    
    def call(self, params: str, **kwargs) -> str:
        prompt = json5.loads(params)['prompt']
        return json.dumps({'image_url': f'https://api.example.com/{prompt}'})
```

### Direct Class Definition

```python
from qwen_agent.tools.base import BaseTool

class MyImageGen(BaseTool):
    name = 'my_image_gen'
    description = 'AI image generation service'
    parameters = {...}
    
    def call(self, params: str, **kwargs) -> str:
        # Implementation
        pass
```

## Built-in Tools

| Tool | Description |
|------|-------------|
| `code_interpreter` | Python code execution in Docker sandbox |
| `web_search` | Web search queries |
| `web_extractor` | Web page content extraction |
| `image_search` | Reverse image search |
| `image_zoom_in_tool` | Image region cropping |
| `retrieval` | RAG-based document retrieval |
