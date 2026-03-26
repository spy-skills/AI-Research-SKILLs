---
date: 2026-03-26
type: skill-analysis
skill: quantizing-models-bitsandbytes
category: optimization
score: 4.3/5
tags: [Optimization, Bitsandbytes, Quantization, 8-Bit, 4-Bit]
---

# bitsandbytes - LLM Quantization

## Đánh giá (Score 4.3/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 5.0/5 | 30% | 1.50 |
| Modularity | 4.0/5 | 20% | 0.80 |
| Safety | 2.5/5 | 20% | 0.50 |
| **Weighted Total** | | | **4.15/5** |

**Summary:** Skill bitsandbytes có usability xuất sắc — 3 workflows rõ ràng covering inference quantization, QLoRA fine-tuning, và 8-bit optimizer; memory calculation formulas cụ thể; VRAM-to-model-size decision table. Rất accessible cho researchers với limited GPU resources. Điểm yếu là thiếu accuracy benchmarks thực tế và không đề cập đến quantization-aware training.

## Ưu điểm

- **Memory calculation formulas cụ thể và actionable**: FP16/INT8/INT4 formula (Parameters × bytes / 1e9) kèm theo ví dụ Llama 2 7B (14GB → 7GB → 3.5GB) — researcher có thể tính ngay trước khi run
- **Decision matrix VRAM vs model size**: Bảng mapping GPU VRAM (8/12/16/24/40+ GB) tới model size và recommended quantization — eliminates guesswork hoàn toàn
- **QLoRA workflow đầy đủ**: 4 steps từ install → configure 4-bit base → add LoRA adapters → train, với detail về `prepare_model_for_kbit_training`, LoRA config parameters, và output "trainable params: 4.2M (0.06%)"
- **8-bit optimizer workflow rõ ràng**: Optimizer memory reduction formula (56GB → 14GB cho Llama 2 7B), cả `optim="paged_adamw_8bit"` shortcut lẫn manual `bnb.optim.AdamW8bit` usage
- **Common issues section thực tế**: 4 issues (CUDA error, slow loading, accuracy loss, OOM even with 4-bit) với specific fixes — đặc biệt CPU/disk offload solutions cho extreme memory constraints

## Nhược điểm

- **Thiếu accuracy benchmarks thực tế**: Claim "<1% accuracy loss" không có supporting data; không có benchmark table so sánh INT4 vs INT8 vs FP16 trên MMLU, GSM8K, HumanEval
- **Không đề cập quantization-aware training (QAT)**: bitsandbytes chủ yếu là post-training quantization; user cần biết limitations khi so sánh với QAT approaches
- **AMD ROCm support không được giải thích chi tiết**: Mention "experimental" nhưng không có installation steps hay known limitations — confusing cho AMD users
- **Deployment limitations sau quantization**: Không mention rằng 4-bit models không thể directly serve với vLLM/TensorRT (cần convert) — important cho production workflow
- **Không có validation step**: Sau khi quantize, không có guidance về cách verify model correctness (run inference, compare outputs, check perplexity) trước khi training

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User hỏi "làm thế nào để fit model X vào GPU Y GB"
- Setup QLoRA fine-tuning cho consumer hardware (RTX 3090, 4090, A100 40GB)
- Training với memory constraints và không muốn dùng multi-GPU
- Inference với limited VRAM (single GPU deployment)
- 8-bit optimizer để reduce optimizer memory during full fine-tuning

### Integration points
- **Với Axolotl/LLaMA-Factory fine-tuning skills**: bitsandbytes là prerequisite cho QLoRA — natural chaining (quantization config → LoRA config → training)
- **Với vLLM/TensorRT-LLM skills**: Boundary condition — bitsandbytes cho experimentation, GPTQ/AWQ cho production serving
- **Với lm-evaluation-harness skill**: Evaluate quantized models với `load_in_4bit=True` flag — integration point rõ ràng
- **OMC workflow**: `analyst` assess hardware constraints → bitsandbytes config → `executor` implement QLoRA → `verifier` validate accuracy preservation

### Priority
- **High** — Memory efficiency là bottleneck phổ biến nhất trong AI research; bitsandbytes là standard solution cho QLoRA; trigger conditions rất common trong research workflows

## Ghi chú kỹ thuật

- **Dependencies:** bitsandbytes, transformers, accelerate, torch (no version constraints — weakness)
- **Line count:** 411 lines (well within 200-500 target)
- **Known issues:** CUDA version mismatch (most common install issue); không support CPU-only machines; Windows support problematic; 4-bit không support gradient checkpointing với some configs
- **Alternatives:** GPTQ/AWQ (faster inference, production serving), GGUF/llama.cpp (CPU inference), FP8 on H100 (hardware-accelerated, faster than INT8), HQQ (higher quality quantization)
- **Reference files:** qlora-training.md, quantization-formats.md, memory-optimization.md — appropriate split
- **Hardware requirement:** NVIDIA compute capability 7.0+ (Turing/Ampere/Hopper); CUDA 11.1+ (12.0+ recommended)
