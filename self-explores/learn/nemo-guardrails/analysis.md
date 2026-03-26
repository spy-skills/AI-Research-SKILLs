---
date: 2026-03-26
type: skill-analysis
skill: nemo-guardrails
category: 07-safety-alignment
score: 4.2/5
tags: [Safety Alignment, NeMo Guardrails, NVIDIA, Jailbreak Detection, Guardrails]
---

# NeMo Guardrails

## Đánh giá (Score 4.2/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 3.5/5 | 20% | 0.70 |
| Safety | 4.0/5 | 20% | 0.80 |
| **Weighted Total** | | | **4.20/5** |

**Summary:** NeMo Guardrails skill is among the most practically complete in the safety-alignment category. Five distinct workflows — jailbreak detection, self-check I/O, fact-checking, PII filtering, and LlamaGuard integration — cover the full spectrum of production runtime safety concerns. The Colang 2.0 DSL snippets are realistic and runnable, and the latency table (pattern matching <1ms through LlamaGuard 100-300ms) gives engineers concrete expectations. The main gap is that Colang 2.0 syntax is used throughout without a brief introduction to its semantics, creating a steep on-ramp for first-time users.

## Ưu điểm
- **Breadth of safety mechanisms**: Five distinct workflows cover jailbreak, input/output validation, fact-checking, PII masking, and LlamaGuard integration — the widest safety coverage of any single skill in the library.
- **Concrete Colang DSL examples**: Every workflow shows real Colang flow definitions with `define user`, `define bot`, `define flow`, `parallel:` patterns — not pseudocode.
- **Latency quantification**: The hardware section gives per-mechanism latency breakdowns (pattern matching <1ms, LLM-based 50-200ms, LlamaGuard 100-300ms) critical for production SLA planning.
- **Integration ecosystem**: Demonstrates integration with Presidio (PII), ActiveFence (toxicity), and LlamaGuard (moderation) — shows the composable architecture.
- **Parallel checks pattern**: The `parallel:` Colang snippet for running toxicity + jailbreak + PII simultaneously is a key performance optimization not commonly documented.

## Nhược điểm
- **No Colang syntax primer**: The skill jumps into `define user ask about illegal activity` and `define flow` patterns without explaining the Colang 2.0 language model (variables, flow control, action invocation). Users unfamiliar with Colang will be confused before reaching Workflow 2.
- **Placeholder action bodies**: `check_input_toxicity` calls `toxicity_detector(user_message)` without specifying which detector — the function is undefined. Same for `verify_facts()` in the fact-checking workflow.
- **Version drift risk**: References "v0.9.0+ (v0.12.0 expected)" — NeMo Guardrails has had multiple breaking API changes; the Colang 2.0 DSL was itself a breaking change from Colang 1.0, and the skill does not clarify which version each snippet targets.
- **No conversation state management**: Does not explain how guardrails interact with multi-turn conversations or how context is preserved across turns — critical for chatbot deployments.
- **Missing LLM provider configuration**: The fact-checking workflow hardcodes `gpt-4` but does not explain how to configure local models (Ollama, vLLM) as the main engine, which is required for air-gapped deployments.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User is building an LLM application (chatbot, RAG system, API wrapper) and asks about safety or content filtering at runtime
- User asks: "How do I prevent jailbreaks in my app?"
- User mentions PII protection, GDPR compliance, or hallucination detection
- User is deploying with `14-agents/` skills and asks how to add guardrails to agents
- User asks about production safety without wanting to retrain the model

### Integration points
- **Pairs with `12-inference-serving/vllm/` or `12-inference-serving/sglang/`**: NeMo Guardrails wraps the LLM — it sits in front of any inference backend
- **Pairs with `15-rag/` skills**: Workflow 4 (PII) and Workflow 3 (fact-checking) directly address RAG-specific injection and hallucination risks
- **Pairs with `17-observability/langsmith/`**: Rail trigger events and blocked queries should be logged to LangSmith for audit trails
- **Complements `constitutional-ai/`**: CAI handles training-time alignment; NeMo Guardrails handles runtime enforcement — use both in a full safety pipeline

### Priority
- **High** — Production LLM deployments almost always require runtime safety layers. NeMo Guardrails is the most feature-complete open-source option covering jailbreak, PII, hallucination, and toxicity in a single framework. Will be frequently needed when users move from model training to application deployment.

## Ghi chú kỹ thuật
- **Dependencies:** nemoguardrails (+ nemoguardrails[eval] for evaluation, microsoft-presidio for PII)
- **Line count:** 298 lines (within target range)
- **Known issues:** Colang 2.0 broke backward compatibility with Colang 1.0 examples; `LlamaGuard` import path from `nemoguardrails.integrations` may not exist in all versions — verify against actual installed version
- **Alternatives:** LlamaGuard (standalone moderation model), OpenAI Moderation API (simple but cloud-only), Perspective API (toxicity only), Guardrails AI (competitor framework with different DSL approach)
- **Star count:** 4,300+ GitHub stars as of skill authoring date — active and maintained project
