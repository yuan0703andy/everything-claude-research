# Claude 專案配置

> 將此模板複製到新專案目錄並重命名為 CLAUDE.md
> 這個文件告訴 Claude 如何為這個專案配置研究團隊
>
> **重要配套文件**:
> - `PROJECT.md` - 專案定義（Vision, Scope, Goals）
> - `STATE.md` - 跨 session 狀態管理
> - `RESEARCH_PLAN.md` - Phase workflow 規劃

## 專案基本資訊

**專案名稱**: [從 PROJECT.md 複製]
**專案目錄**: [此專案的路徑]
**專案文件**: 參考本目錄下的 PROJECT.md

## Domain Knowledge 配置

### 主要領域
**領域**: `domains/[domain-name]/`

[選擇一個]
- [ ] `domains/stats-theory/` - 統計理論
- [ ] `domains/policy-making/` - 政策研究
- [ ] 其他: [指定路徑]

**領域文件**:
- 核心知識: `domains/[domain-name]/DOMAIN.md`
- 術語表: `domains/[domain-name]/terminology.md`
- 文獻: `domains/[domain-name]/literature/`

### 次要領域（如果跨領域）
**次要領域**: [如果適用]

**領域整合說明**:
[如果是跨領域專案，說明如何整合兩個領域的知識]
- 主要框架來自: [領域 A]
- 方法論來自: [領域 B]
- 當兩者衝突時: [如何處理]

## Available Commands

### Phase Workflow (GSD-inspired)

**Phase 1: Context & Ideation**
```
/brainstorm [topic]      # 結構化 brainstorming（討論階段）
                         # 輸出: .planning/phases/phase-N-CONTEXT.md
```

**Phase 2: Planning & Review**
```
/review-hypothesis H-XXX # 深度假說審查（規劃階段）
                         # 輸出: .planning/phases/phase-N-PLAN.md
                         #       reviews/H-XXX_review.md

/eval define H-XXX       # 定義驗證標準（規劃階段）
```

**Phase 3: Execution**
```
/execute-analysis        # 執行分析（執行階段）
                         # 輸出: .planning/phases/phase-N-EXECUTE.md
                         #       results/, figures/
```

**Phase 4: Verification**
```
/verify-results          # 驗證分析結果（驗證階段）
                         # 輸出: .planning/phases/phase-N-VERIFY.md
                         #       reviews/H-XXX_verification_report.md

/eval check H-XXX        # 檢查是否達標
```

### State Management (跨 Session 連續性)

```
/progress                # 快速查看研究進度
                         # 使用時機：Session 開始、需要快速回顧

/update-state            # 更新 STATE.md
                         # 使用時機：Session 結束、關鍵決定後
```

### Coordination

```
/lab-meeting             # 虛擬 lab meeting（週會）
                         # 輸出: meeting_notes/YYYY-MM-DD.md
```

### Typical Workflow

```
┌─────────── Session N ───────────┐
│                                  │
│  1. /progress                    │  ← 了解當前狀態
│      ↓                           │
│  2. [選擇任務]                    │
│      ↓                           │
│  3. /brainstorm              OR  │  ← Phase 1-2
│     /review-hypothesis           │
│      ↓                           │
│  4. /execute-analysis            │  ← Phase 3
│      ↓                           │
│  5. /verify-results              │  ← Phase 4
│      ↓                           │
│  6. /update-state                │  ← 記錄進度
│                                  │
└──────────────────────────────────┘
        ↓
┌─────────── Session N+1 ──────────┐
│  /progress (讀取 STATE.md)        │
│  [繼續工作]                       │
└──────────────────────────────────┘
```

### Additional Commands (from plugin)

```
/checkpoint              # 保存研究檢查點
/verify                  # 3-phase 驗證循環（generic）
/tdd                     # Test-driven 開發（寫分析函數時）
/code-review             # 程式碼審查（分析腳本）
```

## 研究團隊專業化

> 根據專案需求調整團隊成員的專業知識重點

### Theorist (理論博後)
**基礎角色**: `~/.claude/agents/senior-postdocs/theorist.md`

**本專案特別熟悉**:
- [該專案相關的理論 1]
- [該專案相關的理論 2]
- [該專案相關的關鍵文獻]

**重點任務**:
- [例如：發展 XX 理論在 YY 情境下的擴展]
- [例如：整合 AA 和 BB 兩個理論框架]

### Experimentalist (實驗博後)
**基礎角色**: `~/.claude/agents/senior-postdocs/experimentalist.md`

**本專案特別熟悉**:
- [該專案相關的方法 1]
- [該專案相關的方法 2]
- [該專案相關的數據類型]

**重點任務**:
- [例如：設計 XX 情境下的因果推論策略]
- [例如：處理 YY 類型的數據問題]

### Methodologist (方法論專家)
**基礎角色**: `~/.claude/agents/senior-postdocs/methodologist.md`

**本專案特別關注**:
- [該專案的主要方法論挑戰]
- [該專案的效度威脅]
- [該領域的審稿標準]

**審查重點**:
- [例如：高維度數據的多重比較問題]
- [例如：觀察性研究的因果推論標準]

### Lab Manager (實驗室管理員)
**基礎角色**: `~/.claude/agents/lab-manager/coordinator.md`

**本專案追蹤**:
- 專案進度: 按 RESEARCH_PLAN.md 中的 phases
- 假說排名: 維護本專案的假說 Elo 系統
- 狀態管理: 負責 STATE.md 和 /progress, /update-state
- 資源分配: [本專案的特定資源限制]

## Skills 配置

### Core Skills (from everything-claude-code)

**Theorist 使用**:
- `iterative-retrieval` - 4-phase 文獻檢索

**Experimentalist 使用**:
- `verification-loop` - 3-phase 驗證（Build → Functionality → Quality）

**Methodologist 使用**:
- `eval-harness` - 正式評估框架（pass@k 指標）

**Lab Manager 使用**:
- `strategic-compact` - 策略性上下文壓縮建議

### 專案特定 Skills
[如果需要額外技能，列在這裡]

**需要額外載入**:
- [ ] `causal-inference` - 因果推論方法
- [ ] `text-analysis` - 文本分析（如果分析文本）
- [ ] `machine-learning` - 機器學習方法
- [ ] 其他: [指定]

## Rules 配置

### 全域 Rules（必須遵守）
- `~/.claude/rules/research-principles.md` - 研究基本原則
- `~/.claude/rules/analysis-code-style.md` - 分析代碼規範
- `~/.claude/rules/research-workflow.md` - 研究工作流程
- `~/.claude/rules/validation.md` - 驗證要求
- `~/.claude/rules/ethics.md` - 研究倫理

### 專案特定 Rules
[如果這個專案有特殊規範]

```markdown
## [專案名稱] 特定規則

### 數據處理規則
- [規則 1]
- [規則 2]

### 分析規則
- [規則 1]
- [規則 2]

### 報告規則
- [規則 1]
- [規則 2]
```

## 專案約束與偏好

### 時間約束
- **截止日期**: [如果有]
- **時間壓力**: [高/中/低]
- **優先級**: [在多個專案中的優先級]

### 資源約束
- **計算資源**: [限制說明]
- **數據限制**: [限制說明]
- **人力限制**: [限制說明]

### 方法偏好
- **偏好方法**: [如果有特定偏好]
- **避免方法**: [如果有特定限制]
- **方法創新**: [是否鼓勵方法創新]

### 風險容忍度
- **風險偏好**: [保守/中性/激進]
- **容忍失敗**: [高/中/低]
- **創新要求**: [高/中/低]

## 輸出與文檔規範

### 核心狀態文件（GSD-inspired）
```
STATE.md                 # 跨 session 狀態（每次 session 更新）
RESEARCH_PLAN.md         # Phase workflow 規劃
PROJECT.md               # 專案定義和 scope
```

### Phase 記錄（自動生成）
```
.planning/
├── phases/
│   ├── phase-1-CONTEXT.md    # /brainstorm 輸出
│   ├── phase-1-SUMMARY.md    # Phase 1 總結
│   ├── phase-2-PLAN.md       # /review-hypothesis 輸出
│   ├── phase-2-SUMMARY.md
│   ├── phase-3-EXECUTE.md    # /execute-analysis 輸出
│   ├── phase-3-VERIFY.md     # /verify-results 輸出
│   └── phase-3-SUMMARY.md
└── research/                 # 文獻研究記錄
```

### 假說文件
```
hypotheses/
├── HYPOTHESES.md            # 假說排名總表
├── H-001.md                 # 個別假說詳細
├── H-002.md
└── ...
```

### 審查記錄
```
reviews/
├── H-001_review.md          # /review-hypothesis 輸出
├── H-001_verification_report.md  # /verify-results 輸出
└── ...
```

### 會議記錄
```
meeting_notes/
├── 2024-01-27.md            # /lab-meeting 輸出
└── ...
```

### 分析結果
```
results/                     # /execute-analysis 產生
figures/                     # /execute-analysis 產生
data/
├── raw/                     # 原始數據（gitignored）
└── processed/               # 處理後數據
```

### 程式碼
```
analysis/                    # 分析腳本
├── 01_load.R
├── 02_clean.R
├── 03_main_analysis.R
└── 04_robustness.R

functions/                   # 可重用函數
```

## Context Management

### State Files 優先級

當開始新 session 時，優先讀取：
1. **STATE.md** - 最重要（上次到哪裡了？）
2. **RESEARCH_PLAN.md** - 當前 phase 和計畫
3. **PROJECT.md** - 專案 vision 和 scope
4. **hypotheses/HYPOTHESES.md** - 假說排名

### Context 刷新策略（strategic-compact）

**Lab Manager 會建議壓縮時機**:
- ✅ Lab Meeting 結束後
- ✅ Brainstorming 完成後
- ✅ Phase 轉換時
- ✅ 工具調用 > 50 次

**不建議壓縮**:
- ❌ 假說審查進行中
- ❌ 複雜推理過程中
- ❌ 等待關鍵決定時

## 協作與溝通

### 內部溝通
- **Lab Meeting**: [頻率，例如：每週]
- **進度更新**: 每次 session 結束運行 `/update-state`
- **假說審查**: [何時觸發]

### 外部溝通
[如果涉及外部協作者]
- [協作者角色]: [溝通方式和頻率]

## 版本控制

### Git 設置
- **Repository**: [如果適用]
- **重要**: STATE.md, RESEARCH_PLAN.md, PROJECT.md 都應該 commit
- **Commit 規範**:
  - `hypothesis:` - 假說相關
  - `analysis:` - 分析執行
  - `review:` - 審查記錄
  - `state:` - STATE.md 更新

### 數據版本控制
- **策略**: [如何追蹤數據版本]
- **備份**: [備份策略]

## 啟動檢查清單

在開始使用此專案配置前，確認：
- [ ] PROJECT.md 已填寫完整（包含 Vision 和 Scope）
- [ ] STATE.md 已從模板複製並初始化
- [ ] RESEARCH_PLAN.md 已從模板複製並填寫
- [ ] DOMAIN.md 已準備好（至少有框架）
- [ ] 專案目錄結構已建立
  - [ ] hypotheses/, meeting_notes/, reviews/, .planning/
  - [ ] data/, results/, figures/, analysis/
- [ ] 所有 Agents 都已配置
- [ ] 已進行第一次 /brainstorming 或假說提出
- [ ] Lab Manager 已建立 HYPOTHESES.md

## 啟動流程

```bash
# Session 1: 設置
1. 複製模板到專案目錄
2. 填寫 PROJECT.md (Vision, Scope, Goals)
3. 初始化 STATE.md
4. 填寫 RESEARCH_PLAN.md 框架
5. /brainstorm "your research topic"
   → 產生 phase-1-CONTEXT.md 和初步假說

# Session 2: 規劃
1. /progress (查看 Session 1 成果)
2. /review-hypothesis H-001
   → 產生 phase-2-PLAN.md 和 review
3. /update-state

# Session 3+: 執行循環
1. /progress
2. /execute-analysis 或 /review-hypothesis (new)
3. /verify-results
4. /update-state
```

## 知識累積計畫

### 預期更新 Domain Knowledge
完成此專案後，預期更新：
- [ ] `domains/[xxx]/DOMAIN.md` - [更新什麼部分]
- [ ] `domains/[xxx]/literature/` - [新增什麼文獻]
- [ ] `domains/[xxx]/methods.md` - [新增什麼方法]

### Lessons Learned
[專案結束後記錄在 STATE.md 最後]
- 什麼做得好
- 什麼需要改進
- 未來類似專案的建議

## 特殊指示

### 給 Lab Manager 的重點指示
- 嚴格維護 STATE.md（這是跨 session 連續性的關鍵）
- 每次 session 結束前提醒運行 `/update-state`
- 追蹤 phase 轉換
- 在合適時機建議 `/compact`

### 給所有 agents 的指示
- 查看 STATE.md 了解當前狀態
- 遵循 RESEARCH_PLAN.md 的 phase 流程
- 記錄重要決定（會進入 STATE.md）

## 附註

[任何其他需要說明的事項]

---

**配置建立**: [日期]
**最後更新**: [日期]
**配置版本**: 2.0 (GSD-integrated)
