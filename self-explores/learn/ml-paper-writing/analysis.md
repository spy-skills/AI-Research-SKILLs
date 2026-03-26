---
date: 2026-03-26
type: skill-analysis
skill: ml-paper-writing
category: 20-ml-paper-writing
score: 4.8/5
tags: [Academic Writing, NeurIPS, ICML, ICLR, ACL, AAAI]
---

# ML Paper Writing for Top AI Conferences

## Đánh giá (Score 4.8/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 5/5 | 30% | 1.50 |
| Usability | 5/5 | 30% | 1.50 |
| Modularity | 4/5 | 20% | 0.80 |
| Safety | 5/5 | 20% | 1.00 |
| **Weighted Total** | | | **4.80/5** |

**Summary:** This is the most comprehensive skill in the library — it covers every stage of the ML paper lifecycle from repo exploration through submission, with explicit citation safety protocols grounded in real academic risk (~40% AI hallucination rate). The writing philosophy section synthesizes guidance from 7 top researchers (Nanda, Farquhar, Gopen & Swan, Lipton, Steinhardt, Perez, Karpathy), making it uniquely authoritative. The one gap is that it lacks a "post-rejection analysis" workflow and doesn't address rebuttals in detail.

## Ưu điểm

- **Citation safety protocol is exceptional**: The skill opens with a mandatory anti-hallucination section, establishes a 5-step programmatic verification workflow (Exa MCP → Semantic Scholar → CrossRef DOI fetch), and provides explicit LaTeX placeholder templates for unverifiable citations. This directly prevents a real academic misconduct risk.
- **Multi-conference coverage with concrete specs**: The conference quick-reference table (NeurIPS 9pp, ICML 8pp, ICLR 9pp, ACL 8pp, AAAI 7pp, COLM 9pp) plus per-venue required sections (Broader Impact, LLM disclosure, Limitations) is immediately actionable — no guessing.
- **Workflow 0 (Starting from a Research Repo)** is uniquely agent-oriented: it instructs Claude to scan for `.bib` files, grep for existing citations, and treat those as high-signal starting points — a pattern not found in any other skill.
- **Gopen & Swan's 7 principles table** (subject-verb proximity, stress position, topic position, old-before-new, one-unit-one-function, action-in-verb, context-before-new) gives sentence-level rewriting guidance with explicit bad/good examples, making it actionable at edit time.
- **Format conversion workflow (Workflow 3)** explicitly warns against copying LaTeX preambles between templates — a non-obvious pitfall that would cause silent compilation failures — and provides a content-only migration strategy.

## Nhược điểm

- **No rebuttal workflow**: Major conferences (NeurIPS, ICLR, ICML) have rebuttal phases. The skill acknowledges "addressing previous reviews" for resubmissions but never covers how to write effective rebuttals to provisional rejections.
- **Template directory assumed to exist locally**: Workflow 4 does `cp -r templates/neurips2025/` but there's no documentation of how to initially obtain or update those templates if they're stale or missing.
- **Reviewer scoring section is descriptive only**: The 6-point NeurIPS scale is listed but there's no actionable guidance on how to self-assess a paper against these criteria before submission.
- **Line count 937**: This is the longest SKILL.md in the library, nearly 2x the recommended 500-line ceiling. Several sections (LaTeX template workflow, format conversion) could be moved to reference files to improve agent load performance.
- **Citation Python code uses synchronous requests**: The `doi_to_bibtex()` function has no timeout or retry logic; in production use this would silently fail on network errors.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User says "write a paper", "draft a paper from this repo", "submit to NeurIPS/ICML/ICLR/ACL/AAAI/COLM"
- User provides a research repository path with results and asks for writeup assistance
- User says "find citations for", "verify this reference", or pastes a BibTeX entry for checking
- User says "convert paper from X to Y format" or "resubmit to different venue"

### Integration points
- **OMC autopilot flow**: When triggered by autopilot, the `explore` agent should run Workflow 0 (repo scan) first, then `executor` handles the drafting. The citation verification step maps naturally to a `document-specialist` sub-task for each unverified reference.
- **team pipeline**: `team-plan` stage should run the "Define one-sentence contribution" confirmation step as a blocking task requiring human input before `team-exec` begins drafting sections in parallel.
- **MCP integration**: The skill already documents Exa MCP installation. OMC should auto-install Exa MCP (`claude mcp add exa`) at skill activation time if not already present, since citation workflow depends on it.
- **Production gap**: The skill needs a `post-draft` verification hook that automatically counts line citations marked `[CITATION NEEDED]` and surfaces them as a checklist before the scientist reviews the draft.

### Priority
- **High** — This is the only skill in the library covering academic paper output, a high-value terminal action for AI research workflows. It has the broadest surface area for integration with OMC's `deep-executor` agent for fully autonomous paper drafting.

## Ghi chú kỹ thuật
- **Dependencies:** semanticscholar, arxiv, habanero, requests
- **Line count:** 937 lines (exceeds 500-line target by ~87%)
- **Known issues:** No retry/timeout on `doi_to_bibtex()` HTTP call; template directory must be locally present
- **Alternatives:** Overleaf AI (no citation verification), Paperpal (no agent API), ScholarAI (API-only, no LaTeX templates)
