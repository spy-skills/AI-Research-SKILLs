---
date: 2026-03-26
type: skill-analysis
skill: llamaguard
category: 07-safety-alignment
score: 4.5/5
tags: [Safety Alignment, LlamaGuard, Content Moderation, Meta, Guardrails]
---

# LlamaGuard - AI Content Moderation

## Đánh giá (Score 4.5/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 5/5 | 30% | 1.50 |
| Usability | 5/5 | 30% | 1.50 |
| Modularity | 4/5 | 20% | 0.80 |
| Safety | 3/5 | 20% | 0.60 |
| **Weighted Total** | | | **4.40/5** |

**Summary:** This skill delivers the most practical deployment coverage of any safety skill in the library — five working workflows from basic HuggingFace inference through production FastAPI endpoints, with a concrete throughput spec (50-100 req/sec on A100). The false-positive handling via logit threshold calibration is a valuable production insight. However, a safety skill that teaches content moderation receives a lower Safety score for lacking guidance on adversarial bypass testing and providing no accuracy breakdown by category (only aggregate 94-95%).

## Ưu điểm

- **Five distinct deployment workflows** (basic inference, input filtering, output filtering, vLLM batch serving, FastAPI endpoint, NeMo Guardrails integration) cover the full deployment spectrum from prototype to production — unusual depth for a single skill.
- **Latency spec table** (HuggingFace Transformers: 300-500ms, vLLM: 50-100ms, batched vLLM: 20-50ms per request) provides concrete SLA guidance, enabling OMC to select the right deployment path based on throughput requirements.
- **False-positive mitigation via logit thresholding** (`torch.softmax(logits.scores[0][0])` with configurable threshold) addresses a real production problem. The pattern of reading raw model probabilities rather than just the text output is a non-obvious technique that significantly reduces over-blocking in borderline cases.
- **Hardware requirements table** (FP16: 14GB, INT8: 7GB, INT4: 4GB) with memory optimization path enables the skill to be useful across GPU tiers, from inference cloud to local development.
- **NeMo Guardrails integration** (Workflow 5) shows how LlamaGuard fits into a larger safety orchestration framework — the Rails config DSL pattern (`rails: input: flows:`) enables declarative safety policy definition without code changes.

## Nhược điểm

- **Safety score reduced: no adversarial bypass testing guidance**: A skill teaching content moderation should include red-teaming guidance — common jailbreak patterns that evade LlamaGuard (e.g., role-play framing, code-switching, indirect requests) are well-documented in the literature but absent here.
- **Per-category accuracy not provided**: The skill claims "94-95% accuracy" as an aggregate but doesn't break down false-negative rates per safety category (S1-S6). In practice, LlamaGuard performs very differently on S5 (self-harm, ~90% recall) vs S3 (weapons, ~97% recall). This omission could lead to overconfidence in high-stakes categories.
- **API endpoint code has a bug**: In Workflow 4 (FastAPI), `tokenizer` is used in the endpoint handler but is defined in Workflow 1 scope — in a real service this would cause a `NameError`. The example lacks the service initialization boilerplate.
- **Model version table is underdeveloped**: LlamaGuard 1 vs 2 vs 3 differences are listed but without accuracy comparisons or migration guidance. Should a user upgrade from v1 to v3? The skill doesn't say.
- **No rate limiting or abuse prevention guidance**: The FastAPI endpoint example has no authentication, rate limiting, or queue management. For production deployment these are mandatory but completely absent.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User builds an LLM application and mentions "content moderation", "input filtering", "output safety"
- User mentions "deploy safety layer", "LlamaGuard", "classify harmful content"
- OMC detects that an autopilot session is building a user-facing LLM product (chatbot, API, agent)
- User explicitly mentions safety categories (violence, sexual content, weapons, substances, self-harm)

### Integration points
- **OMC autopilot safety layer**: When autopilot builds any LLM application, it should automatically propose LlamaGuard integration as a step in the deployment checklist. The `check_input()` and `check_output()` patterns from Workflows 1-2 are drop-in additions to any inference pipeline.
- **team pipeline**: `team-exec` building an LLM service should call the `security-reviewer` agent, which in turn checks whether content moderation is in place. If not, it should reference this skill to add it.
- **Integration with `07-safety-alignment` category**: LlamaGuard works best as a first-pass filter before Constitutional AI (for fine-tuning) and NeMo Guardrails (for policy enforcement). OMC should suggest this layered architecture when safety requirements are mentioned.
- **Production gap**: The skill needs an OMC-specific wrapper that auto-selects deployment mode (HuggingFace vs vLLM) based on available GPU memory, inferred from system context. The memory table already provides the data needed for this decision.

### Priority
- **High** — Content moderation is a production requirement for any deployed LLM application. LlamaGuard is the only open-weights model in this space with documented accuracy metrics and HuggingFace integration. This skill enables OMC to autonomously add safety layers to any LLM deployment workflow.

## Ghi chú kỹ thuật
- **Dependencies:** transformers, torch, vllm
- **Line count:** 338 lines (within 200-500 target)
- **Known issues:** FastAPI example has scoping bug with `tokenizer`; model requires HuggingFace login + license acceptance; no built-in rate limiting
- **Alternatives:** OpenAI Moderation API (free, API-only, no self-hosting), Perspective API (Google, toxicity-focused), NeMo Guardrails (framework-level, uses LlamaGuard internally), Llama-Guard-3-11B-Vision (multimodal, newer)
