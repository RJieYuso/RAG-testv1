# Document RAG Q&A System

A Retrieval-Augmented Generation (RAG) pipeline that answers questions about uploaded PDF documents, built with LlamaIndex and OpenRouter.

## Overview

This project demonstrates an end-to-end RAG workflow: PDF documents are parsed, chunked, embedded into a vector index, and queried using semantic similarity search. Retrieved context is passed to a large language model to generate grounded, citation-based answers.

## Features

- **PDF Ingestion**: Automatic parsing and text extraction using `pypdf` / `llama-index-readers-file`
- **Semantic Search**: Vector-based retrieval using `BAAI/bge-small-en-v1.5` sentence embeddings (runs locally, no API cost)
- **LLM Integration**: Answer generation via OpenRouter API (configurable model, e.g. `openai/gpt-4o-mini`)
- **Retrieval Diagnostics**: Built-in checks to verify document parsing quality and inspect retrieved chunks before generation
- **Grounded Prompting**: Custom prompt template enforces citation of specific facts and explicit "not mentioned" responses to reduce hallucination

## Tech Stack

| Component | Tool |
|---|---|
| Orchestration | LlamaIndex |
| Embeddings | HuggingFace (`bge-small-en-v1.5`) |
| LLM | OpenRouter (GPT-4o-mini) |
| PDF Parsing | pypdf |
| Environment | Google Colab |

## How It Works

1. PDF files are loaded from a local `data/` folder
2. Documents are chunked and embedded into a `VectorStoreIndex`
3. A retriever pulls the top-k most relevant chunks for a given query (`similarity_top_k=6`)
4. A structured prompt instructs the LLM to answer strictly based on retrieved context, citing specific details and avoiding vague language
5. Diagnostic output shows chunk count, character count, and retrieved passage previews for debugging

## Setup

```bash
pip install llama-index llama-index-llms-openrouter llama-index-embeddings-huggingface pypdf llama-index-readers-file
```

Set your OpenRouter API key as a Colab secret (`OPENROUTER_API_KEY`), or as an environment variable if running locally.

## Example Output

Given a project report PDF, the system correctly extracts and summarizes:
- Project pain points
- Core technologies used
- Team member names and IDs

with citations grounded in the actual document content rather than generic placeholder text.

## Relevant Skills Demonstrated

Retrieval-Augmented Generation (RAG) · Vector search / semantic search · Prompt engineering for grounded responses · LLM API integration

## Future Improvements

- [ ] Add OCR support for scanned/image-based PDFs
- [ ] Swap in-memory vector store for a persistent vector database (e.g. Chroma, Pinecone)
- [ ] Extend to multi-step agentic retrieval (query decomposition, re-ranking)
- [ ] Add evaluation metrics for answer faithfulness
