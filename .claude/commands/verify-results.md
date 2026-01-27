---
name: verify-results
description: 驗證分析結果（對應 GSD verify-work）
---

# /verify-results

系統性驗證分析結果的正確性和穩健性。

## 目的

確保研究結果：
- 數據品質合格
- 分析方法正確
- 結果穩健可靠
- 符合驗證標準

## 前置條件

- [ ] 已完成 `/execute-analysis`
- [ ] 結果文件已產生
- [ ] 驗證標準已定義（`/eval define`）

## 流程

### 1. 三階段驗證（Verification Loop）

**Experimentalist** 執行驗證循環：

#### Phase 1: BUILD - 代碼可執行性

```markdown
## Build Sanity Checks

- [ ] 代碼無錯誤運行
- [ ] 所有依賴項可用
- [ ] 數據文件正確載入
- [ ] 結果文件成功產生
- [ ] 隨機種子已設定

### 測試
```bash
# 清除環境後重新運行
Rscript --vanilla analysis/main_analysis.R

# 或 Python
python analysis/main_analysis.py
```

### 結果
✅ PASS / ❌ FAIL

### 問題
[列出發現的問題]
```

#### Phase 2: FUNCTIONALITY - 結果正確性

```markdown
## Functionality Checks

### 數據驗證
- [ ] 樣本量符合預期（N = [預期值]）
- [ ] 無異常缺失值（< 5%）
- [ ] 變數範圍合理
- [ ] 無重複觀測
- [ ] 處理過程無錯誤

### 統計假設
- [ ] 線性假設（殘差圖）
- [ ] 常態性（Q-Q plot, Shapiro test）
- [ ] 同質變異（BP test）
- [ ] 獨立性（Durbin-Watson）
- [ ] 無多重共線性（VIF < 10）

### 結果合理性
- [ ] 係數方向符合預期
- [ ] 效果量在合理範圍
- [ ] 標準誤合理
- [ ] 顯著性符合預期
- [ ] 與文獻一致

### 測試結果
- Normality test p-value: [值]
- Homoscedasticity test p-value: [值]
- VIF max: [值]

### 結果
✅ PASS / ⚠️ WARNING / ❌ FAIL

### 警告和問題
[列出需要注意的問題]
```

#### Phase 3: QUALITY - 穩健性和品質

```markdown
## Quality Checks

### Robustness
- [ ] 替代規格 1: [結果一致性]
- [ ] 替代規格 2: [結果一致性]
- [ ] 子樣本分析: [結果一致性]
- [ ] 排除 outliers: [結果穩定性]
- [ ] Bootstrap SE: [與原結果比較]

### Sensitivity Analysis
- [ ] 不同控制變數組合
- [ ] 不同樣本定義
- [ ] 不同估計方法

### 可重現性
- [ ] 在清除環境下可重現
- [ ] 種子固定，結果一致
- [ ] 文檔完整可追蹤

### Publication Ready
- [ ] 圖表清晰完整
- [ ] 表格格式正確
- [ ] 結果解釋準確
- [ ] 限制已說明

### pass@k Metrics
- pass@1: [%] (單次執行通過率)
- pass@3: [%] (三次執行通過率)
- pass^3: [%] (複製+穩健+重現)

### 結果
✅ PASS / ❌ FAIL

### 需要改進
[列出需要改進的地方]
```

### 2. Methodologist 審查

**Methodologist** 進行方法論審查：

```markdown
## Methodology Review

### 研究設計
- [ ] 研究設計適當
- [ ] 因果推論邏輯清晰
- [ ] 識別策略合理
- [ ] 控制變數充分

### 統計方法
- [ ] 方法選擇正確
- [ ] 假設檢驗適當
- [ ] 多重比較已調整
- [ ] 效果量報告

### 倫理和透明度
- [ ] 所有分析都已報告
- [ ] 探索性 vs 驗證性清楚
- [ ] 數據使用符合倫理
- [ ] 限制已說明

### 評級
- 嚴重問題 (CRITICAL): [N]
- 主要問題 (MAJOR): [N]
- 次要問題 (MINOR): [N]
- 建議 (SUGGESTION): [N]

### 結論
✅ 通過審查 / ⚠️ 有條件通過 / ❌ 需要重大修改
```

### 3. 產生驗證報告

Lab Manager 產生 `reviews/H-XXX_verification_report.md`：

```markdown
# Verification Report: H-XXX

**日期**: [YYYY-MM-DD]
**假說**: [假說標題]
**分析者**: Experimentalist
**審查者**: Methodologist

## 執行摘要

- **整體結論**: ✅ PASS / ⚠️ CONDITIONAL PASS / ❌ FAIL
- **pass@3 指標**: [%]
- **建議**: [繼續 / 修改後繼續 / 重新設計]

## Verification Loop 結果

### Phase 1: Build (可執行性)
[✅/❌] 狀態
[問題描述]

### Phase 2: Functionality (正確性)
[✅/⚠️/❌] 狀態
[檢查詳情]

### Phase 3: Quality (穩健性)
[✅/❌] 狀態
[Robustness 分析結果]

## Methodologist 審查

### 評級分佈
- CRITICAL: [N] 個
- MAJOR: [N] 個
- MINOR: [N] 個
- SUGGESTION: [N] 個

### 主要發現
1. [發現 1]
2. [發現 2]

### 必須修正
- [ ] [CRITICAL 問題 1]
- [ ] [MAJOR 問題 1]

### 建議改進
- [ ] [MINOR 問題 1]
- [ ] [建議 1]

## 對照 Eval Criteria

讀取 `/eval` 定義的標準並檢查：

| 標準 | 預期 | 實際 | 狀態 |
|------|------|------|------|
| [標準 1] | [值] | [值] | ✅/❌ |
| [標準 2] | [值] | [值] | ✅/❌ |

## 決定

### 是否通過驗證？
[✅ 是 / ❌ 否]

### 如果通過
- [ ] 更新假說狀態為「已驗證」
- [ ] 更新 Elo 排名（+50）
- [ ] 進入 writeup 階段

### 如果未通過
- [ ] 記錄需要修正的問題
- [ ] 制定修正計畫
- [ ] 回到 `/execute-analysis`

## 下一步

[具體的下一步行動]

## 附件

- 分析腳本: `analysis/`
- 結果文件: `results/`
- 圖表: `figures/`
- 診斷圖: `diagnostics/`
```

### 4. 更新狀態

自動執行：
```
- 更新 STATE.md
- 更新 hypotheses/H-XXX.md
- 如果通過：更新 Elo 排名
- 創建 .planning/phases/phase-N-VERIFY.md
```

## 輸出

1. **驗證報告**: `reviews/H-XXX_verification_report.md`
2. **診斷圖表**: `diagnostics/` 目錄
3. **Phase 記錄**: `.planning/phases/phase-N-VERIFY.md`
4. **更新的 STATE.md**

## 通過標準

### 最低要求
- [ ] Phase 1 (Build): PASS
- [ ] Phase 2 (Functionality): PASS or WARNING（無 CRITICAL）
- [ ] Phase 3 (Quality): pass@3 ≥ 80%
- [ ] Methodologist: 無 CRITICAL 問題

### 理想標準
- [ ] 所有三個 phases: PASS
- [ ] pass@3 ≥ 90%
- [ ] Methodologist: 無 MAJOR 問題
- [ ] 所有 eval criteria 滿足

## 常見失敗原因

### CRITICAL 級別
- 數據品質嚴重問題
- 統計假設嚴重違反
- 結果完全不可重現
- 方法論錯誤

**處理**: 必須修正才能繼續

### MAJOR 級別
- 部分假設違反
- Robustness 問題
- 文檔不完整

**處理**: 建議修正，可有條件通過

### MINOR 級別
- 小的代碼品質問題
- 圖表格式問題
- 文檔小錯誤

**處理**: 記錄但不阻止通過

## 與其他 Commands 的關係

```
/execute-analysis        執行分析
        ↓
/verify-results          ← 當前步驟
        ↓
    [通過?]
   ↙      ↘
[是]      [否]
  ↓         ↓
/update-state   回到 /execute-analysis
  ↓
/lab-meeting    (決定下一步)
```

## 使用範例

```
User: /verify-results H-003