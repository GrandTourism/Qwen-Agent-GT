# RAG (Retrieval-Augmented Generation) API

## Overview

Document-based retrieval system for grounding LLM responses in user-provided knowledge sources.

```yaml
openapi: 3.0.3
info:
  title: Qwen-Agent RAG API
  version: 2.0.0
```

## Core Components

### Document Parser

| Property | Value |
|----------|-------|
| Supported Formats | `.pdf`, `.docx`, `.pptx`, `.txt`, `.csv`, `.tsv`, `.xlsx`, `.xls`, `.html` |
| Chunk Size | `parser_page_size` (default: 500 tokens) |
| Output | Structured text chunks |

### Retrieval Engine

| Algorithm | Library | Strategy |
|-----------|---------|----------|
| BM25 | `rank_bm25` | Sparse keyword matching |
| Keyword Generation | LLM-based | Multilingual query decomposition |

### Configuration

```json
{
  "max_ref_token": 20000,
  "rag_searchers": ["keyword_search", "front_page_search"]
}
```

## Usage

```python
from qwen_agent.agents import Assistant
from qwen_agent.llm.schema import Message, ContentItem

agent = Assistant(
    llm={'model': 'qwen-max'},
    files=['manual.pdf']
)

response = agent.run(messages=[
    Message(role="user", content=[
        ContentItem(text="How long is the product warranty?"),
        ContentItem(file="manual.pdf")
    ])
])
```

## Processing Pipeline

1. **Parse**: Convert files to text chunks via `DocParser`
2. **Retrieve**: Match query against chunks using BM25
3. **Return**: Structured JSON via `retrieval` tool

## Dependencies

```bash
pip install "qwen-agent[rag]"
```

## Characteristics

- Lightweight retrieval without embeddings/vector databases
- Source-grounded information augmentation
- Configurable token limits for context window management
