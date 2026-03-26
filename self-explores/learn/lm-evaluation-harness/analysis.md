---
date: 2026-03-26
type: skill-analysis
skill: evaluating-llms-harness
category: evaluation
score: 4.3/5
tags: [Evaluation, LM Evaluation Harness, Benchmarking, MMLU, HumanEval]
---

# lm-evaluation-harness - LLM Benchmarking

## Đánh giá (Score 4.3/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 5.0/5 | 30% | 1.50 |
| Modularity | 3.5/5 | 20% | 0.70 |
| Safety | 2.0/5 | 20% | 0.40 |
| **Weighted Total** | | | **3.95/5** |

**Summary:** lm-evaluation-harness là một trong những skills usable nhất trong library với 4 workflows rõ ràng (standard benchmark, training progress tracking, multi-model comparison, vLLM acceleration). Quick start chỉ cần `pip install lm-eval` và một CLI command — barrier to entry cực thấp. Workflow 2 (training progress tracking với PyTorch Lightning callback) đặc biệt valuable. Điểm yếu là thiếu safety/bias evaluation guidance và không có guidance về custom task creation trong SKILL.md.

## Ưu điểm

- **Minimal setup**: `pip install lm-eval` + single CLI command — fastest time-to-first-result trong toàn bộ library; không cần git clone, không cần complex configuration
- **Workflow 2 "Track training progress" rất unique**: Bao gồm PyTorch Lightning callback example, fast benchmark selection guide (HellaSwag 10min vs MMLU 2 hours), và matplotlib learning curve plotting — end-to-end training monitoring workflow
- **vLLM integration workflow**: 5-10x speedup (MMLU 2 hours → 15-20 phút) với `--model vllm` flag — practical acceleration path cho frequent evaluation
- **Multi-model comparison với script automation**: Shell script + Python comparison table generator với pandas + markdown output — reproducible benchmarking pipeline
- **Results interpretation guidance**: JSON output structure được explain rõ với primary metrics per task (acc vs exact_match) — researcher biết cách parse results ngay

## Nhược điểm

- **Safety/fairness evaluation hoàn toàn thiếu**: Library có ToxiGen, WinoBias, CrowS-Pairs tasks nhưng skill không đề cập — nghiêm trọng vì responsible AI evaluation quan trọng như accuracy
- **Custom task creation không có trong SKILL.md**: Chỉ có link đến references/custom-tasks.md — với 60+ tasks, custom task là common need và xứng đáng có at least one example inline
- **API evaluation không được cover trong SKILL.md**: OpenAI/Anthropic API evaluation chỉ link đến references — researcher cần này ngay khi so sánh với proprietary models
- **Statistical significance thiếu**: Benchmark results có stderr nhưng không guidance về khi nào difference là statistically significant — researcher có thể over-interpret small differences
- **Modularity thấp hơn BigCode**: Không có `--generation_only` + load_generations workflow như BigCode — không thể decouple generation từ evaluation, problematic cho expensive models

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- Cần benchmark model sau khi fine-tuning hoặc pretraining
- So sánh multiple models trên standard academic benchmarks (MMLU, GSM8K, HellaSwag)
- Tracking training progress với periodic evaluation checkpoints
- Chuẩn bị model release với standardized benchmark results
- Academic paper cần reproducible evaluation numbers

### Integration points
- **Với TorchTitan pretraining skill**: Evaluate checkpoints trong training loop — lm-eval là standard tool sau pretraining
- **Với Axolotl/LLaMA-Factory fine-tuning skills**: Post-fine-tuning evaluation workflow — compare before/after fine-tuning scores
- **Với vLLM serving skill**: Use vLLM backend cho faster evaluation (`--model vllm`) — direct dependency
- **Với bigcode-evaluation-harness skill**: Complementary coverage — general benchmarks (this skill) + code-specific (BigCode) cho comprehensive evaluation suite
- **OMC workflow**: After training → `executor` save checkpoint → lm-eval evaluation → `scientist` interpret results → `writer` format benchmark table for paper

### Priority
- **High** — Evaluation là universal requirement trong AI research; lm-evaluation-harness là defacto industry standard (used by HuggingFace Open LLM Leaderboard, EleutherAI, major labs); most frequently needed after any training run

## Ghi chú kỹ thuật

- **Dependencies:** lm-eval, transformers, vllm (no version constraints — weakness for reproducibility)
- **Line count:** 491 lines (just under 500-line limit — tight)
- **Known issues:** MMLU takes 2 hours on 7B model single GPU (vLLM reduces to 15-20 min); HumanEval requires `pip install human-eval` + `--allow_code_execution`; results differ from papers if num_fewshot/temperature don't match exactly
- **Alternatives:** HELM (Stanford, broader: fairness/efficiency/calibration), AlpacaEval (instruction-following with LLM judges), MT-Bench (conversational multi-turn), custom scripts (domain-specific)
- **Version note:** Task names must match exactly (e.g., `mmlu` not `mmlu_fewshot`) — common gotcha for reproducibility
- **Reproducibility critical params:** --num_fewshot (most papers use 5-shot for MMLU), --temperature (typically 0 for greedy), --batch_size (use `auto` for memory efficiency)
