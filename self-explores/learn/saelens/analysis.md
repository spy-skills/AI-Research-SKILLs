---
date: 2026-03-26
type: skill-analysis
skill: sparse-autoencoder-training
category: 04-mechanistic-interpretability
score: 4.6/5
tags: [Sparse Autoencoders, SAE, Mechanistic Interpretability, Feature Discovery, Superposition]
---

# SAELens: Sparse Autoencoders for Mechanistic Interpretability

## Đánh giá (Score 4.6/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 5/5 | 30% | 1.50 |
| Usability | 5/5 | 30% | 1.50 |
| Modularity | 4/5 | 20% | 0.80 |
| Safety | 4/5 | 20% | 0.80 |
| **Weighted Total** | | | **4.60/5** |

**Summary:** This skill provides an exceptionally well-structured introduction to the SAELens workflow, covering all three core use cases (loading pre-trained SAEs, training custom SAEs, feature analysis and steering) with complete runnable code. The evaluation metrics table (L0, CE Loss Score, Dead Features, Explained Variance with target ranges) is particularly valuable and immediately actionable. The main gap is the absence of multi-GPU training guidance and no coverage of the newer JumpReLU/BatchTopK SAE variants.

## Ưu điểm

- **Three-workflow structure maps perfectly to typical research progression**: Workflow 1 (load pre-trained SAE, 5-step checklist) → Workflow 2 (train custom SAE, 7-step checklist) → Workflow 3 (feature analysis and steering) follows the exact order a researcher would encounter them, reducing cognitive overhead.
- **Evaluation metrics table with target ranges** (L0: 50-200, CE Loss Score: 80-95%, Dead Features: <5%, Explained Variance: >90%) is the most concrete specification of SAE quality criteria found in any public tutorial, making it possible to autonomously judge training success/failure.
- **Feature attribution code** (`features * (W_dec @ W_U[:, paris_token])`) demonstrates a non-trivial logit lens technique that maps individual SAE features to specific token predictions — this pattern is directly usable for interpretability experiments on any model.
- **Common Issues section** covers the three most frequent failure modes (dead features, poor reconstruction, non-interpretable features) with both diagnosis and solution code, plus the critical architectural fix: `use_ghost_grads=True` to revive dead features.
- **Neuronpedia integration** with URL pattern (`neuronpedia.org/gpt2-small/8-res-jb/{feature_idx}`) enables automated feature annotation lookups from any analysis script.

## Nhược điểm

- **No multi-GPU training guidance**: The `LanguageModelSAERunnerConfig` examples use single-GPU settings. For training SAEs on large models (Gemma-2 27B, Llama-3 70B), multi-GPU parallelism is required but not documented.
- **SAE architecture table is incomplete**: Only Standard, Gated, and TopK architectures are listed. JumpReLU SAEs (introduced in Anthropic's 2024 scaling work) and BatchTopK variants (often better than TopK) are absent despite being production-relevant.
- **No guidance on dataset selection for training**: The example uses `monology/pile-uncopyrighted` with no explanation of why this dataset, how to choose domain-specific data, or what corpus size is needed for different model scales.
- **Feature steering example uses fixed `strength=5.0`**: No guidance on how to calibrate steering strength or detect when over-steering causes incoherent outputs — a common practical issue.
- **References section lists 3 reference files but no indication of their content depth**: Cannot assess whether `references/api.md` covers the newer v6.x SAELens API without reading it separately.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User mentions "sparse autoencoder", "SAE training", "mechanistic interpretability", "find interpretable features"
- User asks to "analyze what [model] has learned about [concept]"
- User mentions "polysemanticity", "superposition", "feature steering", "monosemantic"
- User wants to reproduce Anthropic's "Towards Monosemanticity" results on a custom model

### Integration points
- **OMC scientist agent**: SAE feature analysis maps directly to the `scientist` agent role. A team workflow could have `explore` identify hook points, `executor` run Workflow 1 to load and encode activations, then `scientist` interpret the top features per token.
- **autopilot integration**: When a user provides a model and asks "what does this model know about X?", autopilot should load the matching pre-trained SAE (checking Hugging Face for `saelens`-tagged releases), run feature attribution analysis, and return top feature activations with Neuronpedia URLs.
- **Safety-relevant use case**: The skill notes SAEs can analyze "safety-relevant features (deception, bias, harmful content)". This creates a natural bridge to the `07-safety-alignment` category — an OMC pipeline could automatically run SAE feature analysis on fine-tuned models to surface unexpected capability gains.
- **Production gap**: No streaming/async API for encoding large activation corpora. Running SAE encoding on 1M tokens needs batch processing utilities not shown in the skill.

### Priority
- **High** — SAE analysis is a core capability for AI safety research and model understanding. With pre-trained SAEs available for GPT-2 and Gemma-2B, this skill enables immediate autonomous interpretability experiments without GPU training.

## Ghi chú kỹ thuật
- **Dependencies:** sae-lens>=6.0.0, transformer-lens>=2.0.0, torch>=2.0.0
- **Line count:** 387 lines (within 200-500 target)
- **Known issues:** Dead features without `use_ghost_grads=True` and `l1_warm_up_steps`; memory errors on large models without buffer tuning
- **Alternatives:** TransformerLens (activation analysis without SAE decomposition), pyvene (causal interventions), NNsight (PyTorch-native hooks)
