---
date: 2026-03-26
type: skill-analysis
skill: constitutional-ai
category: 07-safety-alignment
score: 4.2/5
tags: [Safety Alignment, Constitutional AI, RLAIF, Self-Critique, Harmlessness]
---

# Constitutional AI

## Đánh giá (Score 4.2/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.0/5 | 30% | 1.20 |
| Modularity | 4.0/5 | 20% | 0.80 |
| Safety | 4.5/5 | 20% | 0.90 |
| **Weighted Total** | | | **4.25/5** |

**Summary:** The skill covers Anthropic's Constitutional AI paper faithfully with clear two-phase breakdown (SL and RLAIF) and production-ready code using TRL. It is the only skill in the library that addresses training-time safety alignment rather than runtime filtering, making it uniquely positioned for researchers who want to understand how Claude itself is aligned. The content is dense and actionable but relies on somewhat abstract boilerplate code (e.g., `create_dataset`, `parse_preferences`) without filling in those helper implementations.

## Ưu điểm
- **Conceptually authoritative**: Directly traces Anthropic's Dec 2022 paper (arXiv:2212.08073) and explains the two-phase methodology (SL + RLAIF) clearly with correct terminology.
- **Three complete workflows**: Self-critique/revision, RLAIF preference training, and chain-of-thought critique give progressively more sophisticated use cases.
- **Actionable TRL integration**: Uses `SFTTrainer`, `RewardTrainer`, and `PPOTrainer` from TRL — the standard library — so code examples map directly to real tooling.
- **Practical trouble-shooting section**: All four common issues (over-refusal, weak critiques, non-improving revisions, noisy RLAIF preferences) have concrete mitigation strategies including multi-model majority voting.
- **Comparative guidance**: "When to use vs alternatives" table clearly distinguishes CAI from RLHF, DPO, NeMo Guardrails, and LlamaGuard so users can route to the correct tool.

## Nhược điểm
- **Abstract helper functions**: `create_dataset(prompts, revised_responses)` and `parse_preferences(preferences, responses_a, responses_b)` are called but never implemented — a reader copying the code cannot run it without significant work.
- **No evaluation metrics**: The skill does not explain how to measure the success of constitutional training (HH-RLHF win rate, TruthfulQA, toxicity scores) leaving researchers without validation guidance.
- **Hardware section is superficial**: Says "1× A100 40GB for SL phase" but omits training time estimates, cost ranges, or multi-GPU scaling strategies.
- **Constitution examples are generic**: The sample constitution (4 principles) is too abstract for domain-specific deployments (e.g., medical, legal, customer service). The referenced `references/constitution-design.md` should have live examples but its contents are unknown.
- **No dataset sourcing guidance**: The original CAI paper used a specific mix of harmless and helpful prompts; the skill does not explain how to assemble the initial "red-teaming" prompt set.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User asks: "How do I make my model safer without human labelers?"
- User asks about RLAIF, AI feedback, or self-critique training
- User is building a post-training pipeline that follows `06-post-training/trl/` or `06-post-training/grpo-rl-training/` and wants safety constraints
- User asks about how Claude's own safety system works or how to replicate it

### Integration points
- **Pairs with `06-post-training/trl/`**: CAI SL phase uses SFTTrainer; PPO phase uses PPOTrainer — both already documented in TRL skill
- **Pairs with `07-safety-alignment/llama-guard/`**: CAI for training-time alignment, LlamaGuard for inference-time check — complementary pipeline
- **Pairs with `13-mlops/weights-and-biases/`**: RLAIF training runs should log reward scores, KL divergence, and refusal rates to W&B
- **OMC workflow**: In an autopilot session, this skill should activate after a user completes SFT training and is asking "how do I align my model"

### Priority
- **High** — This skill addresses training-time safety which is orthogonal to all runtime guardrail skills. It is the only skill explaining RLAIF, which is directly relevant to understanding Claude's own training. Researchers studying alignment or building safe models will frequently need this.

## Ghi chú kỹ thuật
- **Dependencies:** transformers, torch, trl
- **Line count:** 291 lines (within 200-500 target)
- **Known issues:** Helper function stubs (`create_dataset`, `parse_preferences`) are not implemented — major gap for practitioners trying to run the code end-to-end
- **Alternatives:** RLHF with PPO (human labels, more accurate), DPO/SimPO (offline preference learning, simpler), NeMo Guardrails (runtime, no retraining required)
- **Upstream paper:** arXiv:2212.08073 — "Constitutional AI: Harmlessness from AI Feedback" (Bai et al., Dec 2022)
- **TRL version note:** The skill uses `RewardConfig` and `processing_class` which are TRL v0.9+ API; older TRL uses different argument names
