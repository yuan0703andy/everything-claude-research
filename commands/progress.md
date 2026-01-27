---
name: progress
description: 快速查看研究進度和狀態
---

# /progress

快速顯示當前研究狀態，用於 session 開始或需要快速了解進度時。

## 目的

提供**一目了然**的進度總覽：
- 當前在哪個階段
- 假說排名狀況
- 最近的關鍵決定
- 待解決問題
- 建議下一步

## 使用時機

**推薦場景**：
- ✅ 新 session 開始時（最常用）
- ✅ 離開一段時間後回來
- ✅ 需要快速回顧時
- ✅ 向 PI/collaborator 報告前
- ✅ 不確定該做什麼時

## 流程

### 1. 讀取核心文件

Lab Manager 讀取：
```
必讀：
- STATE.md
- hypotheses/HYPOTHESES.md
- PROJECT.md

選讀：
- RESEARCH_PLAN.md（如果存在）
- .planning/phases/latest（最近的 phase）
- meeting_notes/latest（最近的會議）
```

### 2. 生成進度報告

```markdown
# 研究進度報告

**專案**: [專案名稱]
**生成時間**: 2024-01-27 15:30
**Session**: #12

---

## 📍 當前位置

**階段**: Phase 2 - 假說驗證 (40% 完成)

**進行中**:
- H-003 完成 Experimentalist 審查，待 Methodologist
- H-007 草稿完成，等待審查

**下一步**:
1. 🎯 完成 H-003 Methodologist 審查（優先）
2. 🔄 更新 Elo 排名
3. 📝 開始 H-007 深度審查

---

## 🏆 假說排名 (Top 5)

| # | Δ | ID | Elo | 標題 | 狀態 | 進度 |
|---|---|-----|-----|------|------|------|
| 1 | - | H-003 | 1580 | 高維稀疏迴歸的 minimax rate | 🔄 審查中 | ████░░ 67% |
| 2 | ↑1 | H-001 | 1520 | 自適應估計的收斂速度 | ✅ 已驗證 | ██████ 100% |
| 3 | ↓1 | H-007 | 1450 | 非參數回歸的最優帶寬 | 📝 草稿 | ██░░░░ 33% |
| 4 | - | H-005 | 1380 | 變點檢測的漸近性質 | 🔄 審查中 | ███░░░ 50% |
| 5 | - | H-002 | 1200 | 因果效應的識別 | ⏸️ 擱置 | ░░░░░░ 0% |

**總計**: 7 個假說（2 已驗證，3 審查中，1 草稿，1 擱置）

---

## 🎯 專案進度

```
Phase 1: Context        ████████████ 100% ✅ (2024-01-15 完成)
Phase 2: Hypothesis     ████████░░░░  67% 🔄 (進行中)
Phase 3: Analysis       ░░░░░░░░░░░░   0% 📝 (待開始)
Phase 4: Writeup        ░░░░░░░░░░░░   0% 📝 (待開始)

整體進度: ████░░░░░░░░ 33%
```

**預計時間線**:
- Phase 2 完成: 2024-02-15 (剩餘 2 週)
- Phase 3 完成: 2024-03-31 (剩餘 6 週)
- Phase 4 完成: 2024-04-30 (剩餘 9 週) 🎯 投稿目標

---

## ✅ 最近完成 (近 3 次 session)

- **Session #12** (2024-01-27): H-003 Experimentalist 審查完成
- **Session #11** (2024-01-26): H-007 初稿完成
- **Session #10** (2024-01-25): H-001 驗證通過（pass@3 = 95%）

---

## 🔑 關鍵決定 (近期)

- **2024-01-27** H-002 因數據不可得而擱置
- **2024-01-26** 排除 2008 年前數據（結構性斷點）
- **2024-01-25** 選擇 diff-in-diff 作為主要方法

---

## ❓ 待解決問題 (3 個)

**優先級 HIGH**:
- [ ] H-003 的樣本量是否足夠？需要 power analysis

**優先級 MEDIUM**:
- [ ] 應該用 cluster SE 還是 robust SE？
- [ ] H-007 的理論基礎需要補強

---

## 💡 建議下一步

### 立即行動 (本次 session)
1. **完成 H-003 審查** - `/review-hypothesis H-003` 完成 Methodologist 部分
2. **更新排名** - 如果通過審查，更新 Elo 分數
3. **Power analysis** - 解決樣本量疑慮

### 本週計畫
- 完成 H-007 的深度審查
- 開始 Top 2 假說的分析腳本準備
- 解決 cluster vs robust SE 問題

### 下週 Lab Meeting 前
- 準備 H-003 和 H-007 的進度報告
- 更新所有假說狀態
- 準備下階段計畫

---

## ⚠️ 風險與警示

- **🔴 HIGH**: 4 月截止日期接近，Phase 2 需要加快
- **🟡 MEDIUM**: H-003 和 H-007 理論可能重疊，需釐清
- **🟢 LOW**: 計算資源充足

---

## 📊 統計

- **Session 次數**: 12
- **工作時數**: ~18 小時
- **假說生成數**: 7
- **已驗證假說**: 2
- **通過率**: 28.6% (2/7)
- **平均 Elo**: 1425

---

## 📚 Context 提醒

- **Domain**: domains/stats-theory/ (高維統計理論)
- **PI 偏好**: 理論清晰 > 計算效率
- **資源限制**: 公開數據only，單機計算足夠
- **方法論**: Frequentist 為主，Bayesian 為輔
```

### 3. 互動式問答（可選）

Lab Manager 可以詢問：
```
📊 進度報告已生成

想要查看詳情嗎？
- [1] 查看特定假說詳情（例如：H-003）
- [2] 查看最近的 phase 記錄
- [3] 查看完整的假說列表
- [4] 查看時間線和截止日期
- [5] 直接開始工作

你的選擇：_
```

## 輸出格式

### 簡潔版（默認）
適合快速瀏覽：
```
# 進度快照

📍 Phase 2 - 假說驗證 (67%)
🎯 進行中：H-003 審查
🏆 Top 假說：H-003 (1580 Elo)
✅ 最近完成：H-001 驗證通過

⏭️ 下一步：
1. 完成 H-003 Methodologist 審查
2. 更新 Elo 排名
3. 開始 H-007 審查

⚠️ 風險：4月截止日期接近
```

### 詳細版
包含所有資訊（如上面的完整報告）

## 使用範例

### 場景 1: Session 開始

```
User: /progress

Lab Manager:
[生成進度報告]

歡迎回來！上次是 2 天前（Session #11）

現在的狀態：
- Phase 2 進行中（67%）
- H-003 等待你的 Methodologist 審查
- 2 個待解決問題

建議從：
/review-hypothesis H-003

需要先回顧什麼嗎？
```

### 場景 2: 不確定該做什麼

```
User: /progress

Lab Manager:
[生成進度報告]

當前最優先的任務：
1. 🔥 完成 H-003 審查（優先級 HIGH）
2. 解決樣本量問題
3. 準備下週 Lab Meeting

你想從哪個開始？
```

### 場景 3: 向 PI 報告

```
User: /progress

Lab Manager:
[生成詳細版報告]

這份報告包含：
- 7 個假說，2 個已驗證
- Phase 2 進度 67%
- 3 個待解決問題
- 風險警示

你可以：
- 導出為 PDF 報告
- 複製到會議準備文件
- 生成 slides 大綱
```

## 進階功能

### 篩選選項

```bash
# 只看高優先級假說
/progress --priority high

# 只看特定狀態
/progress --status "審查中"

# 只看風險和問題
/progress --warnings

# 只看下一步建議
/progress --next-steps
```

### 對比功能

```bash
# 與上週比較
/progress --compare last-week

# 與目標比較
/progress --compare target

輸出：
進度變化：
- 假說數：5 → 7 (+2)
- 已驗證：1 → 2 (+1)
- 平均 Elo：1350 → 1425 (+75)
```

### 導出功能

```bash
# 導出為 Markdown
/progress --export md

# 導出為簡報大綱
/progress --export slides

# 產生週報
/progress --export weekly-report
```

## 與其他 Commands 的關係

```
[Session 開始]
        ↓
/progress           ← 查看當前狀態
        ↓
[選擇要做的工作]
        ↓
[執行 commands：brainstorm, review, execute, verify]
        ↓
[Session 結束前]
        ↓
/update-state      ← 更新狀態
        ↓
[下次 Session]
        ↓
/progress          ← 循環
```

## 資料來源優先級

如果文件衝突，優先級為：
1. **STATE.md** - 最高優先級（最新的單一真相來源）
2. **hypotheses/HYPOTHESES.md** - 假說排名
3. **.planning/phases/** - Phase 詳情
4. **RESEARCH_PLAN.md** - 計畫和時間線
5. **meeting_notes/** - 會議決定

## 自動提醒

Lab Manager 會在 `/progress` 時檢查並提醒：

- ⏰ 截止日期接近（<2 週）
- 📉 進度落後於計畫
- ⚠️ 有 HIGH 優先級問題未解決
- 📅 Lab Meeting 時間到了
- 🎯 里程碑達成

## 最佳實踐

### 每次 Session 開始
```
1. /progress          # 快速了解狀態
2. [讀取建議的下一步]
3. [選擇任務並開始]
```

### Session 中途迷失
```
1. /progress --next-steps   # 看看應該做什麼
2. [回到正軌]
```

### 需要報告時
```
1. /progress --export weekly-report
2. [生成格式化的進度報告]
3. [分享給團隊/PI]
```

## 注意事項

- `/progress` **只讀取**，不修改任何文件
- 資訊來自 STATE.md 和相關文件
- 如果 STATE.md 過時，先運行 `/update-state`
- 建議每次 session 開始都運行一次

## 輸出示例（簡潔版）

```
╔══════════════════════════════════════════╗
║     📊 Research Progress Snapshot      ║
╚══════════════════════════════════════════╝

📍 Phase 2 - Hypothesis Review (67%)
⏱️  Session #12 | 2024-01-27 15:30

🎯 Current Focus:
   H-003 | Minimax rates | 1580 Elo | 🔄 Review

🏆 Top 3 Hypotheses:
   1️⃣  H-003 (1580) ████████████░░░ 67%
   2️⃣  H-001 (1520) ██████████████ 100% ✅
   3️⃣  H-007 (1450) ████░░░░░░░░░░ 33%

✅ Last 3 Wins:
   • H-003 Experimentalist review
   • H-007 draft completed
   • H-001 verification passed (95%)

❗ Blockers (1):
   • H-003 sample size unclear

⏭️  Next Steps:
   1. /review-hypothesis H-003 (Methodologist)
   2. Update Elo rankings
   3. Power analysis for H-003

⚠️  Deadline: 9 weeks until submission

Type /progress --detailed for full report
```
