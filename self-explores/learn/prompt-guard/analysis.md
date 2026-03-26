---
date: 2026-03-26
type: skill-analysis
skill: prompt-guard
category: 07-safety-alignment
score: 4.2/5
tags: [Safety Alignment, Prompt Injection, Jailbreak Detection, Meta, Input Validation]
---

# Prompt Guard

## Đánh giá (Score 4.2/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 3.5/5 | 20% | 0.70 |
| Safety | 3.5/5 | 20% | 0.70 |
| **Weighted Total** | | | **4.10/5** |

**Summary:** Prompt Guard is the most immediately runnable skill in the safety-alignment category. The code is self-contained and copy-pasteable — load the HuggingFace model, call `softmax`, read the label index. Three workflows (user input filtering, third-party data filtering, batch RAG filtering) cover the primary deployment patterns precisely. The threshold table (High Security 0.3 / Balanced 0.5 / Low Friction 0.7) with corresponding TPR/FPR values is an exceptional piece of practical guidance rarely found in skill files. Weaknesses center on the 512-token limit — the sliding window fix is provided but not integrated into the main workflows.

## Ưu điểm
- **Immediately executable code**: The basic usage block is a complete, working Python script — model loading, tokenization, softmax, label indexing — with zero placeholder functions.
- **Three-class label taxonomy explained**: BENIGN (0) / INJECTION (1) / JAILBREAK (2) distinction is clearly explained, with the crucial insight that third-party data should sum labels 1+2 (not just label 2).
- **Threshold recommendation table**: The High Security (0.3) / Balanced (0.5) / Low Friction (0.7) table with actual TPR/FPR numbers is production-grade guidance that directly answers the most common deployment question.
- **Defense-in-depth pattern**: The layered architecture (Prompt Guard → LlamaGuard → LLM → LlamaGuard output check) shows how to compose multiple safety tools — a pattern rarely documented explicitly.
- **Batch processing for RAG**: Workflow 3 provides a proper batched implementation with configurable `batch_size` and returns `(doc, score, is_safe)` tuples — directly usable in a RAG pipeline.

## Nhược điểm
- **512-token truncation is a first-class problem, not an issue**: The sliding window solution is in the "Common issues" section but should be the default implementation for any real deployment where inputs may be long. RAG documents routinely exceed 512 tokens.
- **No fine-tuning guidance**: The model is presented as a fixed classifier but Meta's release includes fine-tuning recipes. If Prompt Guard's in-distribution performance (99.7% TPR) degrades on a user's specific domain, there is no guidance on adaptation.
- **False positive handling in the main workflows is insufficient**: Only the "Common issues" section mentions context-aware filtering with `user_is_trusted`. Production deployments need reputation/role-based thresholding as a first-class feature, not an afterthought.
- **Llama 3.1 Community License restriction not flagged prominently**: The license section is buried in resources. Commercial use restrictions could be a blocker for some users — this should appear in the YAML dependencies or a warning box.
- **No async/streaming support shown**: All examples are synchronous `torch.no_grad()` calls. Production APIs typically need async wrappers or integration with FastAPI/aiohttp, which is not covered.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User asks: "How do I detect prompt injection attacks?"
- User is building a RAG pipeline with `15-rag/` skills and asks about securing retrieved documents
- User mentions jailbreak filtering, input sanitization, or LLM input validation
- User has a chatbot or API and wants lightweight (<2ms) security without adding a full guardrails framework
- User asks about multilingual safety (Prompt Guard supports 8 languages)

### Integration points
- **Pairs with `15-rag/chroma/`, `15-rag/faiss/`, `15-rag/qdrant/`**: The batch filtering workflow (Workflow 3) slots directly into the retrieval step — filter documents before injecting them into the context window
- **Pairs with `14-agents/claude-agent-sdk/`**: Agent tools that call external APIs or scrape web content should route results through Prompt Guard's `get_indirect_injection_score` before adding to agent context
- **Pairs with `07-safety-alignment/nemo-guardrails/`**: Prompt Guard is a lightweight first layer; NeMo Guardrails handles policy-based response control. Prompt Guard at <2ms GPU vs NeMo's 100-500ms overhead makes the tiering decision obvious.
- **Pairs with `07-safety-alignment/llama-guard/`**: Prompt Guard covers jailbreak/injection (input security); LlamaGuard covers content categories (violence, hate, CSAM). Together they form a complete input+content moderation stack.

### Priority
- **High** — Prompt injection is one of the most prevalent real-world attack vectors for LLM applications, especially RAG systems. At 86M parameters with <2ms GPU latency, Prompt Guard has essentially zero overhead cost. Any production deployment skill-pairing involving RAG or external data should mention this skill.

## Ghi chú kỹ thuật
- **Dependencies:** transformers, torch (no additional dependencies — very lightweight)
- **Line count:** 313 lines (slightly over 300 but well within 500 target)
- **Known issues:** 512-token hard limit — injections placed at position 513+ in a document are invisible to the model without the sliding window workaround; Llama 3.1 Community License prohibits certain commercial uses without agreement
- **Alternatives:** LlamaGuard (broader content categories, not injection-specific), NeMo Guardrails (policy-based full framework), Rebuff (open-source prompt injection detector, less performant), Lakera Guard (commercial, API-based)
- **Hardware note:** FP16 weights are only 550MB — easily fits on any inference GPU alongside the main LLM model
- **Performance:** 99.7% TPR / 0.6% FPR in-distribution; degrades to 97.5% TPR / 3.9% FPR on out-of-distribution attacks — reasonable for most deployments
