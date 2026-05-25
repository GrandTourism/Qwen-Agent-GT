# Installation

## PyPI Package

```bash
pip install -U "qwen-agent[gui,rag,code_interpreter,mcp]"
```

### Optional Dependencies

| Feature | Package Extra | Description |
|---------|--------------|-------------|
| GUI | `[gui]` | Gradio-based web interface |
| RAG | `[rag]` | Retrieval-Augmented Generation |
| Code Interpreter | `[code_interpreter]` | Python code execution sandbox |
| MCP | `[mcp]` | Model Context Protocol support |

## Development Installation

```bash
git clone https://github.com/QwenLM/Qwen-Agent.git
cd Qwen-Agent
pip install -e ./\"[gui,rag,code_interpreter,mcp]\"
```

## Requirements

- Python >= 3.10 (for GUI support)
- Docker (for code interpreter sandbox)
