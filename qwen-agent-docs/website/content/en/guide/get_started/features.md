# Features

## Core Capabilities

| Feature | Description | Status |
|---------|-------------|--------|
| **Unified Agent Interface** | High-level `Agent` base class with ready-to-use implementations | ✅ Stable |
| **Advanced Tool Calling** | Parallel, multi-step, multi-turn function calls | ✅ Stable |
| **RAG** | Document QA over 1M+ tokens using hybrid retrieval | ✅ Stable |
| **Built-in Tools** | `code_interpreter`, `web_search`, `image_zoom_in_tool` | ✅ Stable |
| **MCP Integration** | Model Context Protocol for external tools/services | ✅ Stable |
| **Custom Tools** | `@register_tool` decorator for user-defined tools | ✅ Stable |
| **Multi-Model Support** | Qwen3, Qwen3-VL, QwQ via DashScope/OpenAI-compatible APIs | ✅ Stable |
| **Context Management** | Automatic long-context truncation | ✅ Stable |
| **Web GUI** | Gradio 5-based interactive demos | ✅ Stable |
| **Streaming Output** | Real-time token-by-token responses | ✅ Stable |

## Supported Models

- **Qwen Series**: Qwen3, Qwen3-VL, Qwen3-Omni, Qwen3-Coder, QwQ, Qwen2.5
- **Deployment Options**:
  - Alibaba Cloud DashScope API
  - Local OpenAI-compatible servers (vLLM, SGLang, Ollama)
