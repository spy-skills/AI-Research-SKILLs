---
date: 2026-03-26
type: skill-analysis
skill: llamaindex
category: 14-agents
score: 4.4/5
tags: [Agents, LlamaIndex, RAG, Document Ingestion, Vector Indices]
---

# LlamaIndex - Data Framework for LLM Applications

## Đánh giá (Score 4.4/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 5/5 | 30% | 1.50 |
| Usability | 4/5 | 30% | 1.20 |
| Modularity | 5/5 | 20% | 1.00 |
| Safety | 3/5 | 20% | 0.60 |
| **Weighted Total** | | | **4.30/5** |

**Summary:** The LlamaIndex skill has the broadest surface coverage in the library — it addresses data connectors, indices, query engines, retrievers, agents, chat engines, vector store integrations (Chroma, Pinecone, FAISS), multi-modal RAG, and evaluation in a single SKILL.md. The LlamaIndex vs LangChain comparison table is an exceptionally clear when-to-use guide. However, at 570 lines it significantly exceeds the target, and the breadth comes at the cost of depth — advanced topics like hybrid retrieval, re-ranking, and production observability are absent.

## Ưu điểm

- **LlamaIndex vs LangChain comparison table** (Best for, Data connectors, RAG focus, Learning curve, Customization, Documentation) is the clearest framework selection guide in the entire skills library. It gives an AI agent an unambiguous routing decision without requiring human judgment.
- **Three vector store integrations with complete code** (Chroma local, Pinecone cloud, FAISS fast) cover the most common production deployment targets. The pattern of `StorageContext.from_defaults(vector_store=...)` is consistent across all three, reducing the learning curve for switching backends.
- **10 best practices at the end** (persist index to disk, chunk 512-1024 tokens, add metadata, enable streaming, evaluate responses) represent a condensed production checklist. The "save indices to disk" guidance with explicit `index.storage_context.persist()` prevents a common mistake where expensive re-indexing runs on every restart.
- **Evaluation workflow** (`RelevancyEvaluator`, `FaithfulnessEvaluator`) is one of the few skills in the library that includes automated quality measurement. The faithfulness evaluator (hallucination detection) is particularly valuable for production RAG systems.
- **Multi-modal RAG section** demonstrates loading `.jpg`, `.png`, `.pdf` in a single `SimpleDirectoryReader` call with `OpenAIMultiModal` — enabling image-document combined retrieval without any custom preprocessing code.

## Nhược điểm

- **Safety score reduced: no guidance on data privacy or PII handling**: For a skill explicitly targeting "private data" RAG use cases (enterprise data, internal docs), there is zero mention of PII detection, data retention policies, or secure index storage. This is a significant gap for production deployment.
- **570 lines exceeds the 500-line limit**: The skill tries to cover everything, diluting depth. The multi-modal, evaluation, and database connector sections each deserve their own reference file. The current structure makes it hard to find the core workflow quickly.
- **Agent section uses `FunctionAgent.from_tools()` but doesn't explain tool calling**: The code passes raw Python functions as tools, but the actual function-calling mechanism (how LlamaIndex converts them to tool schemas) is not explained. This creates confusion for non-standard tool signatures.
- **`pinecone.init()` is deprecated**: The Pinecone integration code uses the legacy v2 API (`pinecone.init()`). Pinecone v3+ uses `Pinecone(api_key=...)` client. This would fail silently for new users with current Pinecone versions.
- **No chunking strategy guidance**: The best practices say "512-1024 tokens optimal" but there's no discussion of chunking strategies (sentence-window, hierarchical, semantic chunking) or when to deviate from the default `SentenceSplitter`.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User says "build a RAG system", "index my documents", "chat with my data/PDF/codebase"
- User mentions "knowledge base", "document Q&A", "enterprise data search"
- User has a directory of files (PDFs, markdowns, Word docs) and wants to query them
- autopilot detects the user wants an LLM application over private/local data

### Integration points
- **OMC autopilot flow**: When autopilot builds a knowledge retrieval application, `explore` should scan the target data directory first (file types, size, structure), then `executor` uses the appropriate `SimpleDirectoryReader` with `required_exts` to load only relevant files. The `persist_dir` pattern should be used by default to avoid re-indexing.
- **team pipeline**: `team-plan` should decide the index type (vector for semantic search, list for summarization, tree for hierarchical) based on the use case. `team-exec` builds the pipeline. `team-verify` runs the `FaithfulnessEvaluator` to check for hallucination before delivery.
- **Integration with `15-rag` category**: LlamaIndex acts as the orchestration layer over the vector stores in category 15 (Chroma, FAISS, Pinecone, Qdrant). OMC should route users to LlamaIndex for full RAG pipelines but to standalone vector store skills for pure embedding search without query engine overhead.
- **Production gap**: The skill needs a `Settings` configuration section showing how to set global LLM, embedding model, and chunk size once for the entire application. Currently users must pass settings per-query-engine, leading to inconsistent behavior across a multi-index application.

### Priority
- **Medium-High** — LlamaIndex is widely used for RAG applications, which are among the most common LLM use cases. However, the skill's breadth-over-depth tradeoff and stale Pinecone API code reduce its reliability for autonomous agent use without verification. Recommend splitting into focused sub-workflows before high-confidence autopilot use.

## Ghi chú kỹ thuật
- **Dependencies:** llama-index, openai, anthropic
- **Line count:** 570 lines (exceeds 500-line target by ~14%)
- **Known issues:** Pinecone v2 `pinecone.init()` API is deprecated; agent tool-calling mechanism underdocumented; no PII/privacy guidance for enterprise use
- **Alternatives:** LangChain (more general agents, steeper curve), Haystack (production search pipelines, less LLM-native), txtai (lightweight, minimal dependencies), direct vector store libraries (Chroma, FAISS) for simpler retrieval without query engine overhead
