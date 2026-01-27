---
name: lab-meeting
description: 週會：review 進度、Elo tournament、更新排名、規劃下週工作。
argument-hint: [optional: specific agenda or --emergency/--project-kickoff/--project-closure]
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

<lab_meeting_command>

# /lab-meeting Command

執行虛擬 Lab Meeting，審查進度、討論假說、更新排名。

## 目的

這是研究團隊的定期同步機制，結合了：
- **標準議程**：8個固定議題的結構化會議
- **Elo Tournament**：基於成對比較的假說排名系統
- **GSD Goal-backward Review**：檢查是否真正達成研究目標
- **決策記錄**：明確的行動項目和責任人

確保所有人對專案狀態、假說優先級、資源分配有共識。

## 使用時機
- ✅ 建議每週固定時間執行（例如每週一早上）
- ✅ 重大專案里程碑時
- ✅ 需要團隊集體決策時
- ✅ 有新假說需要審查時
- ✅ Elo 排名需要更新時

---

## Workflow

```
User: /lab-meeting
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Generate Status Report         │
    │  - Pull from STATE.md                   │
    │  - Aggregate hypothesis status          │
    │  - Calculate metrics                    │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Hypothesis Tournament          │
    │  - Compare top hypotheses pairwise      │
    │  - Update Elo rankings                  │
    │  - Track match history (W-L-D)          │
    │  - Identify priorities                  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Blockers & Decisions           │
    │  - Surface blockers                     │
    │  - Present decisions to PI              │
    │  - Goal-backward check                  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Plan Next Week                 │
    │  - Prioritize tasks                     │
    │  - Assign to agents                     │
    │  - Set targets                          │
    │  - Update HYPOTHESES.md + STATE.md      │
    └─────────────────────────────────────────┘
```

---

## 標準議程

### 1. 開場與行動項目回顧 (5 分鐘)
**主持**: Coordinator

**內容**:
- 回顧上週會議的決議
- 檢查行動項目完成情況
- 未完成項目的障礙識別

**格式**:
```markdown
### 已完成
- [x] [項目] - 負責人 - ✅ Done

### 未完成
- [ ] [項目] - 負責人
  - 原因: [...]
  - 新截止日期: [...]
  - 需要支援: [...]
```

---

### 2. 專案狀態報告 (5-10 分鐘)
**主持**: Coordinator
**參與**: 所有成員

**內容**:
- 每個活躍專案的狀態更新
- 進度百分比
- 本週完成的里程碑
- 遇到的問題和風險
- 資源使用情況

**格式**:
```
專案 A:
- 狀態: 🟢 On track / 🟡 At risk / 🔴 Blocked
- 進度: XX%
- 本週: [完成了什麼]
- 下週: [計畫做什麼]
- 風險: [有什麼障礙]
- Goal check: [是否朝目標前進？]
```

---

### 3. 假說審查 (20-30 分鐘)
**主持**: 依假說類型選擇合適的 Senior Postdoc

對新假說或有重大更新的假說進行審查：

#### 3.1 假說報告
**Theorist** 報告：
- 假說的理論動機
- 核心主張和機制
- 與現有文獻的關係
- 為什麼這個假說重要

#### 3.2 可行性評估
**Experimentalist** 評估：
- 驗證方案
- 資源需求（數據、時間、計算）
- 技術風險
- 預期時間線

#### 3.3 方法論意見
**Methodologist** 提供：
- 方法論適當性
- 潛在偏誤和陷阱
- 是否符合領域標準
- 預期審稿人關注點

#### 3.4 討論與決議
**全體** 討論：
- 優勢和劣勢
- 與其他假說的比較（為 Elo tournament 準備）
- 優先級判斷
- 資源分配

**決議**:
- ✅ 推進：分配資源開始驗證
- 🔄 修改：需要調整後再審查
- ⏸️ 擱置：暫時不推進，但保留
- ❌ 放棄：不再考慮

---

### 4. 假說排名更新 - **Elo Tournament System** (10-15 分鐘)
**主持**: Coordinator
**方法**: 成對比較 + Elo 計算

#### 4.1 Tournament 流程

如果有多個假說需要排名：

**Step 1**: 選擇比較對
- 優先比較 Elo 接近的假說
- 或新假說 vs 已排名的假說
- 每次會議進行 2-4 場比賽

**Step 2**: 多維度投票
團隊根據以下維度評估：

| 維度 | 權重 | 評估問題 |
|------|------|---------|
| **Novelty** | 25% | 這個假說有多新穎？是否開闢新方向？ |
| **Importance** | 25% | 如果為真，對領域的影響有多大？ |
| **Testability** | 20% | 驗證的可行性如何？資源需求合理嗎？ |
| **Progress** | 15% | 目前進展如何？是否卡住？ |
| **Confidence** | 15% | 我們對這個假說的信心有多高？ |

**Step 3**: 計算結果
```python
# 加權計分
h1_score = sum(score[dim] * weight[dim] for dim in dimensions)
h2_score = sum(score[dim] * weight[dim] for dim in dimensions)

winner = h1 if h1_score > h2_score else h2
margin = abs(h1_score - h2_score)

# Elo 更新
K = 32  # K-factor
expected_h1 = 1 / (1 + 10^((elo_h2 - elo_h1) / 400))

if h1 wins:
    elo_h1_new = elo_h1 + K * (1 - expected_h1)
    elo_h2_new = elo_h2 + K * (0 - (1 - expected_h1))
else:
    elo_h1_new = elo_h1 + K * (0 - expected_h1)
    elo_h2_new = elo_h2 + K * (1 - (1 - expected_h1))
```

#### 4.2 Tournament 輸出格式

```markdown
## 🏆 Hypothesis Tournament

### This Week's Matches

**Match 1: H-003 vs H-001**
| Criterion | H-003 | H-001 | Winner |
|-----------|-------|-------|--------|
| Novelty (25%) | 4/5 | 3/5 | H-003 |
| Importance (25%) | 5/5 | 4/5 | H-003 |
| Testability (20%) | 3/5 | 4/5 | H-001 |
| Progress (15%) | 4/5 | 3/5 | H-003 |
| Confidence (15%) | 4/5 | 3/5 | H-003 |
| **Weighted Total** | **4.10** | **3.55** | **H-003** |

**Elo Update**:
- H-003: 1780 → 1810 (+30)
- H-001: 1850 → 1820 (-30)

**Rationale**: H-003 有更高的理論重要性和新穎性，雖然 H-001 在可驗證性上略勝一籌，但整體來看 H-003 優先級更高。

---

**Match 2: H-007 vs H-002**
[Same structure]

### Updated Rankings (Top 10)

| Rank | Δ | ID | Elo | 標題 | 狀態 | W-L-D |
|------|---|-----|-----|------|------|-------|
| 1 | ↑1 | H-003 | 1810 | [Title] | Ready | 6-1-0 |
| 2 | ↓1 | H-001 | 1820 | [Title] | Review | 5-2-1 |
| 3 | → | H-007 | 1720 | [Title] | Draft | 4-2-0 |
| 4 | ↑2 | H-002 | 1680 | [Title] | Testing | 3-1-0 |
| 5 | ↓1 | H-009 | 1650 | [Title] | Draft | 2-2-1 |
| ... | | | | | | |

### Tournament Summary
- **Matches this week**: 2
- **Biggest mover**: H-002 (+50 Elo, ↑2 ranks)
- **Most wins**: H-003 (6-1-0)
- **New entrant**: H-012 (Initial Elo: 1200)
```

---

### 5. 問題討論 (10-15 分鐘)
**主持**: PI (或最相關的 Senior Postdoc)

討論需要集體智慧的問題：
- 卡住的技術問題
- 方向性疑問
- 資源衝突
- 跨專案協調

**Goal-Backward Check**:
- 我們的行動是否真正朝研究目標前進？
- 還是只是完成任務但沒有達成目標？

---

### 6. 資源與時間線 (5 分鐘)
**主持**: Coordinator

- 資源分配檢視
- 即將到來的截止日期
- 下週重點任務
- 需要外部協助的事項

---

### 7. 下週計畫 (5 分鐘)
**主持**: Coordinator

每位成員簡要說明下週目標：
- **Theorist**: [下週任務]
- **Experimentalist**: [下週任務]
- **Methodologist**: [下週任務]
- **Coordinator**: [下週任務]

---

### 8. 結束與記錄 (5 分鐘)
**主持**: Coordinator

- 總結決議事項
- 確認行動項目和責任人
- 設定下次會議時間
- 建議是否需要 `/compact` (strategic compaction)

---

## Meeting Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 LAB MEETING ► [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📊 Progress Report

### This Week
- Hypotheses generated: [N]
- Reviews completed: [N]
- Moved to testing: [N]
- Results obtained: [N]

### Pipeline Status
```
Draft (3) → Review (2) → Ready (5) → Testing (1) → Complete (4)
```

### Key Accomplishments
1. [Accomplishment]
2. [Accomplishment]

─────────────────────────────────────────────────────

## 🏆 Hypothesis Tournament

### Current Top 5 (by Elo)
| Rank | ID | Title | Elo | Change |
|------|----|-------|-----|--------|
| 1 | H-003 | [Title] | 1810 | ↑1 (+30) |
| 2 | H-001 | [Title] | 1820 | ↓1 (-30) |
| 3 | H-007 | [Title] | 1720 | → |
| 4 | H-002 | [Title] | 1680 | ↑2 (+50) |
| 5 | H-009 | [Title] | 1650 | ↓1 |

### This Week's Matches

**Match 1: H-003 vs H-001**
Comparison: [5 criteria, weighted]
Winner: H-003
Elo Change: H-003 +30, H-001 -30
Reason: [Why H-003 won]

**Match 2: H-007 vs H-002**
[Same structure]

### Tournament Statistics
- Total hypotheses ranked: 15
- Matches played this week: 2
- Cumulative matches: 47
- Biggest upset: [If any]

─────────────────────────────────────────────────────

## 🚧 Blockers & Decisions

### Current Blockers
| Blocker | Impact | Needed From | Deadline |
|---------|--------|-------------|----------|
| [Description] | [What's blocked] | [Who can resolve] | [Date] |

### Decisions for PI

**Decision 1: [Question]**
- Option A: [Description] - Pros: [...] - Cons: [...]
- Option B: [Description] - Pros: [...] - Cons: [...]
- **Recommendation**: [Which option and why]

**Decision 2: [Question]**
[Same structure]

─────────────────────────────────────────────────────

## 🎯 Goal-Backward Check

### Research Goals (from PROJECT.md)
1. [Goal 1]
   - Progress: ✅ On track / ⚠️ Needs attention / ❌ Off track
   - Evidence: [What shows progress?]

2. [Goal 2]
   - Progress: [Status]
   - Evidence: [...]

### Misalignments Identified
- [Activity that's not advancing goals]
- [Suggested correction]

─────────────────────────────────────────────────────

## 📅 Plan for Next Week

### Priority Tasks
| # | Task | Owner | Target | Depends On | Goal Link |
|---|------|-------|--------|------------|-----------|
| 1 | [Task] | [Agent] | [Date] | - | [Which goal] |
| 2 | [Task] | [Agent] | [Date] | Task 1 | [Which goal] |

### Goals
- [ ] [Goal 1]
- [ ] [Goal 2]
- [ ] [Goal 3]

─────────────────────────────────────────────────────

## 💡 Open Discussion

[Space for PI to raise topics, ask questions, provide direction]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 決議摘要
1. [Decision 1]
2. [Decision 2]
3. [Decision 3]

## 行動項目
- [ ] [Task] - [Owner] - [Deadline]
- [ ] [Task] - [Owner] - [Deadline]

**Next Meeting**: [Date and Time]
**Compaction Suggestion**: Yes / No - [Rationale]
```

---

## State Updates

### 1. Update HYPOTHESES.md

```markdown
## Elo Rankings

### Active Hypotheses (Top 10)
[Updated rankings from tournament]

### Elo History (Last 10 Changes)
| Date | Match | Winner | Loser | Δ |
|------|-------|--------|-------|---|
| [Date] | H-003 vs H-001 | H-003 | H-001 | +30/-30 |
| [Date] | H-007 vs H-002 | H-002 | H-007 | +25/-25 |
```

### 2. Update STATE.md

```markdown
## Last Lab Meeting
Date: [Today]
Key Decisions:
1. [Decision]
2. [Decision]

## Elo Rankings (Top 3)
1. H-003: 1810
2. H-001: 1820
3. H-007: 1720

## Next Week's Plan
[From meeting output]

## Goal Progress
[From goal-backward check]
```

---

## Interactive Elements

During lab meeting, PI can:

| Command | Action |
|---------|--------|
| "Compare H-001 and H-005" | Run head-to-head comparison |
| "Why is H-003 ranked first?" | Explain Elo position and match history |
| "Prioritize H-007" | Manually boost priority (+50 Elo bonus) |
| "Drop H-002" | Archive hypothesis, remove from rankings |
| "What's blocking H-001?" | Show blockers for specific hypothesis |
| "Plan for [topic]" | Adjust next week's plan |
| "Replay H-003 vs H-001" | Re-evaluate past match |

---

## 特殊情況

### 緊急會議
如果有緊急問題需要即時討論：
```bash
/lab-meeting --emergency "問題描述"
```
- 使用簡化議程
- 只處理緊急問題
- 快速決策，記錄在案

### 專案啟動會議
新專案開始時：
```bash
/lab-meeting --project-kickoff "[專案名]"
```
- 專注於專案目標、資源分配、時間線設定
- 初始假說排名（如果有）
- 設定里程碑

### 專案結案會議
專案結束時：
```bash
/lab-meeting --project-closure "[專案名]"
```
- 回顧成果
- 總結經驗
- 更新知識庫（DOMAIN.md）
- 最終 Elo 排名
- 發表成功的假說

---

## Meeting Cadence

- **Weekly**: Full lab meeting (所有 8 個議程)
- **Daily (optional)**: Quick standup via `/progress` (5 分鐘快速同步)
- **Ad-hoc**: `/lab-meeting --emergency` for critical decisions

---

## 最佳實踐

### 會議效率
- ⏱️ 控制時間，嚴格按議程進行
- 🔍 技術細節討論可以會後單獨進行
- ⏭️ 如果討論超時，標記為待辦事項
- 📊 使用視覺化輔助（圖表、表格）

### Elo Tournament 最佳實踐
- 🎯 每次會議進行 2-4 場比賽（不要太多）
- ⚖️ 優先比較 Elo 接近的假說（更有意義的比較）
- 📝 記錄比較理由（未來可以回顧）
- 🔄 定期重新評估高排名假說（避免 Elo 通脹）
- 🆕 新假說初始 Elo 1200，通過比賽找到位置

### 決策品質
- ✅ 所有決議都要有明確的行動項目和責任人
- ❓ 不確定時承認不確定，避免倉促決策
- 📄 記錄不同意見，不強求共識
- 🎯 使用 Goal-backward thinking：這個決定真的幫助達成目標嗎？

### 團隊協作
- 🔄 輪流讓不同人主持不同議題
- 💬 鼓勵建設性批評
- 🎉 慶祝進展和里程碑
- 🤝 跨 agent 協作機會

### 持續改進
- 📅 每月回顧會議流程的有效性
- 🔧 根據專案需求調整議程
- 🎨 保持靈活性
- 📊 追蹤會議指標（時長、決策數、行動項目完成率）

---

## Elo Tournament Deep Dive

### Why Elo for Research?

傳統研究優先級排序問題：
- ❌ 主觀評分容易受近因效應影響
- ❌ 絕對評分難以校準
- ❌ 新假說與舊假說難以比較

Elo 系統優勢：
- ✅ **Relative comparison**: 成對比較更容易做出準確判斷
- ✅ **Dynamic**: 排名隨著進展動態更新
- ✅ **Robust**: 少數比賽就能找到合理排名
- ✅ **Transparent**: 每次排名變化都有理由

### Elo Calculation Details

```python
def update_elo(elo_a, elo_b, score_a):
    """
    elo_a, elo_b: Current Elo ratings
    score_a: 1 if A wins, 0 if A loses, 0.5 if draw
    Returns: (new_elo_a, new_elo_b)
    """
    K = 32  # K-factor (sensitivity to new results)

    # Expected scores
    expected_a = 1 / (1 + 10**((elo_b - elo_a) / 400))
    expected_b = 1 - expected_a

    # Update
    new_elo_a = elo_a + K * (score_a - expected_a)
    new_elo_b = elo_b + K * ((1 - score_a) - expected_b)

    return new_elo_a, new_elo_b
```

### When to Use Draws (0.5-0.5)

使用平局的情況：
- 兩個假說同樣重要但測試不同方面
- 團隊無法達成共識
- 兩個假說是互補而非競爭

### Elo Interpretation

| Elo Range | Interpretation |
|-----------|----------------|
| 1800+ | 頂級假說，應該優先投入資源 |
| 1600-1800 | 優秀假說，值得認真考慮 |
| 1400-1600 | 中等假說，可以排隊等待 |
| 1200-1400 | 新假說或尚未證明價值的假說 |
| < 1200 | 較弱假說，可能需要重新思考 |

---

## 注意事項

⚠️ **Lab Meeting 不是研討會**
- 不要深入技術討論
- 重點是同步資訊、做出決策、分配任務

⚠️ **Elo 是工具，不是目的**
- Elo 幫助決策，但不能替代判斷
- 有時候低 Elo 假說因為戰略原因也值得推進
- Elo 反映當前共識，但共識可能錯誤

⚠️ **避免 Elo Gaming**
- 不要為了提高 Elo 而選擇容易的對手
- 定期重新評估高排名假說
- 記錄比較理由以供審查

⚠️ **系統性問題識別**
- 如果連續幾週沒有實質進展，需要反思
- 如果同一個假說一直卡在某個階段，需要介入
- 如果資源分配與 Elo 排名長期不一致，需要檢討

⚠️ **會議記錄時效性**
- 會議記錄要及時整理，最好會後 24 小時內完成
- 行動項目要立即分發給負責人
- HYPOTHESES.md 和 STATE.md 必須在會議結束時更新

---

## Integration with Other Commands

**Before lab-meeting**:
- 📊 `/progress` - Get quick status overview
- 📝 Review STATE.md for context

**During lab-meeting**:
- 🔍 `/review-hypothesis H-XXX` - If detailed review needed
- ✅ `/verify-results H-XXX` - If goal-backward check needed

**After lab-meeting**:
- 📈 Check updated HYPOTHESES.md Elo rankings
- ✅ Assign action items to team
- 🗜️ Consider `/compact` if suggested by Coordinator

---

## Example Meeting Flow

```bash
# Start meeting
/lab-meeting

# System loads STATE.md, HYPOTHESES.md
# Generates status report

# PI can interrupt for specific actions:
"Compare H-003 and H-001"  # Triggers tournament match
"Why is H-007 stuck?"      # Investigate blocker
"Prioritize H-005"         # Manual Elo boost

# Meeting ends with:
# - Updated HYPOTHESES.md (new Elo rankings)
# - Updated STATE.md (decisions, next week plan)
# - meeting_notes/lab_meeting_[date].md
# - Action items distributed
```

</lab_meeting_command>
