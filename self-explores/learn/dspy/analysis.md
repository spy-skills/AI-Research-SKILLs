---
date: 2026-03-26
type: skill-analysis
skill: dspy
category: prompt-engineering
score: 4.4/5
tags: [Prompt Engineering, DSPy, Declarative Programming, RAG, Agents]
---

# DSPy

## Đánh giá (Score 4.4/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 4.5/5 | 20% | 0.90 |
| Safety | 3.5/5 | 20% | 0.70 |
| **Weighted Total** | | | **4.30/5** |

**Summary:** DSPy is a standout skill covering Stanford NLP's declarative LM programming framework with exceptional breadth — signatures, modules (Predict, ChainOfThought, ReAct, ProgramOfThought), and three major optimizer classes (BootstrapFewShot, MIPRO, BootstrapFinetune). The skill is well-structured with progressive complexity, moving from quick starts to multi-stage pipelines, and includes a useful comparison table versus manual prompting and LangChain. Minor gaps exist around safety/guardrails considerations and token cost awareness during optimization loops.

## Ưu điểm
- **Breadth of coverage**: All four major module types (Predict, ChainOfThought, ReAct, ProgramOfThought) with concrete code for each, plus three optimizer types with working examples — a genuine one-stop reference.
- **Multiple LM providers**: Explicit configuration snippets for Anthropic Claude, OpenAI, and local Ollama models, including a multi-model context-switch pattern using `dspy.settings.context()`.
- **Structured output with Pydantic**: Shows `TypedPredictor` with a Pydantic `BaseModel`, directly addressing a common pain point when integrating LM outputs into typed systems.
- **Actionable best practices**: The five "Best Practices" entries cover the full lifecycle — start simple, descriptive signatures, diverse training data, save/load, and tracing — each with a runnable code block.
- **Comparison table**: The Feature vs. Manual/LangChain/DSPy table and the "When to choose" section give agents clear decision guidance, avoiding over-use of a complex framework for simple tasks.

## Nhược điểm
- **No cost/latency guidance for optimizers**: MIPRO with `num_trials=100` and BootstrapFewShot can make hundreds of LM calls. The skill doesn't mention approximate API cost, token budgeting, or how to set `max_bootstrapped_demos` conservatively.
- **Safety section missing**: No mention of prompt injection risks when user-controlled inputs flow into DSPy signatures, assertion bypass strategies, or output validation beyond `isinstance()` checks.
- **Assertion backtrack handler is opaque**: The `assert_transform_module` / `backtrack_handler` pattern in Pattern 2 is shown but not explained — unclear when backtracking loops could become infinite or expensive.
- **SKILL.md slightly over-length**: At 591 lines the file exceeds the stated 500-line guideline for SKILL.md, with redundant code duplication between the "Quick Start" and "Core Concepts" sections.
- **No YAML frontmatter lint note**: The `tags` array has 10 entries; the project standard uses 5 or fewer in the template and analysis format.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User asks to "optimize prompts automatically" or "build a RAG pipeline with automatic prompt tuning"
- Task involves iterating on prompt quality with a labeled dataset (few-shot examples or ground truth answers)
- Agent is building a multi-stage LM pipeline that needs modular components (retrieve → reason → answer)
- Request mentions "DSPy", "BootstrapFewShot", "MIPRO", or "declarative LM programming"

### Integration points
- **OMC `executor` agent**: Can invoke DSPy optimizer workflows as part of autonomous experiment pipelines (align with `06-post-training/grpo-rl-training/` for reward signal sourcing)
- **OMC `planner` agent**: DSPy signatures map naturally to OMC task decomposition — each signature is an atomic subtask with typed inputs/outputs
- **OMC `verifier` agent**: DSPy's `Evaluate` class with custom metrics serves as the verification layer in an OMC `team-verify` stage
- **`ralph` / `ultrawork` mode**: Can run BootstrapFewShot optimization loops with automatic metric-driven iteration, fitting OMC's "don't stop until verified" paradigm

### Priority
- **High** — DSPy is the primary skill for building optimized multi-step LM workflows in research experiments; strong synergy with the broader skill library (RAG skills in `15-rag/`, evaluation in `11-evaluation/`)

## Ghi chú kỹ thuật
- **Dependencies:** `dspy`, `openai`, `anthropic`
- **Line count:** 591 lines (exceeds 500-line guideline — split candidate)
- **Known issues:** MIPRO optimization can exhaust API rate limits; no retry/backoff guidance in skill; `ProgramOfThought` executes generated Python code (code injection risk not addressed)
- **Alternatives:** LangChain (simpler, no automatic optimization), LlamaIndex (RAG-focused), Guidance/Outlines (structured output only), Instructor (structured output with Pydantic, simpler)
