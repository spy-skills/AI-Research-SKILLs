---
date: 2026-03-26
type: task-worklog
task: skills-0kv
title: "Xếp hạng ưu tiên — Đánh giá 84 skills theo weighted scoring"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [ranking, priority, evaluation, weighted-scoring]
---

# Xếp hạng ưu tiên — Detailed Design

## 1. Objective
Đánh giá 84 skills theo 4 tiêu chí weighted (Completeness, Usability, Modularity, Safety/Trust) trên thang 1-5. Output: bảng ranking 84 rows, top 20 highlighted, 3 tiers, learning path.

## 2. Scope
**In-scope:**
- Đọc TOÀN BỘ SKILL.md mỗi skill (không chỉ header)
- Score 4 criteria theo rubric cụ thể
- Weighted total score
- Top 20 skills → input cho Deep Dive batches
- Learning path recommendation

**Out-of-scope:**
- KHÔNG đọc references/ (chỉ check tồn tại)
- KHÔNG sửa/cải thiện skills
- KHÔNG deep dive (đó là batch tasks)

## 3. Input / Output
**Input:**
- `self-explores/learn/overview/analysis.md` (inventory từ skills-482)
- 84 SKILL.md files across 21 categories

**Output:**
- `self-explores/learn/priority-ranking/analysis.md`
  - Bảng 84 rows × 7 columns (Skill, Cat, C, U, M, S, Weighted)
  - Top 20 highlighted
  - 3 Tiers (Must Learn / Should Learn / Nice to Know)
  - Learning path diagram

## 4. Dependencies
- skills-482 (Tổng quan) phải xong — cần inventory table

## 5. Flow xử lý

### Step 1: Load inventory (~5 phút)
Đọc `self-explores/learn/overview/analysis.md`, extract skill list.

### Step 2: Batch 1 scoring — Categories 01-06 (~25 phút)
~22 skills. Đọc mỗi SKILL.md toàn bộ. Score theo rubric:

**SCORING RUBRIC:**
| Score | Completeness (30%) | Usability (30%) | Modularity (20%) | Safety (20%) |
|-------|-------------------|-----------------|------------------|--------------|
| 1 | Stub/placeholder, <100 lines, no workflows | Unstructured, no examples | No references, monolithic | No safety mentions |
| 2 | Has content but gaps, no troubleshooting | Some structure, few examples | Has refs but poorly organized | Basic safety notes |
| 3 | Covers main workflow, some edge cases | Clear structure, copy-paste code | Good ref separation, 1-level deep | Safety section present |
| 4 | Workflows + troubleshooting + alternatives | Progressive disclosure, checklists | Well-separated, reusable refs | Safety warnings + mitigations |
| 5 | Gold standard: workflows + issues + real examples | Beginner-friendly + advanced paths | Perfect separation, cross-linkable | Comprehensive safety guidance |

**Weighted formula:** `Total = C×0.3 + U×0.3 + M×0.2 + S×0.2`

**Verify:** After batch, review scores for internal consistency.

### Step 3: Batch 2 scoring — Categories 07-12 (~25 phút)
~22 skills. Same rubric.

### Step 4: Batch 3 scoring — Categories 13-17 (~20 phút)
~18 skills. Same rubric.

### Step 5: Batch 4 scoring — Categories 18-21 (~20 phút)
~22 skills. Same rubric.

### Step 6: Compile & rank (~15 phút)
- Sort by weighted total descending
- Assign tiers:
  - Tier 1 (Must Learn): Weighted ≥ 4.0
  - Tier 2 (Should Learn): Weighted 3.0-3.9
  - Tier 3 (Nice to Know): Weighted < 3.0
- Highlight top 20
- Generate learning path: Tier 1 first → Tier 2 by category relevance

### Step 7: Write output (~10 phút)
Format: Markdown table + tier sections + learning path.

**Verify:** 84 rows, no duplicates, scores 1-5 range, top 20 = input for batch tasks.

## 6. Edge Cases & Error Handling
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| SKILL.md very short (<50 lines) | Stub skill | Score 1 across all criteria | Note "stub" |
| SKILL.md very long (>500 lines) | Not following standards | May score high C but low M | Note "over-length" |
| Tie in weighted score | Multiple skills same total | Sort alphabetically | Note tie |
| Context window pressure in batch | >20 skills read consecutively | Quality drops | Split batch smaller |

## 7. Acceptance Criteria
- **Happy 1:** Given 84 SKILL.md files When scoring completes Then table has 84 rows, all scores 1-5, weighted calculated correctly
- **Happy 2:** Given completed ranking When top 20 extracted Then exactly 20 skills highlighted, sorted by weighted desc
- **Negative:** Given a stub SKILL.md (<50 lines, no workflows) When scored Then receives ≤2 on all criteria

## 8. Technical Notes
- Persona: ML Engineer on RTX 3090, wants production-usable skills
- Rubric designed to favor actionable skills with code examples over theoretical ones
- Batching by category number prevents cross-contamination of scoring standards
- Gold standard benchmark: `06-post-training/grpo-rl-training/` (should score ~4.5+)

## 9. Risks
- Context exhaustion: Reading 84 full SKILL.md in batches. Mitigation: 4 batches, take breaks between
- Scoring drift: Later batches may be scored differently. Mitigation: Re-calibrate against gold standard after each batch
- Subjectivity: Different readers may score differently. Mitigation: Rubric is table-based, not prose-based

## Phản biện (2026-03-26)
*(Incorporated into detailed design above)*

## Worklog

### Hoàn thành — 4 batches parallel scoring
**Kết quả:**
- 85 skills scored (4 parallel agents, ~22 skills each)
- Score range: 1.0 — 4.8, Average: ~3.77
- Tier 1 (≥4.0): 30 skills | Tier 2 (3.0-3.9): 50 | Tier 3 (<3.0): 5
- Top scorer: ml-paper-writing 4.80
- 5 stubs identified: llama-factory, unsloth, deepspeed (1.0), axolotl (1.8), tensorrt-llm (2.6)
- Strongest category: 04-mech-interp (avg 4.33)
- Learning path: 3 phases (Foundation → Specialization → Research)

**Top 20 for deep dive:** ml-paper-writing, saelens, grpo-rl-training, llamaguard, llamaindex, dspy, peft, trl-fine-tuning, nnsight, pyvene, torchtitan, hf-tokenizers, bitsandbytes, bigcode-eval, lm-eval-harness, constitutional-ai, nemo-guardrails, prompt-guard, brainstorming-research, creative-thinking

**Files tạo:**
- self-explores/learn/priority-ranking/analysis.md

**Output mong đợi:**
- [x] Bảng 85 rows × 7 columns
- [x] Top 20 highlighted
- [x] 3 Tiers + Learning path
