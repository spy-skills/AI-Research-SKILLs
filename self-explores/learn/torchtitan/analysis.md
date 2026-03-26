---
date: 2026-03-26
type: skill-analysis
skill: distributed-llm-pretraining-torchtitan
category: model-architecture
score: 4.3/5
tags: [Model Architecture, Distributed Training, TorchTitan, FSDP2, Tensor Parallel]
---

# TorchTitan - Distributed LLM Pretraining

## Đánh giá (Score 4.3/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 4.0/5 | 20% | 0.80 |
| Safety | 3.5/5 | 20% | 0.70 |
| **Weighted Total** | | | **4.20/5** |

**Summary:** TorchTitan là skill chất lượng cao với 4 workflow chi tiết covering single-node, multi-node SLURM, Float8, và 4D parallelism (FSDP2+TP+PP+CP). Nội dung được cấu trúc tốt với code examples thực tế, performance benchmarks rõ ràng, và bảng so sánh với các alternatives. Điểm yếu chính là thiếu safety/monitoring guidance và phụ thuộc nặng vào NVIDIA hardware.

## Ưu điểm

- **4 workflows hoàn chỉnh** với step-by-step checklists: single-node, multi-node SLURM, Float8 H100, và 4D parallelism cho 405B models — coverage rộng từ 8 GPUs đến 512+ GPUs
- **Performance benchmarks cụ thể trên H100**: Llama 8B đạt 8,532 TPS/GPU với FSDP+compile+FP8 (+48% vs baseline), Llama 405B trên 512 GPUs — dữ liệu thực chiến rõ ràng
- **TOML-based config system** được giải thích tốt với các tham số quan trọng (parallelism degrees, batch size, seq_len, checkpoint interval) và TOML examples copy-paste được
- **Bảng supported models cập nhật** (Llama 3.1/4, DeepSeek V3, GPT-OSS, Qwen 3, Flux) cùng với experimental status — honest về limitations
- **Common issues section** với 5 vấn đề thực tế kèm solutions: OOM, TP memory, Float8 speed, checkpoint resharding, PP initialization — rất practical

## Nhược điểm

- **Thiếu monitoring/alerting guidance**: Không có guidance về cách monitor distributed training health (NCCL errors, GPU utilization, throughput degradation) ngoài TensorBoard basic
- **Safety trong pretraining không được đề cập**: Không có guidance về data poisoning, checkpoint validation, hay training stability monitoring khi scale lên 512+ GPUs
- **SLURM configuration thiếu details**: SLURM script example thiếu NCCL environment variables quan trọng (NCCL_IB_DISABLE, NCCL_SOCKET_IFNAME), network fabric setup cho multi-node
- **Không có troubleshooting cho network failures**: Distributed training thường xuyên gặp network partition, NCCL timeout — không có guidance cụ thể
- **Dependencies phức tạp không được giải thích đầy đủ**: PyTorch nightly requirement cho source install, torchao compatibility matrix với CUDA versions chưa được làm rõ

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User yêu cầu pretrain LLM từ scratch (không phải fine-tune)
- Câu hỏi về 4D parallelism (FSDP, TP, PP, CP) configuration
- Cần scale training lên 64+ GPUs hoặc multi-node setup
- H100 Float8 training optimization
- Checkpoint format conversion (torchtitan → HuggingFace)

### Integration points
- **Với DeepSpeed skill**: User hỏi về ZeRO stages → suggest TorchTitan FSDP2 như alternative PyTorch-native
- **Với Megatron-Core skill**: Performance comparison cho NVIDIA-only vs portable deployments
- **Với MLflow/W&B skills**: TensorBoard output từ TorchTitan có thể được consumed bởi monitoring skills
- **Với Modal/SkyPilot skills**: Infrastructure provisioning trước khi chạy TorchTitan training jobs
- **OMC workflow**: `team-plan` → infra provisioning → `team-exec` với TorchTitan config → `team-verify` với benchmark metrics

### Priority
- **High** — Pretraining từ scratch là use case đặc thù cần specialized knowledge; TorchTitan là PyTorch official solution; ít overlap với fine-tuning skills

## Ghi chú kỹ thuật

- **Dependencies:** torch>=2.6.0, torchtitan>=0.2.0, torchao>=0.5.0
- **Line count:** 359 lines (within 200-500 target)
- **Known issues:** Float8 chỉ benefit cho large GEMMs (cần filter small layers); PP initialization yêu cầu seed checkpoint trước; TP có memory overhead với async collectives
- **Alternatives:** Megatron-LM (max NVIDIA perf), DeepSpeed (ZeRO ecosystem), Axolotl/TRL (fine-tuning), LitGPT (educational/small scale)
- **Reference files:** fsdp.md, float8.md, checkpoint.md, custom-models.md — tốt, one level deep
- **Hardware constraint:** NVIDIA only (H100 cho Float8); AMD/Intel không được mention
