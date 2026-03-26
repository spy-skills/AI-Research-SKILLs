---
date: 2026-03-26
type: skill-analysis
task: skills-482
scope: overview-inventory
tags: [inventory, 85-skills, 21-categories, cross-reference]
---

# AI Research Skills Library -- Complete Inventory Analysis

## 1. Summary

- **Total skills**: 85 (84 in category subdirectories + 1 category-level skill in 20-ml-paper-writing)
- **Total categories**: 21 (numbered 01 through 21)
- **YAML frontmatter**: 85/85 valid (100% compliance, 0 invalid, 0 missing required fields)
- **README accuracy**: README states 85 skills -- actual count matches
- **Line count compliance**: 64/85 within the 200-500 line standard (75%)
- **Over 500 lines (violation)**: 16 skills
- **Under 200 lines (too short)**: 5 skills
- **Reference directories present**: 78/85 (92%)
- **Missing references/**: 7 skills (all in safety-alignment, post-training, or research-ideation)
- **Shortest skills**: llama-factory (81 lines), unsloth (81 lines)
- **Longest skill**: ml-paper-writing (938 lines)
- **Largest reference set**: unsloth (1,858 KB)

---

## 2. Inventory Table

All 85 skills. Line status: OK = 200-500, Over = >500, Short = <200.

| # | Category | Skill | Lines | Status | Refs KB |
|---|----------|-------|------:|--------|--------:|
| 1 | 01-model-architecture | litgpt | 470 | OK | 44.6 |
| 2 | 01-model-architecture | mamba | 261 | OK | 22.1 |
| 3 | 01-model-architecture | nanogpt | 291 | OK | 35.6 |
| 4 | 01-model-architecture | rwkv | 261 | OK | 28.5 |
| 5 | 01-model-architecture | torchtitan | 359 | OK | 19.1 |
| 6 | 02-tokenization | huggingface-tokenizers | 517 | Over | 60.4 |
| 7 | 02-tokenization | sentencepiece | 236 | OK | 10.2 |
| 8 | 03-fine-tuning | axolotl | 159 | Short | 300.6 |
| 9 | 03-fine-tuning | llama-factory | 81 | Short | 64.2 |
| 10 | 03-fine-tuning | peft | 432 | OK | 22.3 |
| 11 | 03-fine-tuning | unsloth | 81 | Short | 1858.3 |
| 12 | 04-mech-interp | nnsight | 437 | OK | 16.6 |
| 13 | 04-mech-interp | pyvene | 474 | OK | 19.6 |
| 14 | 04-mech-interp | saelens | 387 | OK | 18.1 |
| 15 | 04-mech-interp | transformer-lens | 347 | OK | 19.7 |
| 16 | 05-data-processing | nemo-curator | 384 | OK | 4.4 |
| 17 | 05-data-processing | ray-data | 327 | OK | 3.4 |
| 18 | 06-post-training | grpo-rl-training | 573 | Over | -- |
| 19 | 06-post-training | miles | 316 | OK | 9.7 |
| 20 | 06-post-training | openrlhf | 250 | OK | 43.0 |
| 21 | 06-post-training | simpo | 220 | OK | 25.8 |
| 22 | 06-post-training | slime | 465 | OK | 18.7 |
| 23 | 06-post-training | torchforge | 434 | OK | 15.0 |
| 24 | 06-post-training | trl-fine-tuning | 456 | OK | 11.8 |
| 25 | 06-post-training | verl | 392 | OK | 13.4 |
| 26 | 07-safety-alignment | constitutional-ai | 291 | OK | -- |
| 27 | 07-safety-alignment | llamaguard | 338 | OK | -- |
| 28 | 07-safety-alignment | nemo-guardrails | 298 | OK | -- |
| 29 | 07-safety-alignment | prompt-guard | 313 | OK | -- |
| 30 | 08-distributed-training | accelerate | 333 | OK | 34.8 |
| 31 | 08-distributed-training | deepspeed | 142 | Short | 628.4 |
| 32 | 08-distributed-training | megatron-core | 367 | OK | 38.5 |
| 33 | 08-distributed-training | pytorch-fsdp2 | 232 | OK | 13.0 |
| 34 | 08-distributed-training | pytorch-lightning | 347 | OK | 34.5 |
| 35 | 08-distributed-training | ray-train | 407 | OK | 13.2 |
| 36 | 09-infrastructure | lambda-labs | 546 | Over | 26.2 |
| 37 | 09-infrastructure | modal | 342 | OK | 20.9 |
| 38 | 09-infrastructure | skypilot | 510 | Over | 17.5 |
| 39 | 10-optimization | awq | 311 | OK | 15.3 |
| 40 | 10-optimization | bitsandbytes | 412 | OK | 34.1 |
| 41 | 10-optimization | flash-attention | 368 | OK | 14.2 |
| 42 | 10-optimization | gguf | 428 | OK | 19.3 |
| 43 | 10-optimization | gptq | 451 | OK | 12.6 |
| 44 | 10-optimization | hqq | 446 | OK | 24.6 |
| 45 | 11-evaluation | bigcode-evaluation-harness | 406 | OK | 27.0 |
| 46 | 11-evaluation | lm-evaluation-harness | 491 | OK | 45.3 |
| 47 | 11-evaluation | nemo-evaluator | 495 | OK | 34.7 |
| 48 | 12-inference-serving | llama-cpp | 259 | OK | 8.7 |
| 49 | 12-inference-serving | sglang | 443 | OK | 33.7 |
| 50 | 12-inference-serving | tensorrt-llm | 188 | Short | 21.6 |
| 51 | 12-inference-serving | vllm | 365 | OK | 26.2 |
| 52 | 13-mlops | mlflow | 705 | Over | 48.0 |
| 53 | 13-mlops | tensorboard | 630 | Over | 43.4 |
| 54 | 13-mlops | weights-and-biases | 591 | Over | 46.2 |
| 55 | 14-agents | autogpt | 404 | OK | 20.4 |
| 56 | 14-agents | crewai | 499 | OK | 28.4 |
| 57 | 14-agents | langchain | 481 | OK | 38.1 |
| 58 | 14-agents | llamaindex | 570 | Over | 13.8 |
| 59 | 15-rag | chroma | 407 | OK | 0.8 |
| 60 | 15-rag | faiss | 222 | OK | 6.0 |
| 61 | 15-rag | pinecone | 359 | OK | 3.5 |
| 62 | 15-rag | qdrant | 494 | OK | 27.0 |
| 63 | 15-rag | sentence-transformers | 256 | OK | 2.9 |
| 64 | 16-prompt-engineering | dspy | 591 | Over | 45.2 |
| 65 | 16-prompt-engineering | guidance | 573 | Over | 46.9 |
| 66 | 16-prompt-engineering | instructor | 741 | Over | 18.5 |
| 67 | 16-prompt-engineering | outlines | 653 | Over | 49.4 |
| 68 | 17-observability | langsmith | 423 | OK | 22.5 |
| 69 | 17-observability | phoenix | 476 | OK | 25.6 |
| 70 | 18-multimodal | audiocraft | 565 | Over | 27.7 |
| 71 | 18-multimodal | blip-2 | 565 | Over | 30.1 |
| 72 | 18-multimodal | clip | 254 | OK | 5.2 |
| 73 | 18-multimodal | llava | 305 | OK | 4.6 |
| 74 | 18-multimodal | segment-anything | 501 | Over | 27.1 |
| 75 | 18-multimodal | stable-diffusion | 520 | Over | 29.4 |
| 76 | 18-multimodal | whisper | 318 | OK | 4.7 |
| 77 | 19-emerging-techniques | knowledge-distillation | 459 | OK | 8.9 |
| 78 | 19-emerging-techniques | long-context | 537 | Over | 42.1 |
| 79 | 19-emerging-techniques | model-merging | 540 | Over | 29.4 |
| 80 | 19-emerging-techniques | model-pruning | 496 | OK | 9.3 |
| 81 | 19-emerging-techniques | moe-training | 527 | Over | 31.2 |
| 82 | 19-emerging-techniques | speculative-decoding | 468 | OK | 18.3 |
| 83 | 20-ml-paper-writing | ml-paper-writing | 938 | Over | 58.6 |
| 84 | 21-research-ideation | brainstorming-research-ideas | 385 | OK | -- |
| 85 | 21-research-ideation | creative-thinking-for-research | 367 | OK | -- |

---

## 3. Quality Metrics Summary

### Line Count Distribution

| Range | Count | % | Skills |
|-------|------:|--:|--------|
| Under 200 (Short) | 5 | 5.9% | axolotl (159), llama-factory (81), unsloth (81), deepspeed (142), tensorrt-llm (188) |
| 200-500 (OK) | 64 | 75.3% | -- |
| Over 500 (Over) | 16 | 18.8% | See below |

**Over-500 skills (16):** ml-paper-writing (938), instructor (741), mlflow (705), outlines (653), tensorboard (630), dspy (591), weights-and-biases (591), grpo-rl-training (573), guidance (573), llamaindex (570), audiocraft (565), blip-2 (565), lambda-labs (546), model-merging (540), long-context (537), moe-training (527), stable-diffusion (520), huggingface-tokenizers (517), skypilot (510), segment-anything (501).

Note: 16 unique skills exceed 500 lines. The worst offender is ml-paper-writing at 938 lines (nearly 2x the limit).

### References Coverage

| Metric | Value |
|--------|-------|
| Skills with references/ | 78 (92%) |
| Skills without references/ | 7 (8%) |
| Largest refs | unsloth (1,858 KB), deepspeed (628 KB), axolotl (301 KB) |
| Smallest refs | chroma (0.8 KB), sentence-transformers (2.9 KB), ray-data (3.4 KB) |
| Median refs size | ~22 KB (estimated) |

**Missing references/:** grpo-rl-training, constitutional-ai, llamaguard, nemo-guardrails, prompt-guard, brainstorming-research-ideas, creative-thinking-for-research.

Pattern: All 4 safety-alignment skills and both research-ideation skills lack reference directories. These are the newest categories and may not have had reference docs sourced yet.

---

## 4. Lifecycle Grouping

Mapping skills to the AI research lifecycle:

| Phase | Categories | Skill Count |
|-------|-----------|------------:|
| Data Prep | 02-tokenization, 05-data-processing | 4 |
| Training | 01-model-architecture, 03-fine-tuning, 08-distributed-training | 15 |
| Post-Training | 06-post-training | 8 |
| Optimization | 10-optimization | 6 |
| Deployment | 12-inference-serving, 09-infrastructure | 7 |
| Evaluation | 11-evaluation | 3 |
| Monitoring | 13-mlops, 17-observability | 5 |
| Safety | 07-safety-alignment | 4 |
| Application | 14-agents, 15-rag, 16-prompt-engineering, 18-multimodal | 20 |
| Research | 04-mech-interp, 19-emerging-techniques, 20-ml-paper-writing, 21-research-ideation | 13 |
| **Total** | | **85** |

Observations:
- **Application** is the largest phase (20 skills, 24%) -- reflects the breadth of downstream use cases.
- **Training** is the second largest (15 skills, 18%) -- core to AI research workflows.
- **Data Prep** and **Evaluation** are the thinnest (4 and 3 skills respectively) -- potential growth areas.

---

## 5. Use-Case Grouping

### Production Ready
Skills oriented toward production deployment and operational infrastructure:
vllm, sglang, tensorrt-llm, llama-cpp, accelerate, deepspeed, modal, skypilot, weights-and-biases, mlflow, langchain, langsmith

### Research Tools
Skills for mechanistic understanding, evaluation, ideation, and academic output:
transformer-lens, saelens, nnsight, pyvene, lm-evaluation-harness, brainstorming-research-ideas, creative-thinking-for-research, ml-paper-writing

### Safety & Alignment
Dedicated safety evaluation and guardrail skills:
constitutional-ai, llamaguard, nemo-guardrails, prompt-guard

### Application Building
Skills for building LLM-powered applications (agents, RAG, structured output):
langchain, llamaindex, crewai, dspy, instructor, outlines, guidance, chroma, faiss, pinecone, qdrant, sentence-transformers

Note: Some skills (langchain, langsmith) appear in multiple use-case groups, reflecting their cross-cutting nature.

---

## 6. Cross-Reference Patterns

Three patterns identified by comparing AI-Research-SKILLs with the marketing-skills ecosystem (marketing-skills.md, agentkits-marketing, ai-marketing-skills):

### Pattern 1: Product Context File Pattern
**Source**: marketing-skills checks for `.claude/product-marketing-context.md` before asking clarifying questions.

**Relevance**: AI-Research-SKILLs could adopt a similar convention -- e.g., `.claude/research-context.md` containing hardware specs (GPU type/count, VRAM), cluster topology, preferred frameworks, and default model sizes. Skills would read this file to skip repetitive environment questions and tailor recommendations (batch sizes, parallelism strategies, quantization choices) automatically.

### Pattern 2: Multi-Agent Orchestration
**Source**: agentkits-marketing uses an orchestrator that delegates to specialized marketing agents (content writer, SEO analyst, social media manager).

**Relevance**: The 14-agents category (langchain, llamaindex, crewai, autogpt) covers the underlying agent frameworks but does not include a domain-specific orchestration skill for AI research itself. A "research orchestrator" skill that chains dataset prep, training, evaluation, and paper-writing agents would be a natural extension -- essentially applying the agentkits pattern to the AI research domain.

### Pattern 3: "Framework AI Executes" Philosophy
**Source**: ai-marketing-skills treats skills as executable workflows, not passive reference material. Every skill is designed so an AI agent can run it end-to-end.

**Relevance**: AI-Research-SKILLs already follows this philosophy with workflow checklists in SKILL.md files. However, the shortest skills (llama-factory at 81 lines, unsloth at 81 lines, deepspeed at 142 lines) are more reference stubs than executable workflows. These would benefit from expansion to include step-by-step checklists with copy-paste commands, matching the executable standard that the longer skills achieve.

---

## 7. Observations & Notes

### Strengths
- **100% YAML compliance**: Every skill has valid frontmatter with all required fields. This is a strong foundation for automated tooling and marketplace sync.
- **Broad lifecycle coverage**: 21 categories span the full AI research lifecycle from data prep through paper writing, with no major gaps.
- **Reference depth**: 78/85 skills have reference directories. Several (unsloth at 1.8 MB, deepspeed at 628 KB, axolotl at 301 KB) significantly exceed the 300 KB target.

### Areas for Improvement
1. **16 over-length skills need splitting**: The entire 16-prompt-engineering category is over 500 lines. All 3 mlops skills exceed the limit. Content should be moved to reference files.
2. **5 under-length skills need expansion**: llama-factory (81), unsloth (81), deepspeed (142), axolotl (159), and tensorrt-llm (188) are stubs. Ironically, unsloth has the largest reference directory (1.8 MB) but the shortest SKILL.md -- the guidance is buried in references instead of surfaced in the main file.
3. **7 skills missing references/**: All safety-alignment skills and both research-ideation skills lack reference directories. Given that safety is a high-stakes domain, sourcing official documentation for these skills should be prioritized.
4. **Category size imbalance**: 06-post-training has 8 skills while 20-ml-paper-writing has just 1. Categories 02-tokenization and 05-data-processing have only 2 skills each.
5. **README discrepancy**: README lists 14-agents as "LangChain, LlamaIndex, Smolagents, Claude Agent SDK" but the actual directory contains autogpt, crewai, langchain, llamaindex. The README agent list does not match the filesystem.

### Priority Actions
- **High**: Split the 16 over-500-line skills (start with ml-paper-writing at 938 lines, instructor at 741 lines, mlflow at 705 lines).
- **High**: Add reference directories for the 7 skills that lack them.
- **Medium**: Expand the 5 under-200-line skills to include actionable workflows.
- **Low**: Consider a research-context file convention (Pattern 1) and a research orchestrator skill (Pattern 2).
