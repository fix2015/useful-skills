---
name: rag-engine
description: >
  Agentic RAG framework for Node.js — zero runtime deps, auto-retries with query
  rewriting, relevance judging, full decision trace. 5-line quickstart.
tags:
  - rag
  - ai
  - llm
  - embeddings
  - vector-search
  - agentic
  - nodejs
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# RAG Engine — Skill

Agentic RAG framework for Node.js. Not basic retrieve-and-generate — the agent judges relevance, rewrites queries, retries, and gives up honestly.

Use when: user asks to build RAG, add document Q&A, create a knowledge base, search documents with AI, or implement retrieval-augmented generation in Node.js/TypeScript.

---

## 5-Line Quickstart

```javascript
import { RagEngine } from 'rag-engine'

const rag = await RagEngine.create()
await rag.ingest('./docs')
const result = await rag.query('How does auth work?')
console.log(result.answer)
```

Auto-detects `OPENAI_API_KEY` from env. Zero config needed.

---

## Install

```bash
npm install rag-engine
```

---

## How the Agent Thinks

After every retrieval, an LLM judge scores how well the chunks answer the question:

| Decision | When | What happens |
|----------|------|-------------|
| SYNTHESIZE | Relevance >= 0.7 | Generate answer from chunks |
| REWRITE | Relevance 0.3-0.7 | Rewrite query, retry search |
| BROADEN | < 3 results | Broaden query, retry |
| GIVE_UP | Max retries or < 0.3 | Honestly say "I don't know" |

---

## Ingest Documents

```javascript
await rag.ingest('./docs')                           // all text files
await rag.ingest('./src', { glob: '**/*.ts' })       // TypeScript only
await rag.ingest('./README.md')                      // single file
await rag.ingest('Raw text content to index')        // raw string
```

---

## Query with Full Trace

```javascript
const result = await rag.query('What is the refund policy?')

result.answer    // "Returns allowed within 30 days..."
result.sources   // [{ id, content, score, metadata }]
result.trace     // [{ action: "search" }, { action: "evaluate", score: 0.89 }, ...]
result.metrics   // { totalTimeMs, retrievalTimeMs, llmCalls }
```

---

## Custom Config

```javascript
const rag = await RagEngine.create({
  llm: { provider: 'openai', model: 'gpt-4o', temperature: 0.1 },
  agent: { maxRetries: 3, relevanceThreshold: 0.7 },
  chunker: { maxTokens: 512, overlap: 50 },
  retrieval: { topK: 10 },
})
```

---

## CLI

```bash
npx rag-engine ingest ./docs
npx rag-engine query "How does authentication work?"
npx rag-engine stats
```

---

## Express.js Integration

```javascript
import express from 'express'
import { RagEngine } from 'rag-engine'

const app = express()
const rag = await RagEngine.create()
await rag.ingest('./docs')

app.use(express.json())
app.post('/ask', async (req, res) => {
  const result = await rag.query(req.body.question)
  res.json(result)
})
app.listen(3000)
```
