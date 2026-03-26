---
date: 2026-03-26
type: task-worklog
task: skills-tno
title: "Review & Notion — Tổng hợp kết quả đẩy lên Notion"
status: open
detailed_at: 2026-03-26
detail_score: ready-for-dev
tags: [review, notion, trello, tracking, integration]
---

# Review & Notion — Detailed Design

## 1. Objective
Tổng hợp toàn bộ kết quả nghiên cứu (summary + 20 analyses), đẩy lên Notion (Database) và Trello (Tracking cards), tạo daily log tổng kết.

## 2. Scope
**In-scope:**
- Pre-check Notion + Trello MCP availability
- Tạo Notion Database under Experiments > Skills > 10_AI-Research-SKILLs
- Tạo 20 Trello tracking cards
- Tạo daily log `self-explores/daily/{date}.md`
- Dolt commit trạng thái

**Out-of-scope:**
- KHÔNG tạo lại analysis files
- KHÔNG modify scores
- KHÔNG tạo Notion pages riêng lẻ (dùng Database thay vì Pages)

## 3. Input / Output
**Input:**
- `self-explores/learn/summary.md` — Master summary từ coordinator
- 20 files `self-explores/learn/{skill}/analysis.md`
- Notion workspace (MCP)
- Trello active board (MCP)

**Output:**
- Notion: 1 Database với 20 entries
- Trello: 20 cards trên list "📚 Tracking"
- Local: `self-explores/daily/2026-03-26.md`

## 4. Dependencies
- skills-0mz (Coordinator) phải xong — cần summary.md + verified analysis files

## 5. Flow xử lý

### Step 1: Pre-check external services (~5 phút)
```
# Check Notion
mcp__notion__notion-search query="Experiments"
# Check Trello
mcp__trello__get_active_board_info
mcp__trello__get_lists
```
**Decision matrix:**
| Notion OK | Trello OK | Action |
|-----------|-----------|--------|
| Yes | Yes | Full push (Notion DB + Trello cards) |
| Yes | No | Notion only + local log |
| No | Yes | Trello only + local log |
| No | No | Local-only + thông báo user |

**Verify:** At least 1 external service available, OR fallback to local-only documented.

### Step 2: Prepare data from summary (~5 phút)
Parse `self-explores/learn/summary.md`:
- Extract master table (20 rows)
- For each skill: name, category, C, U, M, S, weighted, tier, integration priority
- For each skill: read analysis.md to get ưu/nhược tóm tắt (first 3 bullets each)

### Step 3: Create Notion Database (~15 phút)
```
# Find or create parent page
mcp__notion__notion-search query="Experiments"
# Navigate to: Experiments > Skills > 10_AI-Research-SKILLs

# Create Database with properties:
# - Skill Name (title)
# - Category (select)
# - Completeness (number, 1-5)
# - Usability (number, 1-5)
# - Modularity (number, 1-5)
# - Safety (number, 1-5)
# - Weighted Score (number)
# - Tier (select: Must/Should/Nice)
# - Integration Priority (select: High/Medium/Low)
# - Ưu điểm (rich_text)
# - Nhược điểm (rich_text)

# Populate 20 entries from data
```

**Verify:** Database exists, 20 entries, all fields populated.

### Step 4: Create Trello tracking cards (~10 phút)
```
# Find or create list "📚 Tracking"
mcp__trello__get_lists
# If not found: mcp__trello__add_list_to_board name="📚 Tracking"

# For each of 20 skills:
mcp__trello__add_card_to_list
  name: "[{score}/5] {skill-name}"
  description: |
    ## Skill Analysis
    - **Category:** {category}
    - **Score:** {weighted}/5 (C:{c} U:{u} M:{m} S:{s})
    - **Tier:** {tier}
    - **Integration:** {priority}

    ## Summary
    **Ưu:** {top 3 bullets}
    **Nhược:** {top 3 bullets}

    **Local file:** `self-explores/learn/{skill}/analysis.md`
```

**Idempotency:** Before creating, search cards containing skill name. Skip if exists.

**Verify:** 20 cards created (or skipped if duplicate), all on "📚 Tracking" list.

### Step 5: Create daily log (~10 phút)
Write `self-explores/daily/2026-03-26.md`:
```markdown
---
date: 2026-03-26
type: daily
tasks_completed: [skills-482, skills-0kv, skills-kik, skills-ssy, skills-hn9, skills-i8b, skills-0mz, skills-tno]
---

# Daily Log — 2026-03-26

## Tasks hoàn thành
- skills-482: Inventory 84 skills, phân nhóm lifecycle + use-case
- skills-0kv: Ranked 84 skills, top 20 identified
- skills-kik/ssy/hn9/i8b: Deep dive 20 skills, 20 analysis.md files
- skills-0mz: Summary + learning path
- skills-tno: Notion DB + Trello cards + this log

## Key Findings
{Top 5 insights từ summary.md}

## Metrics
- Skills analyzed: 84 (inventory) + 20 (deep dive)
- External tracking: Notion DB + Trello cards
- Progress: 8/8 tasks (100%)
```

### Step 6: Dolt commit (~2 phút)
```bash
bd dolt commit -m "Review: Full analysis pipeline complete - 84 skills inventoried, 20 deep dived, Notion+Trello synced"
```

**Verify:** Commit successful.

## 6. Edge Cases & Error Handling
| Case | Trigger | Expected | Recovery |
|------|---------|----------|----------|
| Notion MCP token expired | Auth failure | Log error, skip Notion | Local-only + thông báo |
| Trello board not set | No active board | Log error, skip Trello | Local-only + thông báo |
| Notion "Experiments" page not found | New workspace | Create page hierarchy | Or ask user for parent page ID |
| Trello rate limit | >100 API calls | Slow down, retry | Wait 30s between batches of 5 |
| Duplicate cards | Re-run task | Detect by card name search | Skip existing cards |

## 7. Acceptance Criteria
- **Happy 1:** Given Notion + Trello available When review runs Then Notion DB has 20 entries, Trello has 20 cards
- **Happy 2:** Given all services available When daily log created Then file exists with all sections + correct task IDs
- **Negative:** Given Notion unavailable When review runs Then local-only output + user notified + no crash

## 8. Technical Notes
- Notion Database > individual Pages because: queryable, filterable, sortable
- Trello card naming: `[{score}/5] {name}` for visual quick-scan
- Dolt commit preserves beads state snapshot for rollback
- Rate limiting: Max 5 Notion API calls/second, 10 Trello calls/second

## 9. Risks
- Medium: External API failures. Mitigation: Local fallback always works.
- Low: Data mismatch between summary.md and analysis files. Mitigation: Coordinator task verified.
- Low: Notion page hierarchy doesn't exist. Mitigation: Create on-the-fly or ask user.

## Phản biện (2026-03-26)
*(Incorporated into detailed design above)*

## Worklog
*(Chưa bắt đầu)*
