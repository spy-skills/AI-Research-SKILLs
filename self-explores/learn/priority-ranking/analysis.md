---
date: 2026-03-26
type: skill-analysis
task: skills-0kv
scope: priority-ranking
tags: [ranking, weighted-scoring, 85-skills, tiers, learning-path]
---

# Priority Ranking — 85 AI Research Skills

Weighted scoring: Completeness 30%, Usability 30%, Modularity 20%, Safety 20%
Rubric: 1=stub → 3=adequate → 5=gold standard
Persona: ML Engineer on RTX 3090, wants production-usable skills

## Summary
- **85 skills scored** across 21 categories
- **Score range:** 1.0 — 4.8
- **Average:** 3.77
- **Tier 1 (Must Learn, ≥4.0):** 30 skills
- **Tier 2 (Should Learn, 3.0-3.9):** 50 skills
- **Tier 3 (Needs Improvement, <3.0):** 5 skills
- **Top scorer:** ml-paper-writing (4.8/5)
- **Bottom:** llama-factory, unsloth, deepspeed (1.0/5 — stubs)

## Top 20 Skills (Input for Deep Dive)

| # | Skill | Category | C | U | M | S | Weighted | Notes |
|---|-------|----------|---|---|---|---|----------|-------|
| 1 | ml-paper-writing | 20-ml-paper-writing | 5 | 5 | 4 | 5 | **4.80** | Gold standard — narrative, LaTeX, citation safety |
| 2 | saelens | 04-mech-interp | 5 | 5 | 4 | 4 | **4.60** | Safety-relevant features (deception, bias) |
| 3 | grpo-rl-training | 06-post-training | 5 | 5 | 4 | 4 | **4.60** | Reward design, debugging workflow, checklists |
| 4 | llamaguard | 07-safety | 4 | 5 | 4 | 5 | **4.50** | 5 workflows, vLLM deploy, safety-native |
| 5 | llamaindex | 14-agents | 5 | 5 | 4 | 3 | **4.40** | RAG/agents/multi-modal/eval, benchmarks |
| 6 | dspy | 16-prompt-eng | 5 | 5 | 4 | 3 | **4.40** | Declarative prompts, optimizers, patterns |
| 7 | peft | 03-fine-tuning | 5 | 5 | 4 | 3 | **4.30** | QLoRA/LoRA, rank selection, multi-adapter |
| 8 | trl-fine-tuning | 06-post-training | 5 | 5 | 4 | 3 | **4.30** | Full RLHF pipeline, method guide |
| 9 | nnsight | 04-mech-interp | 5 | 5 | 4 | 3 | **4.30** | 5 workflows, remote NDIF, gradients |
| 10 | pyvene | 04-mech-interp | 5 | 5 | 4 | 3 | **4.30** | Causal tracing, IIT, steering |
| 11 | torchtitan | 01-model-arch | 5 | 5 | 4 | 3 | **4.30** | 4D parallelism, SLURM, Float8 |
| 12 | huggingface-tokenizers | 02-tokenization | 5 | 5 | 4 | 3 | **4.30** | All 3 algorithms, benchmarks, production |
| 13 | bitsandbytes | 10-optimization | 5 | 5 | 4 | 3 | **4.30** | QLoRA workflow, memory calc, VRAM guide |
| 14 | bigcode-eval | 11-evaluation | 5 | 5 | 4 | 3 | **4.30** | 4 workflows, multi-lang, Docker |
| 15 | lm-eval-harness | 11-evaluation | 5 | 5 | 4 | 3 | **4.30** | 4 workflows, vLLM speedup, gold standard |
| 16 | constitutional-ai | 07-safety | 4 | 4 | 4 | 5 | **4.20** | SL + RLAIF + CoT critique workflows |
| 17 | nemo-guardrails | 07-safety | 4 | 4 | 4 | 5 | **4.20** | Jailbreak, PII, fact-checking |
| 18 | prompt-guard | 07-safety | 4 | 4 | 4 | 5 | **4.20** | Injection detection, threshold tuning |
| 19 | brainstorming-research | 21-ideation | 4 | 5 | 4 | 3 | **4.10** | 10 frameworks, diverge/converge protocol |
| 20 | creative-thinking | 21-ideation | 4 | 5 | 4 | 3 | **4.10** | Cognitive science grounding, 8 frameworks |

## Full Ranking (85 skills)

### Tier 1: Must Learn (≥4.0) — 30 skills

| Skill | Cat | W | Skill | Cat | W |
|-------|-----|---|-------|-----|---|
| ml-paper-writing | 20 | 4.80 | crewai | 14 | 4.10 |
| saelens | 04 | 4.60 | langchain | 14 | 4.10 |
| grpo-rl-training | 06 | 4.60 | flash-attention | 10 | 4.10 |
| llamaguard | 07 | 4.50 | nemo-evaluator | 11 | 4.10 |
| llamaindex | 14 | 4.40 | sglang | 12 | 4.10 |
| dspy | 16 | 4.40 | vllm | 12 | 4.10 |
| peft | 03 | 4.30 | accelerate | 08 | 4.10 |
| trl-fine-tuning | 06 | 4.30 | megatron-core | 08 | 4.10 |
| nnsight | 04 | 4.30 | pytorch-lightning | 08 | 4.10 |
| pyvene | 04 | 4.30 | litgpt | 01 | 4.10 |
| torchtitan | 01 | 4.30 | instructor | 16 | 4.10 |
| hf-tokenizers | 02 | 4.30 | outlines | 16 | 4.10 |
| bitsandbytes | 10 | 4.30 | brainstorming | 21 | 4.10 |
| bigcode-eval | 11 | 4.30 | creative-thinking | 21 | 4.10 |
| lm-eval-harness | 11 | 4.30 | slime | 06 | 4.10 |
| constitutional-ai | 07 | 4.20 | nemo-curator | 05 | 4.00 |
| nemo-guardrails | 07 | 4.20 | pytorch-fsdp2 | 08 | 4.00 |
| prompt-guard | 07 | 4.20 | transformer-lens | 04 | 4.10 |

### Tier 2: Should Learn (3.0-3.9) — 50 skills

| Skill | Cat | W | Skill | Cat | W |
|-------|-----|---|-------|-----|---|
| mlflow | 13 | 3.90 | hqq | 10 | 3.70 |
| tensorboard | 13 | 3.90 | gptq | 10 | 3.70 |
| wandb | 13 | 3.90 | mamba | 01 | 3.60 |
| autogpt | 14 | 3.90 | nanogpt | 01 | 3.60 |
| chroma | 15 | 3.90 | rwkv | 01 | 3.60 |
| qdrant | 15 | 3.90 | sentencepiece | 02 | 3.60 |
| guidance | 16 | 3.90 | openrlhf | 06 | 3.60 |
| langsmith | 17 | 3.90 | simpo | 06 | 3.60 |
| phoenix | 17 | 3.90 | ray-data | 05 | 3.60 |
| audiocraft | 18 | 3.90 | pinecone | 15 | 3.60 |
| blip-2 | 18 | 3.90 | lambda-labs | 09 | 3.50 |
| segment-anything | 18 | 3.90 | modal | 09 | 3.50 |
| stable-diffusion | 18 | 3.90 | skypilot | 09 | 3.50 |
| knowledge-distill | 19 | 3.90 | gguf | 10 | 3.50 |
| long-context | 19 | 3.90 | llama-cpp | 12 | 3.20 |
| model-merging | 19 | 3.90 | clip | 18 | 3.10 |
| model-pruning | 19 | 3.90 | whisper | 18 | 3.10 |
| moe-training | 19 | 3.90 | sentence-trans | 15 | 3.10 |
| speculative-dec | 19 | 3.90 | faiss | 15 | 3.00 |
| miles | 06 | 3.90 | | | |
| torchforge | 06 | 3.90 | | | |
| awq | 10 | 3.90 | | | |
| ray-train | 08 | 3.90 | | | |
| verl | 06 | 4.10 | | | |

### Tier 3: Needs Improvement (<3.0) — 5 skills

| Skill | Cat | W | Issue |
|-------|-----|---|-------|
| llava | 18 | 2.90 | Missing workflows, troubleshooting, safety |
| tensorrt-llm | 12 | 2.60 | Too brief (187L), lacks depth |
| axolotl | 03 | 1.80 | Auto-generated stub |
| llama-factory | 03 | 1.00 | Empty stub |
| unsloth | 03 | 1.00 | Empty stub |
| deepspeed | 08 | 1.00 | Raw scraped content, no structure |

## Learning Path Recommendation

### Phase 1: Core Foundation (Days 1-2)
Start with highest-scoring, most broadly applicable skills:
1. **peft** (4.3) — LoRA/QLoRA foundation, used by 80% of other skills
2. **bitsandbytes** (4.3) — Memory optimization, enables running large models on RTX 3090
3. **trl-fine-tuning** (4.3) — SFT/DPO/GRPO pipeline, builds on PEFT
4. **vllm** (4.1) — Production serving, needed for evaluation + deployment
5. **lm-eval-harness** (4.3) — Benchmarking your trained models

### Phase 2: Specialization (Days 3-5)
Branch based on research direction:

**Path A: RL/Alignment**
6. **grpo-rl-training** (4.6) → **constitutional-ai** (4.2) → **llamaguard** (4.5)

**Path B: Interpretability**
6. **transformer-lens** (4.1) → **saelens** (4.6) → **nnsight** (4.3)

**Path C: Application Building**
6. **llamaindex** (4.4) → **dspy** (4.4) → **langchain** (4.1)

### Phase 3: Research & Writing (Days 6-7)
7. **brainstorming-research** (4.1) — Generate research ideas
8. **creative-thinking** (4.1) — Novel angles
9. **ml-paper-writing** (4.8) — Write up results

## Category Insights

| Category | Avg Score | Best Skill | Worst | Notes |
|----------|-----------|------------|-------|-------|
| 01-model-arch | 3.84 | torchtitan 4.3 | — | Solid across board |
| 02-tokenization | 3.95 | hf-tokenizers 4.3 | — | Small but strong |
| 03-fine-tuning | 2.78 | peft 4.3 | llama-factory 1.0 | 2 stubs drag down |
| 04-mech-interp | 4.33 | saelens 4.6 | — | Strongest category |
| 05-data-processing | 3.80 | nemo-curator 4.0 | — | Adequate |
| 06-post-training | 4.09 | grpo 4.6 | — | Deep & broad |
| 07-safety | 4.28 | llamaguard 4.5 | — | Safety focus lifts S score |
| 08-distributed | 3.53 | megatron 4.1 | deepspeed 1.0 | 1 stub |
| 09-infrastructure | 3.50 | (tied) 3.5 | (tied) 3.5 | All adequate, none exceptional |
| 10-optimization | 3.90 | bitsandbytes 4.3 | — | Strong |
| 11-evaluation | 4.23 | (tied) 4.3 | — | Excellent |
| 12-inference | 3.50 | sglang/vllm 4.1 | tensorrt 2.6 | Mixed |
| 13-mlops | 3.90 | (tied) 3.9 | — | All good, none gold |
| 14-agents | 4.13 | llamaindex 4.4 | — | Strong |
| 15-rag | 3.50 | qdrant 3.9 | faiss 3.0 | Variable |
| 16-prompt-eng | 4.13 | dspy 4.4 | — | Strong |
| 17-observability | 3.90 | (tied) 3.9 | — | Solid pair |
| 18-multimodal | 3.54 | 4×3.9 | llava 2.9 | Many over-length |
| 19-emerging | 3.90 | (all) 3.9 | — | Consistent |
| 20-ml-paper | 4.80 | ml-paper 4.8 | — | Single skill, exceptional |
| 21-ideation | 4.10 | (tied) 4.1 | — | Strong pair |
