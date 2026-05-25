# Context Management API

## Overview

Automatic context truncation maintaining dialogue structure within model limits.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent Context Management API
  version: 2.0.0
```

## Trigger Conditions

- Automatically activated when `agent.run(...)` or `llm.call(...)` is invoked
- Modifies context only when length reaches `max_input_tokens` threshold

## Management Strategy

| Step | Action | Priority |
|------|--------|----------|
| **S1** | Remove oldest complete turn if still exceeds limit | High |
| **S2** | Fold (compress/summarize) oldest tool-responses | Medium-High |
| **S3** | Remove oldest tool-call step (excluding user query/final response) | Medium |
| **S4** | Fold most recent step's tool-response progressively | Medium-Low |
| **S5** | Truncate user query or final response | Low |

## Configuration

```json
{
  "max_input_tokens": 90000
}
```

## Visual Flow

```
┌─────────────────────────────────────┐
│ Context Length Check                │
└──────────────┬──────────────────────┘
               │ Exceeds limit?
               ├─────────────► Yes ──► S1: Remove oldest turn
               │                       │
               │                       ▼
               │                   Still exceeds?
               │                       │
               ├─────────────► Yes ──► S2: Fold tool-responses
               │                       │
               │                       ▼
               │                   Still exceeds?
               │                       │
               ├─────────────► Yes ──► S3: Remove tool-call steps
               │                       │
               │                       ▼
               │                   Still exceeds?
               │                       │
               ├─────────────► Yes ──► S4: Fold recent tool-response
               │                       │
               │                       ▼
               │                   Still exceeds?
               │                       │
               └─────────────► Yes ──► S5: Truncate query/response
```

## Limitations

- Memory recording suboptimal with infinite context strategy
- Advanced context memory module planned for future releases
