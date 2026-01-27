# Claude 專案配置

> 將此模板複製到新專案目錄並重命名為 CLAUDE.md
> 這個文件告訴 Claude 如何為這個專案配置研究團隊

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
- 專案進度: 按 PROJECT.md 中的階段
- 假說排名: 維護本專案的假說 Elo 系統
- 資源分配: [本專案的特定資源限制]

## Skills 配置

### 全域 Skills（所有專案共用）
- `~/.claude/skills/research-workflow/` - 研究工作流程
- `~/.claude/skills/statistical-methods/` - 基礎統計方法

### 專案特定 Skills
[如果需要額外技能，列在這裡]

**需要額外載入**:
- [ ] `~/.claude/skills/causal-inference/` - 因果推論方法
- [ ] `~/.claude/skills/text-analysis/` - 文本分析（如果分析文本）
- [ ] `~/.claude/skills/machine-learning/` - 機器學習方法
- [ ] 其他: [指定]

**專案獨有技能**:
[如果這個專案開發了獨特的方法或流程]
- [技能名稱]: [路徑] - [說明]

## Rules 配置

### 全域 Rules（必須遵守）
- `~/.claude/rules/research-principles.md` - 研究基本原則

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

### 假說文件
**位置**: `hypotheses/`
**格式**: 使用 `hypotheses/H-XXX.md` 標準格式

### 會議記錄
**位置**: `meeting_notes/`
**頻率**: [建議的 Lab Meeting 頻率]

### 分析結果
**位置**: `results/`
**組織方式**: [按假說/按時間/其他]

### 程式碼
**位置**: `code/`
**組織方式**:
```
code/
├── data-processing/
├── analysis/
├── visualization/
└── utilities/
```

### 文獻筆記
**位置**: `notes/literature/`
**格式**: [Markdown/Obsidian/其他]

## 協作與溝通

### 內部溝通
- **Lab Meeting**: [頻率]
- **進度報告**: [頻率]
- **假說審查**: [何時觸發]

### 外部溝通
[如果涉及外部協作者]
- [協作者角色]: [溝通方式和頻率]

## 版本控制

### Git 設置
- **Repository**: [如果適用]
- **Branch 策略**: [如果適用]
- **Commit 規範**: [如果適用]

### 數據版本控制
- **策略**: [如何追蹤數據版本]
- **備份**: [備份策略]

## 知識累積計畫

### 預期更新 Domain Knowledge
完成此專案後，預期更新：
- [ ] `domains/[xxx]/DOMAIN.md` - [更新什麼部分]
- [ ] `domains/[xxx]/literature/` - [新增什麼文獻]
- [ ] `domains/[xxx]/methods.md` - [新增什麼方法]

### 預期新增 Skills
- [ ] [新技能名稱] - [說明]

### 預期新增 Agents
- [ ] [新 agent 角色] - [說明]

### Lessons Learned
[專案結束後記錄]
- 什麼做得好
- 什麼需要改進
- 未來類似專案的建議

## Context Management

### 重要上下文文件
[Claude 在處理此專案時應該優先載入的文件]
1. `PROJECT.md` - 專案基本資訊
2. `domains/[xxx]/DOMAIN.md` - 領域知識
3. `hypotheses/ranking.md` - 當前假說排名
4. [其他關鍵文件]

### Context 優先級
當 context window 有限時：
1. **必須載入** (Critical):
   - PROJECT.md
   - 當前正在討論的假說
   - 相關的 DOMAIN.md

2. **應該載入** (Important):
   - 最近的 Lab Meeting 記錄
   - 假說排名表
   - 相關文獻摘要

3. **可選載入** (Optional):
   - 較早的會議記錄
   - 擱置的假說
   - 詳細的文獻筆記

### Context 刷新策略
- **何時刷新**: [例如：每週 Lab Meeting 後]
- **保留什麼**: [重要決策、活躍假說]
- **歸檔什麼**: [完成的假說、舊的討論]

## 啟動檢查清單

在開始使用此專案配置前，確認：
- [ ] PROJECT.md 已填寫完整
- [ ] DOMAIN.md 已準備好（至少有框架）
- [ ] 專案目錄結構已建立
- [ ] 所有 Agents 都已配置
- [ ] 必要的 Skills 已準備
- [ ] 已進行第一次 brainstorming 或假說提出
- [ ] Lab Manager 已建立專案追蹤表

## 特殊指示

### 給 Theorist 的指示
[任何針對 Theorist 的特殊指示]

### 給 Experimentalist 的指示
[任何針對 Experimentalist 的特殊指示]

### 給 Methodologist 的指示
[任何針對 Methodologist 的特殊指示]

### 給 Lab Manager 的指示
[任何針對 Lab Manager 的特殊指示]

## 附註

[任何其他需要說明的事項]

---

**配置建立**: [日期]
**最後更新**: [日期]
**配置版本**: 1.0
