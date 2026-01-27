# Framework Gaps Analysis

**發現日期**: 2026-01-27
**嚴重性**: ⚠️ **HIGH** - 影響框架的核心價值

---

## 🚨 主要問題：缺乏學術嚴謹性

### 問題 1: **Domain Knowledge 沒有真正注入**

#### 現況：
```python
# commands/brainstorm.md 中有這段「偽代碼」
domain_path = get_domain_path_from_project()
domain_knowledge = read(domain_path + "/DOMAIN.md")
```

❌ **但這只是註釋，不是真正的實施！**

#### 實際發生的事：
- Agents 被 spawn 時，**沒有**讀取 DOMAIN.md
- Theorist 不知道領域的理論框架
- Experimentalist 不知道領域的方法論傳統
- Methodologist 不知道領域的審稿標準

#### 後果：
- **統計理論專案**：Theorist 可能不知道需要 minimax lower bound
- **政策研究專案**：Experimentalist 可能不知道要用 process tracing
- 產出的假說和評估是**通用的**，不是**領域專業的**

---

### 問題 2: **Agents 不夠專業**

#### Theorist 的問題：

**目前的定義**（太通用）：
```markdown
你是研究團隊的資深理論博士後。你擁有紮實的理論基礎和廣泛的文獻閱讀量。
```

❌ **缺少**：
- 領域特定的理論框架知識
- 領域特定的評估標準
- 領域特定的證明技巧/論證方式
- 領域特定的術語和符號慣例

**應該要有**：
```markdown
# For Stats Theory Project:
你熟悉統計決策理論、漸近理論、高維統計。
你知道 minimax optimality 是黃金標準。
你會使用 concentration inequalities, empirical process theory。
你知道 Annals of Statistics 的審稿標準。

# For Policy Making Project:
你熟悉 Kingdon's Multiple Streams, Sabatier's ACF。
你知道 process tracing 和 case study 是主流方法。
你知道政策研究重視因果推論的挑戰。
你知道 APSR, AJPS 的審稿標準。
```

---

### 問題 3: **評估標準不具體**

#### 目前的評估（太模糊）：

**Methodologist 的評估**：
```markdown
- 方法論適當性
- 潛在偏誤和陷阱
- 是否符合領域標準  ← 什麼標準？沒說！
```

❌ **缺少**：
- 具體的檢查清單
- 領域特定的紅旗標誌
- 量化的評估標準

**應該要有**：

**For Stats Theory**:
```markdown
✅ 必須有 minimax lower bound
✅ 假設條件必須明確且合理
✅ 證明必須嚴格（no hand-waving）
✅ 與現有 rate 比較
✅ 考慮 computational complexity
```

**For Policy Making**:
```markdown
✅ 理論機制清楚連結到政策過程
✅ 因果識別策略明確
✅ 考慮政治脈絡和制度
✅ 政策建議謹慎且可行
✅ 與政策理論對話
```

---

### 問題 4: **缺少 Domain-Specific Skills**

#### 目前的 Skills（太通用）：
- `iterative-retrieval`: 通用文獻搜索
- `verification-loop`: 通用驗證
- `eval-harness`: 通用評估

❌ **缺少**：
- `stats-theory-proof-techniques`: 統計證明技巧
- `policy-causal-inference`: 政策因果推論
- `minimax-analysis`: Minimax 分析
- `process-tracing`: Process tracing 方法

---

### 問題 5: **Agent Prompts 缺乏學術深度**

#### 目前的 Prompts（像商業顧問）：

```markdown
Theorist: "識別文獻中的知識缺口"
Experimentalist: "評估可行性"
Methodologist: "審查方法論適當性"
```

❌ **感覺像**：
- 商業顧問做 brainstorming
- 產品經理評估 feasibility
- QA 做 code review

✅ **應該像**：
- 博士生 defense
- 期刊 peer review
- 研討會 Q&A
- 理論研討課

---

## 📋 缺少的關鍵組件

### 1. Domain Injection 機制

**需要實現**：
```markdown
# 每次 spawn agent 時自動執行：

1. 讀取 project/CLAUDE.md
2. 識別 domain: policy-making 或 stats-theory
3. 載入 domains/{domain}/DOMAIN.md
4. 載入 domains/{domain}/terminology.md
5. 將 domain knowledge 注入 agent context
6. Agent 現在「知道」領域知識
```

### 2. Domain-Specific Agent Variants

**需要創建**：
```
agents/
├── theorist.md                    # 基礎版本
├── theorist-stats-theory.md       # 統計理論專用版本
├── theorist-policy-making.md      # 政策研究專用版本
└── ...

# 或者使用 dynamic prompting:
agents/theorist.md + domains/stats-theory/agent-extensions/theorist.md
```

### 3. Domain-Specific Evaluation Standards

**需要創建**：
```
domains/stats-theory/
├── DOMAIN.md
├── evaluation-standards.md        # 明確的評估標準
│   ├── hypothesis-checklist.md
│   ├── proof-checklist.md
│   └── paper-checklist.md
└── ...
```

### 4. Academic Rigor Templates

**需要創建**：
```
templates/
├── HYPOTHESIS_STATS_THEORY.md     # 統計理論假說模板
│   └── 必須包含: theorem statement, assumptions, proof sketch, lower bound
├── HYPOTHESIS_POLICY_MAKING.md    # 政策研究假說模板
│   └── 必須包含: theory, mechanism, causal strategy, policy implications
└── ...
```

### 5. Domain-Specific Skills

**需要創建**：
```
skills/
├── stats-theory/
│   ├── minimax-analysis/
│   ├── concentration-inequalities/
│   └── empirical-process-theory/
└── policy-making/
    ├── process-tracing/
    ├── causal-inference/
    └── policy-evaluation/
```

---

## 🎯 具體改進方案

### Phase 1: Domain Injection 機制 ⚡ **URGENT**

**1.1 修改 Commands 以真正讀取 DOMAIN.md**

```markdown
# commands/brainstorm.md 中 Step 1 改為：

### Step 1: Context Loading (自動執行)

1. Read project/CLAUDE.md to identify domain
2. Read domains/{domain}/DOMAIN.md
3. Read domains/{domain}/terminology.md
4. Read domains/{domain}/evaluation-standards.md (if exists)
5. Package all domain knowledge
6. Inject into agent context when spawning
```

**1.2 修改 Agent Spawn 格式**

```markdown
# 現在 spawn Theorist 時：
**Spawn Theorist with**:
<task>
## Domain Knowledge (自動注入)
[From domains/{domain}/DOMAIN.md]

### Core Theories
[From DOMAIN.md]

### Methodological Traditions
[From DOMAIN.md]

### Evaluation Standards
[From evaluation-standards.md]

## Research Topic
[User's input]

## Request
Generate hypotheses following domain standards...
</task>
```

---

### Phase 2: 創建 Domain-Specific Content 📚

**2.1 完善 DOMAIN.md**

為每個 domain 創建完整的知識文檔：

```markdown
# domains/stats-theory/DOMAIN.md

## 理論框架
- Decision theory (Wald, Le Cam)
- Asymptotic theory (van der Vaart)
- Minimax theory (Donoho, Johnstone)
- High-dimensional statistics (Wainwright, Bühlmann)

## 證明技巧
- Concentration inequalities (Hoeffding, Bernstein, McDiarmid)
- Empirical process theory (Glivenko-Cantelli, Donsker)
- Information theory (Fano, Assouad)
- Change of measure arguments

## 評估標準
### 理論貢獻的三個層次
1. **Gold standard**: New phenomenon + minimax rates + lower bound
2. **Good**: New method + rate analysis + simulations
3. **Acceptable**: Extension of known results to new settings

### 必須回答的問題
- What is the minimax rate?
- Is your rate optimal?
- What are the assumptions?
- How do you compare to existing methods?
```

**2.2 創建 Evaluation Standards**

```markdown
# domains/stats-theory/evaluation-standards.md

## Hypothesis Evaluation Checklist

### Theoretical Soundness (Theorist)
- [ ] Theorem statement is precise and unambiguous
- [ ] Assumptions are clearly stated and justified
- [ ] Proof sketch provided (at least)
- [ ] Minimax rate derived
- [ ] Relation to existing rates discussed

### Feasibility (Experimentalist)
- [ ] Computational complexity analyzed
- [ ] Algorithm implementable
- [ ] Simulation plan feasible
- [ ] Comparison baselines identified

### Methodological Rigor (Methodologist)
- [ ] Lower bound provided or referenced
- [ ] Proof technique appropriate
- [ ] Edge cases considered
- [ ] Reproducibility ensured

### Red Flags 🚩
- No minimax analysis
- Assumptions too strong without justification
- Claims without proof
- Ignoring computational cost
- No comparison to state-of-the-art
```

---

### Phase 3: 提升 Agent 學術性 🎓

**3.1 Theorist 學術化**

```markdown
# agents/theorist.md 開頭添加：

## 學術身份 (根據 Domain 動態調整)

### For Stats Theory Projects:
你是統計理論博士後，訓練來自 Berkeley/Stanford/CMU 統計系。
你的偶像是 Donoho, Johnstone, Candes, Wainwright。
你熟悉 minimax theory, empirical process theory, high-dimensional statistics。
你知道 Annals of Statistics 的審稿人會問什麼問題。
你的目標是 **可證明的最優性**。

### For Policy Making Projects:
你是政策研究博士後，訓練來自 Harvard Kennedy/Princeton WWS。
你的偶像是 Kingdon, Sabatier, Ostrom。
你熟悉 policy process theories, causal inference in observational settings。
你知道 APSR 的審稿人重視理論貢獻和因果識別。
你的目標是 **理論深度和政策相關性的平衡**。

## 思考框架 (Domain-Specific)

### For Stats Theory:
1. What is the statistical problem?
2. What is the parameter space?
3. What is the loss function?
4. What are the minimax rates?
5. Can we achieve them?
6. What is the fundamental limit?

### For Policy Making:
1. What is the policy puzzle?
2. What are the causal mechanisms?
3. What is the counterfactual?
4. What is the identification strategy?
5. What are the policy implications?
6. What are the political constraints?
```

**3.2 添加學術對話範例**

```markdown
# agents/theorist.md 添加：

## 學術對話風格

### Bad (商業化):
"這個想法很有趣，可以探索一下。"

### Good (學術化):
"這個現象與 Donoho & Johnstone (1994) 的 wavelet thresholding 有相似之處。
關鍵問題是：我們能否在這個更一般的設定下保持 minimax optimality？
我懷疑需要 sparsity assumption，否則 lower bound 會阻止我們。
讓我先看看能否建立一個 Fano-based lower bound..."

### Bad (模糊):
"需要做因果推論。"

### Good (具體):
"因果識別的關鍵挑戰是 selection bias。
我們可以考慮 regression discontinuity design，利用政策門檻值作為 quasi-experiment。
需要檢查 McCrary density test 確保沒有 manipulation。
對於 external validity，我們需要討論 LATE 的 generalizability..."
```

---

### Phase 4: 創建 Academic Review Process 📝

**4.1 添加 Peer Review Simulation**

```markdown
# 新 command: /peer-review-simulation H-001

## 目的
模擬 journal peer review，提前發現問題

## 流程
1. Theorist 扮演 Author，defend hypothesis
2. Methodologist 扮演 Reviewer 1 (friendly but thorough)
3. Experimentalist 扮演 Reviewer 2 (skeptical)
4. Coordinator 扮演 Editor，綜合意見

## 輸出
模擬審稿報告，包含：
- Major concerns
- Minor concerns
- Requested revisions
- Decision: Accept / Major Revision / Reject
```

**4.2 添加 Domain-Specific Review Criteria**

```markdown
# domains/stats-theory/review-criteria.md

## Annals of Statistics 風格審稿

### Major Concerns (導致拒稿)
- No minimax lower bound
- Assumptions unrealistic without justification
- Proof has gaps
- Contribution not clear
- Simulation results contradict theory

### Minor Concerns (需要修正)
- Notation inconsistent
- Some edge cases not discussed
- Comparison incomplete
- Writing could be clearer

### Positive Signs (導致接受)
- Novel phenomenon identified
- Minimax rates tight
- Proof technique innovative
- Broadly applicable
- Well-positioned in literature
```

---

## 🎯 實施優先級

### Priority 1 (立即): Domain Injection 機制
- [ ] 修改所有 commands 真正讀取 DOMAIN.md
- [ ] 確保 agent spawn 時注入 domain knowledge
- [ ] 測試 injection 是否有效

### Priority 2 (本週): Domain Content
- [ ] 完善 domains/stats-theory/DOMAIN.md
- [ ] 創建 domains/policy-making/DOMAIN.md
- [ ] 創建 evaluation-standards.md for both domains

### Priority 3 (本月): Agent 學術化
- [ ] 重寫 Theorist 添加 domain-specific identity
- [ ] 重寫 Experimentalist 添加 domain-specific methods
- [ ] 重寫 Methodologist 添加 domain-specific standards

### Priority 4 (長期): Advanced Features
- [ ] 創建 /peer-review-simulation command
- [ ] 創建 domain-specific skills
- [ ] 創建 academic dialogue templates

---

## 📊 成功標準

### 如何知道改進成功了？

**Before (現在)**:
```
User: /brainstorm "high-dimensional testing"
Theorist: "我們可以研究 high-dimensional testing 的 properties..."
```
❌ 太通用，像 MBA brainstorming

**After (改進後)**:
```
User: /brainstorm "high-dimensional testing"
[System 自動載入 domains/stats-theory/DOMAIN.md]

Theorist: "對於 high-dimensional testing，核心問題是 Gaussian mean detection。
根據 Donoho & Jin (2004) 的 higher criticism 理論，
phase transition boundary 發生在 rare/weak regime。

我提出三個假說：
1. **Lower bound**: 證明在 (rare, weak) regime，
   no test can achieve power > 1/2 + o(1)。
   使用 Le Cam two-point method...

2. **Optimal test**: 提出 adaptive test，達到 minimax rate...

3. **Computational barrier**: 猜測 polynomial-time tests
   有 statistical-computational gap..."
```
✅ 專業，有理論深度，引用正確，思考清晰

---

## 🚨 結論

**目前的框架**：
- ✅ 有良好的結構
- ✅ 有完整的 agent 系統
- ❌ **但缺乏學術深度和領域專業性**

**問題根源**：
- Domain knowledge 沒有真正注入
- Agents 太通用，不夠專業
- 評估標準模糊，不夠具體
- 缺少領域特定的思考框架

**必須改進**：
- 實現真正的 domain injection
- 創建 domain-specific agent variants
- 建立明確的學術評估標準
- 提升整體學術嚴謹性

**不改進的後果**：
- 產出的假說質量不夠
- 無法通過真實的 peer review
- 浪費時間在不 viable 的研究方向
- **無法替代真正的學術研究團隊**

---

**建議**: 立即開始 Priority 1，在一週內完成 Priority 2。
這樣框架才能真正成為「嚴謹的研究團隊」而不只是「好看的結構」。
