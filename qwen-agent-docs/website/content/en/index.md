# Qwen Agent API Specification

```yaml
openapi: 3.0.3
info:
  title: Qwen Agent Framework
  version: 2.0.0
  description: |
    AI agent framework for building LLM applications with:
    - Instruction following
    - Tool usage
    - Planning capabilities
    - Memory management
tags:
  - name: guide
    description: Core API documentation
  - name: benchmarks
    description: Performance evaluation metrics
```

## API Endpoints

### Guide API
- **Path**: `/guide/`
- **Methods**: [`GET`](./guide/)
- **Capabilities**:
  - Installation configuration
  - Feature enumeration
  - Parameter specification
  - Module interfaces
  - Development patterns

### Benchmarks API
- **Path**: `/benchmarks/`
- **Methods**: [`GET`](./benchmarks/)
- **Metrics**:
  - `DeepPlanning`: Multi-step planning evaluation
    - `TravelPlanning`: Itinerary generation with constraints
    - `ShoppingPlanning`: Budget optimization with preferences

## Schema Reference

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "QwenAgent",
  "type": "object",
  "properties": {
    "framework": {"type": "string", "const": "qwen-agent"},
    "version": {"type": "string"},
    "capabilities": {
      "type": "array",
      "items": {"type": "string", "enum": ["instruction_following", "tool_usage", "planning", "memory"]}
    }
  }
}
```
