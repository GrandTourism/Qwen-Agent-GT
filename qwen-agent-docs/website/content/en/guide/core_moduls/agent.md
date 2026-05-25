# Agent API

## Overview

Higher-level interface integrating tool calls and LLM for message processing.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent Agent API
  version: 2.0.0
```

## Interface Specification

### Input/Output

- **Input**: `List[Message]` - Message list
- **Output**: `Iterator[List[Message]]` - Stream of message lists

### Base Class

```python
class Agent:
    def _run(self, messages: List[Message], lang: str = 'en', **kwargs) -> Iterator[List[Message]]:
        """Process messages and yield response streams"""
```

## Built-in Agent Types

### Assistant

| Feature | Description |
|---------|-------------|
| Role-playing | Custom system instructions |
| Tool calling | Automatic planning and execution |
| RAG | Document parsing with configurable strategies |

**Example:**

```python
from qwen_agent.agents import Assistant

bot = Assistant(
    llm={'model': 'qwen-max'},
    system_message='Assistant role definition',
    function_list=['image_gen', 'code_interpreter'],
    files=['doc.pdf']
)

for response in bot.run(messages=[{'role': 'user', 'content': 'a cute cat'}]):
    print(response)
```

### GroupChat

Multi-agent coordination with speech order management.

**Features:**
- Automatic agent turn management
- Human-in-the-loop support
- Interruptible execution

## Custom Agent Development

### Nested Development

```python
class VisualStorytelling(Agent):
    def __init__(self, function_list=None, llm=None):
        super().__init__(llm=llm)
        self.image_agent = Assistant(llm={'model': 'qwen-vl-max'})
        self.writing_agent = Assistant(llm=self.llm, function_list=function_list)
    
    def _run(self, messages: List[Message], lang: str = 'zh', **kwargs) -> Iterator[List[Message]]:
        # Image understanding
        for rsp in self.image_agent.run(messages):
            yield rsp
        
        # Story writing
        for rsp in self.writing_agent.run(messages, lang=lang, **kwargs):
            yield rsp
```

### Non-nested Development

```python
class DocQA(Agent):
    def _run(self, messages: List[Message], knowledge: str = '', lang: str = 'en', **kwargs) -> Iterator[List[Message]]:
        system_prompt = f"Reference: {knowledge}"
        messages.insert(0, Message('system', system_prompt))
        return self._call_llm(messages=messages)
```

## Available Methods

| Method | Description |
|--------|-------------|
| `_call_llm(...)` | Invoke LLM with streaming |
| `_call_tool(...)` | Execute tools and return string results |
| `run(...)` | Main entry point for agent execution |
