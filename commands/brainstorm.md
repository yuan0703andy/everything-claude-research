---
name: brainstorm
description: 啟動假設生成流程。結構化 brainstorming + Theorist agent invocation。
argument-hint: <research question or topic>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

<brainstorm_command>

# /brainstorm Command

針對特定問題進行結構化 brainstorming，生成高品質研究假說。

## 目的

這是一個結構化的假說生成流程，通過多輪次的發散和收斂，產出高品質的研究假說。結合了：
- **桌面研討式流程**：5 輪次結構化討論
- **Agent 驅動執行**：自動化 agent invocation
- **Goal-backward thinking**：從研究目標倒推假說需求

## 使用時機
- 開始新專案時
- 遇到研究瓶頸需要新想法時
- 文獻回顧後想要形成假說時
- 有初步數據觀察想要理論化時

## 前置準備
確保你已經：
1. ✅ 閱讀了相關文獻
2. ✅ 理解了研究問題的背景
3. ✅ 準備好專案的 DOMAIN.md（領域知識）
4. ✅ 明確了研究約束條件

---

## Workflow

```
User: /brainstorm [topic]
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Context                   │
    │  - Read domain DOMAIN.md                │
    │  - Read project PROJECT.md              │
    │  - Read existing hypotheses (if any)    │
    │  - Check STATE.md for current status    │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Structured Rounds              │
    │  Round 1: Divergent thinking (Theorist) │
    │  Round 2: Feasibility filter (Exp)      │
    │  Round 3: Refinement (Theorist+Exp)     │
    │  Round 4: Methods review (Method)       │
    │  Round 5: Documentation (Coordinator)   │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Output & State Update          │
    │  - Present hypotheses to user           │
    │  - Update HYPOTHESES.md                 │
    │  - Update STATE.md                      │
    │  - Suggest next command                 │
    └─────────────────────────────────────────┘
```

---

## Execution Steps

### Step 1: Context Loading (自動執行)

```python
# Load domain knowledge
domain_path = get_domain_path_from_project()
domain_knowledge = read(domain_path + "/DOMAIN.md")

# Load project context
project_md = read("PROJECT.md")
state_md = read("STATE.md")

# Load existing hypotheses if any
existing_hypotheses = read("hypotheses/HYPOTHESES.md") if exists else None
```

**輸出**: Context package ready for agents

---

### Step 2: Structured Brainstorming Rounds

#### Round 1: 發散階段 (10 分鐘)
**角色**: Theorist 主導

**Spawn Theorist with**:
```markdown
<task>
## Research Topic
[User's input topic/question]

## Context
### Domain Knowledge
[From DOMAIN.md]

### Project Context
[From PROJECT.md]

### Existing Hypotheses (if any)
[From HYPOTHESES.md]

## Request
**Mode**: Exploration (發散思維)

Generate 5-10 initial ideas following these principles:
- 不批判，鼓勵大膽嘗試
- 每個想法包含：
  - 一句話描述核心主張
  - 為什麼這個想法值得探索
  - 初步的理論依據

Use goal-backward thinking: 從研究目標倒推什麼樣的發現會最有價值。
</task>
```

**輸出**: `brainstorm_ideas_initial.md` with 5-10 raw ideas

---

#### Round 2: 可行性初篩 (10 分鐘)
**角色**: Experimentalist 主導

**任務**:
- 快速評估每個想法的可驗證性
- 標記明顯不可行的（但記錄原因）
- 識別需要大量資源的
- 提出可行性疑問

**輸出**: 更新 `brainstorm_ideas_initial.md`，加入可行性註記
- 🟢 可行
- 🟡 需要額外資源
- 🔴 當前不可行

---

#### Round 3: 深化精煉 (15 分鐘)
**角色**: Theorist + Experimentalist 協作

**任務**:
- 選擇 3-5 個最有潛力的想法
- 將其結構化為正式的假說格式
- 明確定義：
  - 核心主張 (Core Claim)
  - 理論機制 (Mechanism)
  - 可觀察預測 (Predictions)
  - 驗證方法 (Testability)
  - 替代解釋 (Alternative Explanations)

**輸出**: 3-5 個正式的假說提案文件 in `hypotheses/proposals/`

---

#### Round 4: 方法論審查 (10 分鐘)
**角色**: Methodologist

**任務**:
- 對每個假說進行快速方法論審查
- 識別潛在的偏誤和陷阱
- 確保符合領域標準
- 提供改進建議
- 自評 Novelty, Importance, Testability (各 1-5 分)

**輸出**: 為每個假說添加方法論註記和自評分數

---

#### Round 5: 記錄與歸檔 (5 分鐘)
**角色**: Coordinator

**任務**:
- 所有想法存檔（包括被否定的，註明原因）
- 為通過的假說分配 ID (H-XXX)
- 設定初始 Elo 分數 (1200)
- 記錄 brainstorming session 的元數據
- 更新 HYPOTHESES.md index
- 更新 STATE.md

**輸出**:
- 新假說加入 `hypotheses/proposals/` 目錄
- 更新 `hypotheses/HYPOTHESES.md`
- 更新 `STATE.md`
- 創建 `meeting_notes/brainstorm_session_[date].md` 記錄

---

### Step 3: Output Presentation

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 BRAINSTORM ► [Topic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Generated [N] hypothesis proposals:

## H-[NNN]: [Title]
**Core Claim**: [One sentence]
**Novelty**: [Brief statement]
**Self-Assessment**:
  - Novelty: [X/5]
  - Importance: [X/5]
  - Testability: [X/5]
**Initial Elo**: 1200

## H-[NNN]: [Title]
[Same structure]

─────────────────────────────────────────────────────

## Next Steps

Select hypotheses to pursue:
- "Review H-001 and H-003" → Starts review cycle
- "Expand on H-002" → Deeper exploration of one hypothesis
- "More ideas" → Generate additional hypotheses

Or: `/review-hypothesis H-001` to start formal review

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 輸出結構

### 1. brainstorm_session_[date].md
```markdown
# Brainstorming Session - [Topic]

**日期**: [date]
**主題**: [topic]
**參與**: Theorist, Experimentalist, Methodologist, Coordinator

## 背景
[簡要描述為什麼進行這次 brainstorming]

## Round 1: 初始想法 (Divergent Thinking)
1. [想法 1] - Theorist
2. [想法 2] - Theorist
...

## Round 2: 可行性篩選 (Feasibility Filter)
| 想法 | 可行性 | Experimentalist 評論 |
|------|--------|---------------------|
| 1 | 🟢 | 數據可得，方法成熟 |
| 2 | 🟡 | 需要額外計算資源 |
| 3 | 🔴 | 數據不可得 |

## Round 3: 精煉假說 (Refinement)
[列出選中的 3-5 個假說，結構化格式]

### H-001: [Title]
**Core Claim**: [...]
**Mechanism**: [...]
**Predictions**: [...]
**Testability**: [...]

## Round 4: 方法論審查 (Methods Review)
### H-001
- **Methodologist 評估**:
  - Novelty: 4/5
  - Importance: 5/5
  - Testability: 3/5
- **潛在問題**: [...]
- **建議**: [...]

## Round 5: 文檔化 (Documentation)
- **接受假說**: H-001, H-002, H-003
- **擱置想法**: [4, 5] - 原因: [...]
- **Initial Elo**: All set to 1200

## 決議
- ✅ H-001, H-002, H-003 進入假說 pipeline
- 🔄 Next action: `/review-hypothesis H-001` for formal review
- 📊 STATE.md updated with new hypotheses
```

---

### 2. hypotheses/proposals/H-XXX-[slug].md
```markdown
---
id: H-XXX
title: "[標題]"
status: draft
created: [date]
elo: 1200
origin: "Brainstorming session on [topic]"
self_assessment:
  novelty: X/5
  importance: X/5
  testability: X/5
---

# Hypothesis H-XXX: [標題]

## Core Claim
[一句話核心主張]

## Theoretical Basis
[理論依據，為什麼這個假說在理論上合理]

## Mechanism
[因果機制說明，X 如何導致 Y]

## Predictions
如果假說為真，我們應該觀察到：
1. [具體可觀察的預測 1]
2. [具體可觀察的預測 2]
3. [具體可觀察的預測 3]

## Testability
**驗證方法**:
- Data needed: [...]
- Analysis approach: [...]
- Expected timeline: [...]

**Feasibility**: [Experimentalist 初步評估]

## Alternative Explanations
除了本假說，還有哪些可能解釋觀察結果：
1. [替代解釋 1] - 如何排除
2. [替代解釋 2] - 如何排除

## Boundary Conditions
假說在什麼條件下成立：
- [條件 1]
- [條件 2]

## Key References
1. [文獻 1] - 提供理論基礎
2. [文獻 2] - 相關實證證據

## Initial Assessment (from Round 4)
- **Novelty**: [X/5] - [理由]
- **Importance**: [X/5] - [理由]
- **Testability**: [X/5] - [理由]
- **Initial Elo**: 1200

## Notes
[任何額外註記]
```

---

### 3. Update HYPOTHESES.md

Add new hypotheses to the index:

```markdown
### In Development
| ID | Title | Created | Last Update | Stage | Owner | Elo |
|----|-------|---------|-------------|-------|-------|-----|
| H-XXX | [Title] | [Date] | [Date] | Draft | Theorist | 1200 |
| H-YYY | [Title] | [Date] | [Date] | Draft | Theorist | 1200 |
```

---

### 4. Update STATE.md

```markdown
## Hypothesis Pipeline

### Recently Added (Brainstorm [Date])
| ID | Title | Stage | Initial Elo |
|----|-------|-------|-------------|
| H-XXX | [Title] | Draft | 1200 |
| H-YYY | [Title] | Draft | 1200 |

### Next Actions
- [ ] Review H-XXX via `/review-hypothesis H-XXX`
- [ ] Review H-YYY via `/review-hypothesis H-YYY`
```

---

## User Interactions

| User Says | Action |
|-----------|--------|
| "Review H-001" | Route to `/review-hypothesis H-001` |
| "More ideas" | Re-invoke Theorist with "more variety" instruction |
| "Combine H-001 and H-002" | Ask Theorist to synthesize |
| "I like H-003 best" | Mark H-003 as priority, boost Elo by +50 |
| "None of these" | Ask for feedback, adjust approach, try different angle |
| "Expand on H-002" | Theorist deep dive on H-002 only |

---

## 使用範例

```bash
# 在專案目錄下
cd ~/research/projects/my-project

# 啟動 brainstorming
/brainstorm "統計推論在高維度下的基本限制"

# 系統會自動：
# 1. ✅ 載入 domains/stats-theory/DOMAIN.md
# 2. ✅ 召集研究團隊進行 5 輪討論
# 3. ✅ 產出 meeting_notes/brainstorm_session_[date].md
# 4. ✅ 創建新的假說文件在 hypotheses/proposals/
# 5. ✅ 更新 HYPOTHESES.md index
# 6. ✅ 更新 STATE.md
```

---

## Output Files Structure

After brainstorm:
```
project/
├── hypotheses/
│   ├── HYPOTHESES.md              # ✅ Updated with new hypotheses
│   └── proposals/
│       ├── H-001-[slug].md        # ✅ New proposal
│       ├── H-002-[slug].md        # ✅ New proposal
│       └── H-003-[slug].md        # ✅ New proposal
│
├── meeting_notes/
│   └── brainstorm_session_[date].md  # ✅ Session record
│
└── STATE.md                       # ✅ Updated with new hypotheses
```

---

## 最佳實踐

### 時間管理
- 一次 brainstorming 不要超過一個小時
- 如果卡住，先做文獻閱讀再回來
- "需要更多背景知識" 是有效結論

### 質量標準
- 重質不重量，3 個好假說勝過 10 個勉強的假說
- 每個假說都要有明確的可觀察預測
- 不要害怕瘋狂想法，但要能說明為什麼值得探索

### 記錄習慣
- 被否定的想法也要記錄原因，避免重複
- 定期回顧被擱置的想法，技術進步可能讓它們變得可行
- 記錄思考過程，不只是結論

### Goal-Backward Thinking
- 從研究目標倒推：什麼樣的發現會最有價值？
- 考慮：如果我們證明了這個假說，然後呢？
- 優先考慮能開啟新研究方向的假說

---

## 注意事項

⚠️ **這不是替代深度文獻閱讀的工具**
- Brainstorming 前應該已經有紮實的文獻基礎
- 如果對領域不熟悉，先用 Literature-RA 做系統性文獻回顧

⚠️ **產出是初步的**
- Brainstorming 產出的假說需要後續精煉
- 通過 `/review-hypothesis` 進行正式審查
- 預期多輪迭代

⚠️ **沒有產出也是有效結果**
- 如果一次 brainstorming 沒有產出可行假說，這是正常的
- 可能意味著需要更多背景閱讀或換個角度

⚠️ **保持開放心態**
- 不要過早否定想法
- 鼓勵跨領域連結
- 記錄所有想法，包括"不可行"的

---

## Integration with Other Commands

**After brainstorm**:
- ➡️ `/review-hypothesis H-001` - Start formal review
- ➡️ `/progress` - Check hypothesis pipeline status
- ➡️ `/lab-meeting` - Present hypotheses in weekly meeting

**Before brainstorm**:
- Use Literature-RA for systematic lit review
- Ensure DOMAIN.md is up to date
- Check STATE.md for research phase

</brainstorm_command>
