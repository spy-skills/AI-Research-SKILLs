---
date: 2026-03-26
type: task-worklog
task: skills-0mz
title: "Deep Dive Coordinator — Tổng hợp 4 batches"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [coordinator, aggregation, summary, learning-path]
---

# Deep Dive Coordinator — Detailed Design

## 1. Objective
Tổng hợp kết quả từ 4 batch tasks (20 analysis.md files), verify quality, tạo summary table, và generate learning path recommendation.

## 2. Scope
**In-scope:**
- Verify 20 analysis.md files tồn tại + đủ chất lượng
- Tạo summary.md tổng hợp với bảng so sánh
- Cross-check scores với ranking (skills-0kv)
- Generate learning path (Tier 1 → 2 → 3)

**Out-of-scope:**
- KHÔNG đọc lại SKILL.md gốc (đã done ở batches)
- KHÔNG deep dive thêm skills mới
- KHÔNG push lên Notion/Trello (đó là skills-tno)

## 3. Input / Output
**Input:**
- 20 files: `self-explores/learn/{skill-name}/analysis.md`
- `self-explores/learn/priority-ranking/analysis.md` (ranking reference)

**Output:**
- `self-explores/learn/summary.md` — Master summary
- Updated: `self-explores/learn/priority-ranking/analysis.md` (cross-check notes)

## 4. Dependencies
- skills-kik (Batch 1), skills-ssy (Batch 2), skills-hn9 (Batch 3), skills-i8b (Batch 4)

## 5. Flow xử lý

### Step 1: Verify 20 analysis files (~10 phút)
```bash
find self-explores/learn/*/analysis.md | wc -l  # Must = 20
# Per file: check ≥50 lines, has score table, has 4 sections
for f in self-explores/learn/*/analysis.md; do
  echo "$(wc -l < "$f") $f"
done
```
**Quality checks:**
- [ ] 20 files exist
- [ ] Each ≥50 lines
- [ ] Each has `## Đánh giá`, `## Ưu điểm`, `## Nhược điểm`, `## Khả năng tích hợp`, `## Ghi chú kỹ thuật`
- [ ] Score table present in each

Flag any failing files → may need re-do.

**Verify:** 20/20 pass all checks.

### Step 2: Aggregate scores (~10 phút)
Extract scores from all 20 files into master table:

| # | Skill | Category | C | U | M | S | Weighted | Tier | Integration Priority |
|---|-------|----------|---|---|---|---|----------|------|---------------------|

Sort by weighted descending.

### Step 3: Cross-check with ranking (~5 phút)
Compare deep dive scores with original ranking (skills-0kv):
- Flag discrepancies >1.0 point
- Note if any skill moved tiers after deep dive
- Update ranking notes if needed

### Step 4: Identify patterns (~10 phút)
- Which categories consistently score high/low?
- Common strengths across top skills
- Common gaps across all skills
- Integration readiness: how many are "plug and play" for Claude Code?

### Step 5: Generate learning path (~10 phút)
```
LEARNING PATH RECOMMENDATION

Phase 1: Foundation (Tier 1 skills, ~2 days)
  1. {skill} — {why first}
  2. {skill} — {builds on #1}
  ...

Phase 2: Production Skills (Tier 1-2, ~3 days)
  6. {skill} — {why now}
  ...

Phase 3: Specialization (Tier 2, ~2 days)
  11. {skill} — {niche value}
  ...
```

### Step 6: Write summary.md (~15 phút)
Create `self-explores/learn/summary.md` with:
- Executive summary (5 bullets)
- Master score table (20 rows)
- Patterns & insights
- Learning path
- Integration readiness matrix

**Verify:** File exists, has all sections, learning path covers 20 skills.

## 6. Edge Cases & Error Handling
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| <20 analysis files | Batch incomplete | Flag missing, list which | Block until batches fixed |
| Score discrepancy >1.5 | Deep dive vs ranking disagree | Re-evaluate that skill | Note discrepancy + rationale |
| File exists but <50 lines | Low quality batch output | Flag for re-do | List specific gaps |

## 7. Acceptance Criteria
- **Happy 1:** Given 20 analysis.md files When aggregated Then summary.md has 20-row table sorted by score
- **Happy 2:** Given learning path When reviewed Then covers all 20 skills in logical order with rationale
- **Negative:** Given <20 files When coordinator runs Then blocks + lists missing files

## 8. Technical Notes
- ~60 phút total (read 20 short files + write 1 summary)
- No heavy computation — this is a synthesis task
- summary.md is the KEY deliverable that feeds into skills-tno (Notion upload)

## 9. Risks
- Low: Straightforward aggregation task
- Minor: If batch scores are inconsistent, reconciliation takes extra time

## Phản biện (2026-03-26)
*(Incorporated into detailed design above)*

## Worklog
*(Chưa bắt đầu)*
