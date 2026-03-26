---
date: 2026-03-26
type: task-worklog
task: skills-482
title: "Phân tích Tổng quan — Liệt kê & phân nhóm toàn bộ skills"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [analysis, overview, inventory, cross-reference]
---

# Phân tích Tổng quan — Detailed Design

## 1. Objective
Tạo inventory đầy đủ tất cả skills trong repo AI-Research-SKILLs (84 skills, 21 categories), đánh giá metadata quality, phân nhóm theo lifecycle + use-case, và cross-reference với marketing-skills.md để rút ra patterns tái sử dụng.

## 2. Scope
**In-scope:**
- Quét tất cả SKILL.md files, đếm lines, check YAML frontmatter (7 required fields)
- Check references/ directory tồn tại + size
- Phân nhóm 2 chiều: Lifecycle stage + Use-case
- Cross-reference patterns từ marketing-skills.md (3 repos ngoài)

**Out-of-scope:**
- KHÔNG đánh giá chất lượng nội dung (đó là task skills-0kv)
- KHÔNG đọc chi tiết SKILL.md body
- KHÔNG tạo mới hoặc sửa skills

## 3. Input / Output
**Input:**
- Repo: `/home/admin88/1_active_projects/spy-skills/10_AI-Research-SKILLs/`
- All `*/*/SKILL.md` files (84 files across 21 categories)
- File: `/home/admin88/1_active_projects/spy-skills/self-explores/learnings/marketing-skills.md`

**Output:**
- `self-explores/learn/overview/analysis.md` — Bảng inventory + phân nhóm + cross-ref

## 4. Dependencies
- Không có dependency (task đầu tiên trong pipeline)
- Cần: `find`, `wc`, `head`, `python3` (có sẵn)

## 5. Flow xử lý

### Step 1: Dynamic count & validate (~5 phút)
```bash
# Đếm tổng skills
find [0-2][0-9]-*/*/SKILL.md | wc -l
# Liệt kê tất cả skill paths
find [0-2][0-9]-*/*/SKILL.md | sort
```
**Verify:** Count phải match (hiện tại = 84). Nếu khác 85 ghi README → ghi note.

### Step 2: Extract metadata per skill (~15 phút)
Với mỗi SKILL.md, extract:
```bash
# Line count
wc -l {path}
# YAML frontmatter fields (parse between first --- and second ---)
# Check 7 required: name, description, version, author, license, tags, dependencies
head -20 {path}
# Check references/ exists
ls {category}/{skill}/references/ 2>/dev/null
# References size
du -sh {category}/{skill}/references/ 2>/dev/null
```
Dùng python script hoặc Bash loop. Output: JSON hoặc TSV.

### Step 3: Build inventory table (~10 phút)
Tạo bảng Markdown với columns:
| # | Category | Skill Name | Lines | Refs? | Refs Size | YAML Valid | Missing Fields |

### Step 4: Phân nhóm 2 chiều (~10 phút)
**Lifecycle stages:**
- Data Prep: 02-tokenization, 05-data-processing
- Training: 01-model-architecture, 03-fine-tuning, 08-distributed-training
- Post-Training: 06-post-training
- Optimization: 10-optimization
- Deployment: 12-inference-serving, 09-infrastructure
- Evaluation: 11-evaluation
- Monitoring: 13-mlops, 17-observability
- Research: 04-mechanistic-interpretability, 20-ml-paper-writing, 21-research-ideation
- Application: 14-agents, 15-rag, 16-prompt-engineering, 18-multimodal
- Safety: 07-safety-alignment
- Advanced: 19-emerging-techniques

**Use-case groups:**
- Production Ready (deploy today)
- Research Tools (experiment & analyze)
- Safety & Alignment (guardrails)
- Application Building (build products)

### Step 5: Cross-reference marketing-skills.md (~10 phút)
Đọc marketing-skills.md, extract patterns:
- Context file patterns (YAML frontmatter, progressive disclosure)
- Executable frameworks vs reference guides
- Multi-agent orchestration approaches
- Compare quality standards

### Step 6: Write output file (~10 phút)
Tạo `self-explores/learn/overview/analysis.md` với tất cả sections trên.

**Verify:** File tồn tại, có inventory table, phân nhóm, cross-ref ≥3 patterns.

## 6. Edge Cases & Error Handling
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| SKILL.md thiếu frontmatter | No `---` at line 1 | Mark "YAML Invalid" | Flag trong bảng |
| Skill count khác README | find ≠ 85 | Ghi actual count | Note discrepancy |
| References dir empty | Dir exists but 0 files | Mark "Refs: empty" | Ghi note |
| marketing-skills.md not found | File moved/deleted | Skip cross-ref | Thông báo user |

## 7. Acceptance Criteria
- **Happy 1:** Given repo with 84 SKILL.md files When inventory runs Then table has 84 rows, each with 7 columns filled
- **Happy 2:** Given marketing-skills.md exists When cross-ref runs Then ≥3 patterns identified and documented
- **Negative:** Given SKILL.md without frontmatter When YAML check runs Then row shows "Invalid" + lists missing fields

## 8. Technical Notes
- Actual skill count = 84 (README says 85 — verify and note)
- YAML 7 required fields: name, description, version, author, license, tags, dependencies
- Gold standard reference: `06-post-training/grpo-rl-training/SKILL.md`
- marketing-skills.md path: `/home/admin88/1_active_projects/spy-skills/self-explores/learnings/marketing-skills.md`

## 9. Risks
- Low risk task — read-only analysis, no side effects
- Minor: Output file path `self-explores/learn/overview/` needs `mkdir -p`

## Phản biện (2026-03-26)
*(Incorporated into detailed design above)*

## Worklog

### [bat-dau] Bắt đầu + Hoàn thành
**Kết quả:**
- Total skills: 85 (84 subdirectory + 1 category-level 20-ml-paper-writing)
- All 85 have valid YAML frontmatter, 0 missing fields
- 5 skills under 200 lines (stub-like), 16+ over 500 lines
- 7 skills missing references/ directory
- Largest refs: unsloth (1858KB), deepspeed (628KB)
- 3 cross-reference patterns identified from marketing-skills.md

**Files tạo:**
- self-explores/learn/overview/analysis.md (238 lines) — inventory + grouping + cross-ref

**Output mong đợi:**
- [x] Bảng inventory 85 skills
- [x] Phân nhóm theo lifecycle (10 phases) + use-case (4 groups)
- [x] Cross-reference patterns (3 patterns identified)
