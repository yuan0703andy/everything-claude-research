---
name: brainstorm
description: 針對特定問題進行結構化 brainstorming
---

# /brainstorm [topic]

## 目的
針對指定主題生成初步假說，模擬實驗室 brainstorming session。

這是一個結構化的假說生成流程，通過多輪次的發散和收斂，產出高品質的研究假說。

## 使用時機
- 開始新專案時
- 遇到研究瓶頸需要新想法時
- 文獻回顧後想要形成假說時
- 有初步數據觀察想要理論化時

## 前置準備
確保你已經：
1. 閱讀了相關文獻
2. 理解了研究問題的背景
3. 準備好專案的 DOMAIN.md（領域知識）
4. 明確了研究約束條件

## 流程

### Round 1: 發散階段 (10 分鐘)
**角色**: Theorist 主導

**任務**:
- 基於當前專案的領域知識，生成 5-10 個初步想法
- 不批判，鼓勵大膽嘗試
- 每個想法包含：
  - 一句話描述核心主張
  - 為什麼這個想法值得探索
  - 初步的理論依據

**輸出**: `brainstorm_ideas_initial.md`

### Round 2: 可行性初篩 (10 分鐘)
**角色**: Experimentalist 主導

**任務**:
- 快速評估每個想法的可驗證性
- 標記明顯不可行的（但記錄原因，可能未來技術進步後可行）
- 識別需要大量資源的
- 提出可行性疑問

**輸出**: 更新 `brainstorm_ideas_initial.md`，加入可行性註記

### Round 3: 深化精煉 (15 分鐘)
**角色**: Theorist + Experimentalist 協作

**任務**:
- 選擇 3-5 個最有潛力的想法
- 將其結構化為正式的假說格式
- 明確定義：
  - 核心主張
  - 理論機制
  - 可觀察預測
  - 驗證方法
  - 替代解釋

**輸出**: 3-5 個正式的假說提案文件

### Round 4: 方法論審查 (10 分鐘)
**角色**: Methodologist

**任務**:
- 對每個假說進行快速方法論審查
- 識別潛在的偏誤和陷阱
- 確保符合領域標準
- 提供改進建議

**輸出**: 為每個假說添加方法論註記

### Round 5: 記錄與歸檔 (5 分鐘)
**角色**: Lab Manager

**任務**:
- 所有想法存檔（包括被否定的，註明原因）
- 為通過的假說分配 ID (H-XXX)
- 設定初始 Elo 分數 (1200)
- 記錄 brainstorming session 的元數據

**輸出**:
- 新假說加入 `hypotheses/` 目錄
- 更新假說追蹤表
- 創建 `brainstorm_session_[date].md` 記錄

## 輸出結構

### brainstorm_session_[date].md
```markdown
# Brainstorming Session - [Topic]

**日期**: [date]
**主題**: [topic]
**參與**: Theorist, Experimentalist, Methodologist, Lab Manager

## 背景
[簡要描述為什麼進行這次 brainstorming]

## 初始想法 (Round 1)
1. [想法 1]
2. [想法 2]
...

## 可行性篩選 (Round 2)
| 想法 | 可行性 | 評論 |
|------|--------|------|
| 1 | 🟢 | ... |
| 2 | 🟡 | 需要更多資源 |
| 3 | 🔴 | 數據不可得 |

## 精煉假說 (Round 3)
[列出選中的 3-5 個假說]

## 方法論審查 (Round 4)
[Methodologist 的意見]

## 決議
- 接受假說: H-XXX, H-YYY, H-ZZZ
- 擱置想法: [列出，註明原因]
- 下一步行動: [...]
```

### hypotheses/H-XXX.md
```yaml
---
id: H-XXX
title: "[標題]"
status: 提案
created: [date]
elo: 1200
origin: "Brainstorming session on [topic]"
---

# Hypothesis H-XXX: [標題]

## Core Claim
[一句話核心主張]

## Theoretical Basis
[理論依據]

## Mechanism
[因果機制]

## Predictions
1. [預測 1]
2. [預測 2]

## Testability
[驗證方法]

## Alternative Explanations
1. [替代解釋 1]
2. [替代解釋 2]

## Boundary Conditions
[邊界條件]

## Key References
1. [文獻 1]
2. [文獻 2]

## Initial Assessment
- Confidence: [0.0-1.0]
- Feasibility: [1-5]
- Novelty: [1-5]

## Notes
[任何額外註記]
```

## 使用範例

```bash
# 在專案目錄下
/brainstorm "統計推論在高維度下的基本限制"

# 系統會自動：
# 1. 載入 domains/stats-theory/DOMAIN.md
# 2. 召集研究團隊進行 5 輪討論
# 3. 產出 brainstorm_session_[date].md
# 4. 創建新的假說文件在 hypotheses/
# 5. 更新假說追蹤表
```

## 最佳實踐
- 一次 brainstorming 不要超過一個小時
- 發散階段不要自我審查，鼓勵瘋狂想法
- 被否定的想法也要記錄原因，避免重複
- 定期回顧被擱置的想法，技術進步可能讓它們變得可行
- 不要強求產出，有時候「需要更多背景閱讀」也是有效結論

## 注意事項
- 這不是替代深度文獻閱讀的工具
- Brainstorming 產出的假說是初步的，需要後續精煉
- 如果一次 brainstorming 沒有產出可行假說，這是正常的
- 重質不重量，3 個好假說勝過 10 個勉強的假說
