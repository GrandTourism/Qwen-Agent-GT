# Schema API

## Overview

Type-safe messaging system for multimodal conversations, function calling, and reasoning chains.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent Schema API
  version: 2.0.0
```

## Core Types

### Message Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Message",
  "type": "object",
  "required": ["role", "content"],
  "properties": {
    "role": {"type": "string", "enum": ["system", "user", "assistant", "function"]},
    "content": {"oneOf": [{"type": "string"}, {"type": "array", "items": {"$ref": "#/$defs/ContentItem"}}]},
    "reasoning_content": {"oneOf": [{"type": "string"}, {"type": "array", "items": {"$ref": "#/$defs/ContentItem"}}]},
    "name": {"type": "string"},
    "function_call": {"$ref": "#/$defs/FunctionCall"},
    "extra": {"type": "object"}
  },
  "$defs": {
    "ContentItem": {
      "type": "object",
      "properties": {
        "text": {"type": "string"},
        "image": {"type": "string"},
        "file": {"type": "string"},
        "audio": {"oneOf": [{"type": "string"}, {"type": "object"}]},
        "video": {"oneOf": [{"type": "string"}, {"type": "array"}]}
      }
    },
    "FunctionCall": {
      "type": "object",
      "required": ["name", "arguments"],
      "properties": {
        "name": {"type": "string"},
        "arguments": {"type": "string"}
      }
    }
  }
}
```

### Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `SYSTEM` | `"system"` | System role identifier |
| `USER` | `"user"` | User role identifier |
| `ASSISTANT` | `"assistant"` | Assistant role identifier |
| `FUNCTION` | `"function"` | Function role identifier |
| `TEXT` | `"text"` | Text content type |
| `IMAGE` | `"image"` | Image content type |
| `FILE` | `"file"` | File content type |
| `AUDIO` | `"audio"` | Audio content type |
| `VIDEO` | `"video"` | Video content type |

## Usage Examples

### Plain Text Message

```python
msg = Message(role='user', content='What is the weather in Tokyo?')
```

### Multimodal Input

```python
content = [
    ContentItem(text='Describe this image:'),
    ContentItem(image='https://example.com/cat.jpg')
]
msg = Message(role='user', content=content)
```

### Function Call

```python
msg = Message(
    role='assistant',
    content='',
    function_call=FunctionCall(name='get_weather', arguments='{"city": "Tokyo"}')
)
```
