---
date: 2026-03-26
type: skill-analysis
skill: evaluating-code-models
category: evaluation
score: 4.3/5
tags: [Evaluation, Code Generation, HumanEval, MBPP, MultiPL-E]
---

# BigCode Evaluation Harness - Code Model Benchmarking

## Đánh giá (Score 4.3/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 4.0/5 | 20% | 0.80 |
| Safety | 3.0/5 | 20% | 0.60 |
| **Weighted Total** | | | **4.10/5** |

**Summary:** BigCode Evaluation Harness là skill chuyên biệt, chất lượng cao cho code model evaluation với 4 workflows covering standard benchmarks, multi-language evaluation (18 languages via MultiPL-E), instruction-tuned models, và multi-model comparison. Benchmark table với 9 benchmarks rất toàn diện. Security through Docker isolation được đề cập, nhưng details về cách interpret pass@k results và statistical significance còn thiếu.

## Ưu điểm

- **Benchmark coverage toàn diện**: 9 benchmarks trong bảng (HumanEval, HumanEval+, MBPP, MBPP+, MultiPL-E, APPS, DS-1000, HumanEvalPack, Mercury) với problems count, languages, metric, và use case — researcher có thể chọn đúng benchmark ngay
- **MultiPL-E workflow với Docker isolation**: Step-by-step generate on host → evaluate in Docker — security best practice được handle đúng, không để code execution trực tiếp trên host
- **Command reference table đầy đủ**: 14 tham số với default values và descriptions — reference nhanh hơn phải đọc lại toàn bộ docs
- **Hardware requirements table**: 3 model sizes (7B/13B/34B) với VRAM (fp16 và 4-bit) và thời gian evaluation (30 min/1 hour/2 hours trên A100) — planning-ready
- **Multi-model comparison script**: bash script eval_models.sh và Python comparison table generator — reusable template cho leaderboard benchmarking

## Nhược điểm

- **Thiếu guidance về pass@k statistical interpretation**: pass@1 vs pass@10 vs pass@100 có ý nghĩa khác nhau và yêu cầu n_samples khác nhau — chỉ mention n_samples 200 nhưng không explain tại sao hay confidence intervals
- **Không có guidance về benchmark selection criteria**: Khi nào dùng HumanEval vs HumanEval+ vs APPS vs LiveCodeBench — alternatives section mention LiveCodeBench nhưng không explain contamination risks với existing benchmarks
- **Instruction token formats chưa standardized**: Mỗi instruction model có format khác nhau, skill chỉ show một ví dụ CodeLlama `"<s>[INST],</s>,[/INST]"` — researcher cần biết format cho Mistral, Llama 3, Qwen
- **Không có guidance về result reproducibility**: Temperature, sampling strategy, random seeds ảnh hưởng lớn đến results — không có explicit reproducibility checklist
- **DS-1000 và APPS workflows bị thiếu**: Benchmark table list nhưng không có specific workflow examples cho data science hay competition-level problems

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- Cần benchmark code generation model (bất kỳ size) trên HumanEval/MBPP
- So sánh coding ability của multiple models (model comparison leaderboard)
- Evaluate multilingual code generation (18 languages via MultiPL-E)
- Testing instruction-tuned code models với proper prompt formatting
- Reporting coding benchmark results cho academic paper submission

### Integration points
- **Với lm-evaluation-harness skill**: Complementary — BigCode cho code-specific benchmarks, lm-eval cho general benchmarks; thường được dùng cùng nhau cho comprehensive evaluation
- **Với vLLM skill**: Faster evaluation với vLLM backend (5-10x speedup) — explicit integration point
- **Với bitsandbytes skill**: `--load_in_4bit` flag cho evaluating large models trên limited VRAM — direct integration
- **OMC workflow**: `planner` chọn benchmark suite → `executor` setup evaluation → `scientist` analyze pass@k results → `writer` format cho paper

### Priority
- **Medium** — Specialized cho code models specifically; nếu researcher không làm code generation research thì lm-evaluation-harness là better fit; nhưng indispensable khi evaluating code LLMs

## Ghi chú kỹ thuật

- **Dependencies:** bigcode-evaluation-harness, transformers>=4.25.1, accelerate>=0.13.2, datasets>=2.6.1 (version constraints provided — good)
- **Line count:** 406 lines (well within 200-500 target)
- **Known issues:** Code execution security risk (Docker mitigates); different results vs papers due to n_samples/temperature mismatch; MultiPL-E requires dedicated Docker image; instruction token format per-model varies
- **Alternatives:** lm-evaluation-harness (general LLM benchmarks), EvalPlus (stricter HumanEval+/MBPP+), SWE-bench (real-world GitHub issues), LiveCodeBench (contamination-free, continuously updated)
- **Security note:** `--allow_code_execution` flag executes generated code — ALWAYS use Docker for MultiPL-E and untrusted models
- **Installation:** Not pip install — requires `git clone` and `pip install -e .` — più complex than other skills
