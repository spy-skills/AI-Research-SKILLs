---
date: 2026-03-26
type: skill-analysis
skill: brainstorming-research-ideas
category: 21-research-ideation
score: 4.1/5
tags: [Research Ideation, Brainstorming, Problem Discovery, Creative Thinking, Research Strategy]
---

# Research Idea Brainstorming

## Đánh giá (Score 4.1/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.5/5 | 30% | 1.35 |
| Modularity | 3.5/5 | 20% | 0.70 |
| Safety | 3.0/5 | 20% | 0.60 |
| **Weighted Total** | | | **4.00/5** |

**Summary:** This is the most content-dense skill in the entire library at 385 lines, presenting 10 complementary ideation frameworks with detailed workflows, self-check checklists, decision tables, and a 3-phase integrated protocol. The skill goes significantly beyond generic brainstorming advice by grounding each framework in a specific cognitive mode and failure pattern. Its main limitation is that it is entirely conceptual — it generates no code, no data structures, and no machine-verifiable outputs, making it harder for an AI agent to confirm successful execution compared to technical skills.

## Ưu điểm
- **Ten distinct cognitive lenses**: Problem-First vs Solution-First, Abstraction Ladder, Tension Hunting, Cross-Pollination, What Changed, Failure Analysis, Simplicity Test, Stakeholder Rotation, Composition/Decomposition, and Explain-It Test — each targets a genuinely different cognitive failure mode.
- **Concrete failure/symptom/fix table**: The "Common Pitfalls" table (novelty without impact, incremental by default, complexity worship, echo chamber, stale assumptions, single-perspective bias, premature convergence) is immediately actionable diagnostic guidance.
- **Framework Selection Guide**: The "Your Situation → Start With" decision table gives users a direct entry point based on their current state, eliminating the paralysis of choosing among 10 frameworks.
- **3-phase Integrated Workflow**: Diverge (generate 10-20 ideas) → Converge (apply 5 filters to get 3-5) → Refine (sharpen winner with checklist) is a complete end-to-end research ideation protocol with clear completion criteria.
- **Agent Usage Instructions section**: The explicit "When a researcher asks for help brainstorming research ideas: steps 1-6" section makes this skill unusually well-suited for autonomous agent use — it specifies the exact interaction protocol.

## Nhược điểm
- **No code or structured output format**: The skill produces narrative ideas and checklists but no structured data (JSON, YAML, structured research proposal template) that an agent could programmatically validate, store, or pass to downstream skills.
- **Framework 5 (What Changed) lacks a current examples table**: Unlike Framework 4 (cross-pollination) which has a rich table of source fields and concepts, the "What Changed" framework has an abstract categories list but no current examples analogous to what `creative-thinking-for-research` provides (its "Current Adjacent Possibles 2025-2026" table is much more specific).
- **Safety score low because no adversarial use consideration**: Research ideation can be misused (e.g., generating ideas for harmful AI systems). The skill has no guidance on evaluating the ethical dimensions of generated ideas despite having a Stakeholder Rotation framework that includes "Ethicist" and "Adversary" perspectives.
- **Line count at 385 lines is near the 500-line limit**: The three-phase protocol and framework selection guide are valuable but push the file close to the maximum; future additions (e.g., a worked example from a real research problem) would require splitting content into references.
- **Cross-references are incomplete**: The skill references `scientific-skills:literature-review` in the "Do NOT use" section but this skill does not appear to exist in the library's 21 categories — a broken cross-reference that could confuse users.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User says "I want to find a research topic" or "I'm stuck on what to research next"
- User is starting a new project and has not yet defined a research question
- User mentions "brainstorming", "research direction", "what should I work on"
- User has finished reading papers and asks "what's missing in this field?"
- User is pivoting from one research area to another

### Integration points
- **Upstream of `20-ml-paper-writing/`**: This skill generates the research idea; the paper writing skill structures it into a publishable contribution — natural sequential pairing
- **Upstream of domain-specific skills**: Output of this skill (a concrete research question) defines which technical skill to invoke next (e.g., tension identified in training efficiency → `08-distributed-training/`, gap in safety alignment → `07-safety-alignment/`)
- **Pairs with `creative-thinking-for-research/`**: Explicit relationship documented in both skills. `creative-thinking-for-research` generates cognitive raw material; this skill provides the evaluation and selection protocol. OMC should offer both together.
- **OMC orchestration**: In a `ralplan` or `omc-plan` session, this skill could auto-activate in the planning phase when a user's goal is undefined or in the "exploration" stage of a research project

### Priority
- **Medium** — This skill is not needed for most coding/implementation tasks but is highly valuable for research-oriented users. The AI research audience of this library likely needs it at the start of projects. Low frequency of activation but high impact when triggered.

## Ghi chú kỹ thuật
- **Dependencies:** [] (no software dependencies — purely conceptual/methodological)
- **Line count:** 385 lines (high but within 500-line limit; no room for additions without splitting)
- **Known issues:** Cross-reference to `scientific-skills:literature-review` skill appears to reference a non-existent skill in this library; the "What Changed" framework lacks current concrete examples compared to the sister skill
- **Alternatives:** Creative Thinking for Research (same category, complementary — deeper cognitive science grounding), literature review + gap analysis tools (more systematic but less generative), research proposal templates (more structured but less exploratory)
- **Unique value**: The only skill in the library that addresses the pre-technical phase of research — generating and evaluating the idea itself before any tool or method selection
