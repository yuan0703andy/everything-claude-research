---
name: execute-analysis
description: 執行分析並記錄過程（對應 GSD execute-phase）
---

# /execute-analysis

執行研究分析，記錄完整過程。

## 目的

在明確的計畫下執行分析：
- 遵循預先定義的分析計畫
- 記錄所有步驟和決策
- 保存中間結果
- 產生階段總結

## 前置條件

- [ ] 已完成 `/review-hypothesis`（有明確的驗證計畫）
- [ ] 數據已準備完成
- [ ] 分析腳本已規劃
- [ ] 驗證標準已定義（`/eval define`）

## 流程

### 1. 確認分析計畫

Lab Manager 讀取並確認：
```
- 讀取 hypotheses/H-XXX.md 的驗證計畫
- 讀取 STATE.md 確認當前階段
- 檢查 .planning/phases/phase-N-PLAN.md
```

詢問：
- 這次要執行哪個假說的分析？
- 是主要分析還是 robustness check？

### 2. 執行分析

**Experimentalist** 主導執行：

#### 數據驗證
```r
# 檢查數據品質
- 樣本量符合預期？
- 無異常缺失值？
- 變數分佈合理？
- 假設前提滿足？
```

#### 主要分析
```
- 執行預先指定的分析
- 記錄所有參數選擇
- 保存中間結果
- 產生圖表
```

#### Robustness Checks
```
- 替代規格
- 子樣本分析
- 敏感性分析
```

### 3. 記錄過程

Lab Manager 產生 `.planning/phases/phase-N-EXECUTE.md`：

```markdown
# Phase N - Execute: H-XXX 分析

**開始時間**: [YYYY-MM-DD HH:MM]
**結束時間**: [YYYY-MM-DD HH:MM]
**執行者**: Experimentalist

## 分析目標
[本次分析要驗證什麼]

## 數據驗證

### 檢查項目
- [x] 樣本量: N = 1000（符合預期）
- [x] 缺失值: 0.5%（可接受）
- [x] 變數分佈: 正常

### 發現的問題
- [問題描述]
- 解決方式: [如何處理]

## 執行步驟

### 1. 數據準備
- [x] 讀取原始數據
- [x] 清理和轉換
- [x] 產生分析數據集

### 2. 主要分析
- [x] 描述性統計
- [x] 主要迴歸
- [x] 產生表格和圖

### 3. Robustness Checks
- [x] 替代規格 1: [結果]
- [x] 替代規格 2: [結果]
- [x] 子樣本分析: [結果]

## 初步結果

### 主要發現
1. [發現 1]: [描述和統計量]
2. [發現 2]: [描述和統計量]

### 與預期的比較
- ✅ 與假說預測一致
- ⚠️ 效果量小於預期
- ❌ 某個預測未得到支持

### Robustness
- 結果在不同規格下穩健
- 子樣本分析結果一致

## 產生的檔案

- `analysis/01_main_analysis.R` - 主要分析腳本
- `results/main_results.csv` - 主要結果
- `results/robustness_results.csv` - Robustness 結果
- `figures/fig1_main_effect.png` - 主要效果圖
- `figures/fig2_robustness.png` - Robustness 圖

## 遇到的問題

### 問題 1: [描述]
- **嚴重性**: 低/中/高
- **如何解決**: [解決方法]
- **影響**: [對結果的影響]

## Code Quality

- [x] 隨機種子已設定
- [x] 代碼有註釋
- [x] 結果可重現
- [x] 無硬編碼值

## 下一步

- [ ] 進入 `/verify-results` 階段
- [ ] 需要額外的 robustness check？
- [ ] 是否需要調整假說？

## 備註

[其他重要資訊]
```

### 4. 更新狀態

自動執行：
```
- 更新 STATE.md（記錄完成的分析）
- 更新 hypotheses/H-XXX.md（記錄執行狀態）
- 在 .planning/phases/ 創建 phase-N-EXECUTE.md
```

## 輸出

1. **Phase 執行記錄**: `.planning/phases/phase-N-EXECUTE.md`
2. **分析結果**: `results/` 目錄下的文件
3. **圖表**: `figures/` 目錄下的文件
4. **更新的 STATE.md**

## 成功標準

- [ ] 所有計畫的分析都已執行
- [ ] 結果記錄完整
- [ ] 產生的文件都已保存
- [ ] 問題和解決方法都已記錄
- [ ] 代碼可重現

## 與其他 Commands 的關係

```
/review-hypothesis        定義驗證計畫
        ↓
/eval define H-XXX       設定成功標準
        ↓
/execute-analysis        ← 當前步驟
        ↓
/verify-results          驗證分析結果
        ↓
/update-state            更新研究狀態
```

## 使用範例

```
User: /execute-analysis