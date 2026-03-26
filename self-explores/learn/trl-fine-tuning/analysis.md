---
date: 2026-03-26
type: skill-analysis
skill: trl-fine-tuning
category: post-training
score: 4.3/5
tags: [Post-Training, TRL, Reinforcement Learning, Fine-Tuning, SFT]
---

# TRL - Transformer Reinforcement Learning

## Đánh giá (Score 4.3/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 4.0/5 | 20% | 0.80 |
| Safety | 3.5/5 | 20% | 0.70 |
| **Weighted Total** | | | **4.20/5** |

**Summary:** The TRL skill is the most workflow-complete entry in the post-training category, covering the full RLHF stack (SFT → Reward Model → PPO) plus modern alternatives (DPO, GRPO) with copy-paste checklists for each. The three-workflow structure (full RLHF, DPO-only, GRPO) maps precisely to the real spectrum of researcher needs from maximum control to minimal memory. Hardware requirements with method-specific VRAM numbers are a standout feature not found in competing skills.

## Ưu điểm
- **Full RLHF pipeline walkthrough**: Steps 1–4 (SFT → RewardTrainer → PPO → Evaluate) give a complete, sequential pipeline that maps directly to InstructGPT's training procedure, making it directly usable for replication experiments.
- **Method selection decision table**: The "Method selection" bullet list (SFT/DPO/PPO/GRPO/Reward Model with clear decision criteria) eliminates common confusion about when to use each trainer type.
- **Copy-paste checklists**: All three workflows include checkbox-style task lists (`- [ ] Step 1...`) directly usable by OMC agents as structured task trackers.
- **CLI alternatives for every trainer**: The `trl dpo ...` and `trl grpo ...` CLI commands alongside Python API give flexibility for scripted pipelines without Python boilerplate.
- **Hardware VRAM breakdown**: Method-specific VRAM numbers (SFT 7B: 16GB with LoRA, DPO 7B: 24GB due to reference model, PPO 7B: 40GB) are concrete and actionable.

## Nhược điểm
- **GRPO reward function example is naive**: The default reward function in Workflow 3 rewards "length + unique words," which is a toy example that would produce degenerate behavior in real experiments; no guidance on null or negative reward edge cases.
- **PPO training delegated to CLI only**: The PPO workflow (Step 3) uses only a bash CLI command rather than showing the Python API, making it harder to customize KL coefficients, clip ranges, or integrate custom reward logic programmatically.
- **No multi-GPU/distributed training section**: DPO requires storing both policy and reference model simultaneously; for 70B models this implies multi-node training, but `accelerate` integration is only mentioned once without configuration details.
- **Missing RLHF dataset quality guidance**: The skill shows how to load `ultrafeedback_binarized` but doesn't discuss what makes a good preference dataset (margin between chosen/rejected, annotation agreement, diversity), which directly affects reward model quality.
- **No convergence monitoring**: No guidance on reward plateau detection, KL divergence monitoring, or how to diagnose reward hacking — critical for practical RL training stability.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User wants to align a model with human preferences using RLHF or DPO
- Task involves training a reward model from preference data (chosen/rejected pairs)
- Request mentions "PPO", "DPO", "GRPO", "RLHF", "preference alignment", or "instruction tuning"
- Any post-training workflow that goes beyond basic SFT into reinforcement learning

### Integration points
- **OMC `executor` agent**: Orchestrates the three-stage RLHF pipeline using the checklist in Workflow 1 as a task plan; coordinates with `peft-fine-tuning` for memory efficiency
- **OMC `planner` agent**: Maps the method selection criteria (SFT/DPO/PPO/GRPO) to user requirements during `team-plan` stage
- **OMC `verifier` agent**: Validates reward model training by checking if loss decreases and preference accuracy improves on held-out data; checks PPO KL divergence stays bounded
- **`ralph` mode**: DPO training loops naturally fit ralph's "verify → fix → repeat" pattern — iterate on beta parameter and evaluate alignment quality after each run
- **`grpo-rl-training` skill coordination**: TRL skill handles the trainer API level; `grpo-rl-training` skill provides deeper GRPO-specific math and reward design guidance

### Priority
- **High** — TRL is the canonical post-training library for the HuggingFace ecosystem; required by the majority of alignment research workflows; direct dependency for 3 other skills in `06-post-training/`

## Ghi chú kỹ thuật
- **Dependencies:** `trl`, `transformers`, `datasets`, `peft`, `accelerate`, `torch`
- **Line count:** 455 lines (within 500-line guideline)
- **Known issues:** DPO requires loading reference model copy (2x VRAM); PPO training is notoriously unstable with small KL coefficients; GRPO requires generating multiple completions per prompt (slow); `RewardTrainer` is sensitive to dataset format (must have `chosen`/`rejected` columns)
- **Alternatives:** OpenRLHF (distributed RLHF at scale), VERL (vLLM-integrated RL), Axolotl (YAML-driven, wraps TRL), SimPO (DPO without reference model), LLaMA-Factory (multi-method GUI)
