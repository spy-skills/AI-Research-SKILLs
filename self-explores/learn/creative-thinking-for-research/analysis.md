---
date: 2026-03-26
type: skill-analysis
skill: creative-thinking-for-research
category: 21-research-ideation
score: 4.1/5
tags: [Creative Thinking, Research Ideation, Analogical Reasoning, Problem Reformulation, Cognitive Science]
---

# Creative Thinking for Research

## Đánh giá (Score 4.1/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | 4.5/5 | 30% | 1.35 |
| Usability | 4.0/5 | 30% | 1.20 |
| Modularity | 3.5/5 | 20% | 0.70 |
| Safety | 3.5/5 | 20% | 0.70 |
| **Weighted Total** | | | **3.95/5** |

**Summary:** This skill is intellectually the most sophisticated in the entire library, grounding 8 creativity frameworks in named cognitive science theories (Koestler's bisociation, Kaplan & Simon, Gentner's structure-mapping, Boden's three types, De Bono's lateral thinking, Polya's heuristics, Kauffman's adjacent possible, Rothenberg's Janusian thinking). The "Current Adjacent Possibles 2025-2026" table and the "Negation Hall of Fame in CS" are standout contributions that are both concrete and immediately applicable. Its limitation is that it is the hardest skill in the library to operationalize for an AI agent — the outputs are cognitive insights, not verifiable artifacts.

## Ưu điểm
- **Academic grounding with named theorists**: Each framework cites specific cognitive science research (Koestler, Gentner, Dunbar, Boden, TRIZ, Polya, Kauffman, Rothenberg). This makes the skill credible and allows users to trace ideas to primary sources — unusual for a practical skills library.
- **"Negation Hall of Fame in CS" table**: Eight concrete examples of assumption negation that produced major CS advances (eventual consistency, sketches/LSH, self-supervised learning, MoE, online learning, speculative decoding) is an exceptional reference that grounds abstract creative thinking in real history.
- **"Current Adjacent Possibles 2025-2026" table**: Six concrete examples of recent enablers and what they now make possible — the only forward-looking table in any skill in the library, and genuinely useful for orienting research in 2025-2026.
- **Three-tier analogical depth taxonomy**: Surface / Relational / Structural analogy levels with examples and the critical insight (Dunbar's finding that distant-domain analogies drive major discoveries while nearby analogies only refine ideas) is a high-value cognitive model rarely articulated this clearly.
- **TRIZ mapping to CS**: The TRIZ principles (Inversion, Segmentation, Merging, Universality, Nesting, Dynamization) mapped to CS applications is a novel cross-domain import that most ML researchers will have never encountered.

## Nhược điểm
- **No worked example from beginning to end**: The skill presents eight frameworks and a combined protocol but never walks through a complete example from "I am stuck on X" to "I now have a concrete research hypothesis Y." A single end-to-end example would dramatically increase usability.
- **Frameworks 1 and 3 overlap significantly**: Bisociation (F1) and Analogical Reasoning (F3) address the same cognitive operation (cross-domain connection) with only the level of formalization differing. The distinction between "bisociation" and "structure-mapping" is not explained clearly enough to help users choose between them in practice.
- **384-line count is high**: File is 367 lines (checked manually), very close to the 500-line limit. The TRIZ table and several extensive example tables are valuable but could move to a reference file to make room for worked examples.
- **No quantitative output format**: Like its sister skill, this produces narrative insights and checklists but no structured artifact. An agent cannot verify that a user has successfully applied Framework 4 (Constraint Manipulation) without the user confirming it.
- **Relationship to brainstorming skill is asymmetric**: The "Relationship to Brainstorm skill" section says "use them together: creative-thinking to generate raw insight, brainstorm to structure and evaluate it" but the handoff protocol is vague — what is the specific output of creative-thinking that becomes the input to brainstorming? This integration seam needs explicit specification.

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- User says "I feel stuck in a local optimum" or "all my ideas feel incremental"
- User asks for "genuinely novel" research directions (not just extensions)
- User is preparing for a PhD defense or research retreat
- User mentions wanting to "think differently" or break out of conventional approaches
- User explicitly asks about analogical reasoning, cross-domain inspiration, or lateral thinking
- After `brainstorming-research-ideas` session produces only incremental ideas — escalate to this skill

### Integration points
- **Upstream of `brainstorming-research-ideas/`**: This skill generates the raw creative leaps; brainstorming-research-ideas provides the evaluation filter (diverge → converge → refine). OMC should recommend both in sequence.
- **Upstream of `20-ml-paper-writing/`**: The "two-sentence test" from brainstorming skill is referenced here as a final evaluation gate — the output of creative thinking should be validated through that test before moving to paper writing
- **OMC `ralplan` integration**: In a deep planning session (ralplan with --deliberate), this skill could be invoked as a pre-mortem tool — "what alternative framings of this problem have we not considered?" before committing to an approach
- **`sciomc` parallel scientist workflow**: Multiple agents could each apply different frameworks (one does Bisociation, one does Negation, one does Problem Reformulation) in parallel, then synthesize results — the framework structure maps naturally to parallel agent execution

### Priority
- **Medium** — Needed less frequently than technical skills but provides uniquely high-value guidance when activated. The adjacent-possible table and negation hall-of-fame alone justify including this in any AI research ideation session. Best used at the start of a new research direction or when brainstorming has stalled.

## Ghi chú kỹ thuật
- **Dependencies:** [] (no software dependencies — purely conceptual/methodological)
- **Line count:** ~367 lines (within 200-500 target, near upper limit)
- **Known issues:** Frameworks 1 (Bisociation) and 3 (Analogical Reasoning) overlap — could confuse users about which to apply; no worked end-to-end example reduces immediate usability for first-time users
- **Alternatives:** brainstorming-research-ideas (more operational, less cognitive science), Design Thinking frameworks (broader but less CS-specific), TRIZ methodology (more systematic, engineering-focused), SCAMPER technique (simpler but less rigorous)
- **Theoretical grounding**: The cognitive science citations (Koestler 1964, Gentner 1983, Dunbar 1997, Boden 1990, Kauffman 1993, Rothenberg 1979) give this skill unusual academic depth — researchers can read the primary sources for deeper understanding
- **2025-2026 relevance**: The Adjacent Possible table is time-stamped and will need updating as the field evolves — this is the most time-sensitive content in the skill
