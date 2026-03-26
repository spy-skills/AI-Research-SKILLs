---
date: 2026-03-26
type: skill-analysis
skill: peft
category: fine-tuning
score: 4.3/5
tags: [Fine-Tuning, PEFT, LoRA, QLoRA, Parameter-Efficient]
---

# PEFT (Parameter-Efficient Fine-Tuning)

## Đánh giá (Score 4.3/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 4.0/5 | 20% | 0.80 |
| Safety | 3.5/5 | 20% | 0.70 |
| **Weighted Total** | | | **4.20/5** |

**Summary:** The PEFT skill is one of the most technically dense entries in the library, providing production-grade LoRA and QLoRA configurations with concrete memory benchmarks and MMLU quality comparisons. The parameter selection tables (rank vs. memory vs. quality, target modules by architecture) are immediately actionable for any researcher. The skill appropriately scopes itself to PEFT-as-library and delegates full training workflows to TRL/Axolotl, keeping it under 432 lines and well within the 500-line guideline.

## Ưu điểm
- **Quantitative benchmarks**: Memory and quality tables (Full FT vs. LoRA vs. QLoRA on 8B and 13B models) give concrete evidence for trade-off decisions, rare in skill files of this type.
- **Rank selection decision table**: The 4/8/16/32/64 rank comparison with recommended starting points (r=8 "Good", r=16 "Better") eliminates common trial-and-error for beginners.
- **Architecture-specific target_modules**: Separate module lists for Llama/Mistral/Qwen, GPT-2, Falcon, and BLOOM prevent a common configuration error, with the `"all-linear"` PEFT 0.6.0+ shortcut noted.
- **Multi-adapter serving pattern**: The `load_adapter` / `set_adapter` / `disable_adapter` workflow is directly relevant to production deployments and not covered in competing skills.
- **Ecosystem integration**: Explicit TRL `SFTTrainer` integration, Axolotl YAML config example, and vLLM `LoRARequest` inference pattern show the full lifecycle from training to serving.

## Nhược điểm
- **Missing gradient checkpointing in base example**: The standard LoRA quick start doesn't enable `gradient_checkpointing_enable()`, so an 8B model will OOM on 24GB GPUs at the shown batch size of 4.
- **No DoRA coverage in main SKILL.md**: DoRA (a significant improvement over LoRA) is deferred to `references/advanced-usage.md` without any mention in the main skill body — users unaware of it will miss a major quality boost.
- **Safety/catastrophic forgetting not addressed**: No guidance on learning rate warmup, evaluation frequency, or early stopping to prevent forgetting base model capabilities during long fine-tuning runs.
- **`data_collator` lambda in quick start is fragile**: The inline lambda for `data_collator` lacks `labels` masking for prompt tokens (trains on instruction tokens too), which is a common quality bug for instruction tuning.
- **No dataset format guidance**: The skill uses `databricks-dolly-15k` as a stand-in but gives no guidance on chat templates, system prompts, or the difference between instruction/response formatting for different model families.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User mentions fine-tuning a 7B–70B model with limited VRAM (mentions RTX 4090, A100, single GPU)
- Task involves training multiple task-specific adapters from one base model for efficient serving
- Request includes keywords: "LoRA", "QLoRA", "adapters", "PEFT", "parameter-efficient"
- Any fine-tuning task where full parameter training would exceed available GPU memory

### Integration points
- **OMC `executor` agent**: Generates PEFT LoraConfig and training scripts; coordinates with `trl-fine-tuning` skill for SFTTrainer integration
- **OMC `build-fixer` agent**: Handles CUDA OOM errors by applying the three-step fix (gradient checkpointing, batch size reduction, QLoRA upgrade) documented in the skill's common issues section
- **OMC `verifier` agent**: Validates `model.print_trainable_parameters()` output to confirm adapter is correctly applied and within expected parameter budget
- **`team` pipeline**: `team-plan` stage determines rank/alpha from task complexity; `team-exec` runs training; `team-verify` checks held-out evaluation metrics before merge

### Priority
- **High** — PEFT is a foundational dependency for ~60% of fine-tuning workflows in the library; directly required by TRL, Axolotl, Unsloth, and vLLM serving skills

## Ghi chú kỹ thuật
- **Dependencies:** `peft>=0.13.0`, `transformers>=4.45.0`, `torch>=2.0.0`, `bitsandbytes>=0.43.0`
- **Line count:** 432 lines (within 500-line guideline)
- **Known issues:** `bitsandbytes` requires CUDA; Windows support limited; QLoRA with 4-bit on A100 is slower than LoRA on A100 (confirmed by benchmark in skill); `merge_and_unload()` OOMs on small GPUs for large models
- **Alternatives:** Unsloth (2x faster LoRA training, custom CUDA kernels), Axolotl (YAML-driven, wraps PEFT), LLaMA-Factory (GUI + YAML), TorchTune (PyTorch-native, no bitsandbytes dependency)
