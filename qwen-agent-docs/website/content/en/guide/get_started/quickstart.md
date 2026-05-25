# Quick Start

## Prerequisites

### Model Service Options

| Provider | Setup | Environment Variable |
|----------|-------|---------------------|
| DashScope | Use Alibaba Cloud API | `DASHSCOPE_API_KEY` |
| vLLM/SGLang | Deploy open-source Qwen models | — |

### Tool Call Parser Configuration

| Option | Description | Recommended For |
|--------|-------------|-----------------|
| **Model Server Native** | Enable `--enable-auto-tool-choice` and `--tool-call-parser qwen3_coder` in vLLM/SGLang | Qwen3-Coder with `use_raw_api=True` |
| **Qwen-Agent Built-in** | Default `hermes` parsing format | QwQ, Qwen3 series (default) |

## Agent Development Workflow

### Step 1: Define Custom Tool (Optional)

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
        return json5.dumps({'image_url': f'https://image.pollinations.ai/prompt/{prompt}'})
```

### Step 2: Configure LLM

```python
llm_cfg = {
    'model': 'qwen-max-latest',
    'model_type': 'qwen_dashscope',
    'generate_cfg': {'top_p': 0.8}
}
```

### Step 3: Create Agent

```python
from qwen_agent.agents import Assistant

system_instruction = '''Draw an image, download it, then process it using document operations.'''
tools = ['my_image_gen', 'code_interpreter']
files = ['./examples/resource/doc.pdf']

bot = Assistant(
    llm=llm_cfg,
    system_message=system_instruction,
    function_list=tools,
    files=files
)
```

### Step 4: Execute

```python
messages = []
while True:
    query = input('user query: ')
    messages.append({'role': 'user', 'content': query})
    
    response_plain_text = ''
    for response in bot.run(messages=messages):
        response_plain_text = typewriter_print(response, response_plain_text)
    
    messages.extend(response)
```

### Optional: Launch GUI

```python
from qwen_agent.gui import WebUI
WebUI(bot).run()
```

## Component Architecture

| Level | Class | Purpose |
|-------|-------|---------|
| Atomic | `BaseChatModel` | LLM with function calling |
| Atomic | `BaseTool` | Tool interface |
| High-level | `Agent` | Message processing workflow |
