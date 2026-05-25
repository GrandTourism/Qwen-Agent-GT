# MCP (Model Context Protocol) API

## Overview

Standardized protocol for LLM interaction with external tools and services.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent MCP API
  version: 2.0.0
```

## Prerequisites

### System Requirements

| Component | Version | Purpose |
|-----------|---------|---------|
| Node.js | Latest LTS | MCP server runtime |
| uv | >= 0.4.18 | Python-based MCP servers |
| Git | Any | Version control |
| SQLite | Any | Database operations |

### Installation

```bash
pip install -U "qwen-agent[mcp]"
```

## Configuration Schema

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/files"]
    },
    "sqlite": {
      "command": "uvx",
      "args": ["mcp-server-sqlite", "--db-path", "test.db"]
    }
  }
}
```

## Usage

```python
from qwen_agent.agents import Assistant

mcp_config = {
    "mcpServers": {
        "filesystem": {
            "command": "npx",
            "args": ["-y", "@modelcontextprotocol/server-filesystem", "./workspace"]
        },
        "memory": {
            "command": "npx",
            "args": ["-y", "@modelcontextprotocol/server-memory"]
        }
    }
}

agent = Assistant(
    llm={'model': 'qwen3-max', 'model_type': 'qwen_dashscope'},
    system_message="Intelligent assistant with file access and memory",
    function_list=[mcp_config]
)
```

## Use Cases

| Use Case | Description | MCP Server |
|----------|-------------|------------|
| File I/O | Read/write local files | `filesystem` |
| Memory | Store/retrieve preferences | `memory` |
| Database | Execute SQL queries | `sqlite` |

## Security Considerations

1. **Sandbox Limitations**: MCP services may not be sandboxed - use only in trusted environments
2. **Path Restrictions**: Filesystem access limited to explicitly allowed directories
3. **Production Warning**: Not recommended for production deployment

## References

- [Official MCP Servers](https://github.com/modelcontextprotocol/servers)
- [SQLite Bot Example](https://github.com/QwenLM/Qwen-Agent/blob/main/examples/assistant_mcp_sqlite_bot.py)
