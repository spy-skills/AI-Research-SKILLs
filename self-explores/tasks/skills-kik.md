---
date: 2026-03-26
type: task-worklog
task: skills-kik
title: "Deep Dive Batch 1 — Skills #1-5 (top priority)"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [deep-dive, batch-1, analysis, template]
---

# Deep Dive Batch 1 — Skills #1-5 — Detailed Design

**NOTE:** Đây là TEMPLATE BATCH — Batches 2-4 (skills-ssy, skills-hn9, skills-i8b) follow format identical.

## 1. Objective
Deep dive 5 skills ưu tiên cao nhất (từ ranking skills-0kv). Mỗi skill tạo 1 analysis.md file đủ 4 sections, ≥50 dòng, actionable cho integration vào Claude Code / OMC.

## 2. Scope
**In-scope:**
- Đọc SKILL.md toàn bộ cho 5 skills
- Đọc references/README.md (nếu tồn tại, skip nếu >100KB)
- Viết 5 analysis.md files theo format chuẩn
- Đánh giá tích hợp Claude Code + OMC cụ thể

**Out-of-scope:**
- KHÔNG đọc references/api.md, issues.md, releases.md (quá chi tiết cho analysis)
- KHÔNG sửa SKILL.md gốc
- KHÔNG so sánh giữa skills (đó là coordinator task)

## 3. Input / Output
**Input:**
- `self-explores/learn/priority-ranking/analysis.md` — Top 20 list
- 5 SKILL.md files (xác định sau khi ranking xong)
- 5 references/README.md files (nếu có)

**Output:**
- 5 files: `self-explores/learn/{skill-name}/analysis.md`
- Format per file: xem Step 3 bên dưới

## 4. Dependencies
- skills-0kv (Ranking) phải xong — để biết top 5 skills nào

## 5. Flow xử lý

### Step 1: Extract top 5 skills (~5 phút)
```bash
# Đọc ranking file, lấy 5 skills đầu tiên (highest weighted score)
```
Ghi lại 5 skill names + categories + scores vào worklog.

**Verify:** 5 skills extracted, all from Tier 1.

### Step 2: Deep dive mỗi skill (~15 phút × 5 = 75 phút)

Per skill workflow:

**2a. Read SKILL.md toàn bộ**
```bash
wc -l {category}/{skill}/SKILL.md  # Note line count
```
Đọc full file. Ghi chú:
- Core workflow (what does it do?)
- Key APIs / commands
- Unique value proposition
- Known limitations

**2b. Read references/README.md (nếu có)**
```bash
ls {category}/{skill}/references/README.md 2>/dev/null
wc -l {category}/{skill}/references/README.md 2>/dev/null  # Skip if >3000 lines
```
Chỉ đọc nếu <100KB. Ghi chú bổ sung.

**2c. Evaluate 4 criteria (thang 1-5, thống nhất với ranking)**
- Completeness (C): workflows, edge cases, troubleshooting
- Usability (U): copy-paste ready, progressive disclosure
- Modularity (M): ref separation, reusable
- Safety (S): warnings, mitigations

**2d. Assess Claude Code / OMC integration**
- Dùng như skill trong Claude Code? (auto-trigger, manual trigger?)
- Tích hợp vào OMC workflow nào? (autopilot, team, executor?)
- Cần thêm gì để production-ready?

### Step 3: Write analysis.md per skill (~5 phút × 5 = 25 phút)

```bash
mkdir -p self-explores/learn/{skill-name}
```

**Format chuẩn (PHẢI tuân thủ):**

```markdown
---
date: 2026-03-26
type: skill-analysis
skill: {skill-name}
category: {category-name}
score: {weighted-total}/5
tags: [{tags from SKILL.md}]
---

# {Skill Name}

## Đánh giá (Score {X}/5)

| Criteria | Score | Weight | Contribution |
|----------|-------|--------|-------------|
| Completeness | {C}/5 | 30% | {C×0.3} |
| Usability | {U}/5 | 30% | {U×0.3} |
| Modularity | {M}/5 | 20% | {M×0.2} |
| Safety | {S}/5 | 20% | {S×0.2} |
| **Weighted Total** | | | **{total}/5** |

**Summary:** {1-2 câu đánh giá tổng thể}

## Ưu điểm
- {3-5 bullets, cụ thể — trích dẫn sections/features hay}

## Nhược điểm
- {3-5 bullets, cụ thể — gì thiếu, gì mơ hồ, gì outdated}

## Khả năng tích hợp Claude Code / OMC

### Trigger conditions
- {Khi nào skill này nên auto-activate trong Claude Code}

### Integration points
- {Cách dùng trong OMC autopilot/team workflow}
- {Cần sửa/thêm gì để dùng được}

### Priority
- {High/Medium/Low} — {lý do}

## Ghi chú kỹ thuật
- **Dependencies:** {packages + versions từ YAML}
- **Line count:** {N} lines SKILL.md + {M} lines refs
- **Known issues:** {nếu có}
- **Alternatives:** {skills khác cùng chức năng}
```

**Verify:** File ≥50 dòng, đủ 4 sections chính, score table present.

## 6. Edge Cases & Error Handling
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| SKILL.md >500 lines | Over-length skill | Read full, note "over-length" | Don't skip content |
| No references/ dir | Skill without refs | Score Modularity lower | Note in analysis |
| references/README.md >100KB | Very large ref file | Skip ref, note "too large" | Only analyze SKILL.md |
| Skill name has special chars | Path issues | Use kebab-case | Sanitize for mkdir |

## 7. Acceptance Criteria
- **Happy 1:** Given top 5 skills When deep dive completes Then 5 analysis.md files exist, each ≥50 lines
- **Happy 2:** Given analysis.md When reviewed Then has 4 main sections + score table + integration assessment
- **Negative:** Given SKILL.md without references/ When analyzed Then Modularity score ≤3 and "no refs" noted

## 8. Technical Notes
- ~90 phút realistic estimate (15 min read + assess + 5 min write per skill)
- Score table MUST use same rubric as skills-0kv for consistency
- Integration assessment focuses on Claude Code skill triggering + OMC agent routing
- Output dir: `self-explores/learn/{skill-name}/` — lowercase kebab-case

## 9. Risks
- Context pressure after reading 5 full SKILL.md files (~2500 lines). Mitigation: Write each analysis immediately after reading, don't batch.
- Score inconsistency with ranking. Mitigation: Cross-check against skills-0kv scores during write.

## Worklog
*(Chưa bắt đầu)*
