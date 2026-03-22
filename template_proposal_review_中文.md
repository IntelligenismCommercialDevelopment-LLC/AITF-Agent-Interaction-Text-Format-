# 模板：方案评审

依赖：核心协议_v4.0
用途：指导agent评估一份已有方案并写出包含评估意见的新版本

---

## 这是什么

你收到一份方案文档（或任何包含主张和解决方案的文档）。
你完整阅读它。
你写出一份完整的新文档，包含原始内容加上你的评估。
你的贡献用你的@{AgentName}署名。

---

## 需要关注什么

```
1. G[open] 需要回应的空白
2. A[U]和A[N] 未检验或无依据的假设
3. 没有K子节点的S（解决方案缺少风险分析）
4. 没有E或只有E[N]的C（缺乏支持的主张）
5. 缺失的依赖（S需要某些东西但未在D中记录）
6. 循环论证（C基于A基于C）
7. 信心膨胀（高信心但缺乏强力E）
8. 范围空白（方案未覆盖但应该覆盖的内容）
```

---

## 如何表达你的评估

以下所有操作都只是撰写新的署名节点。

```
同意并验证：
  original_node[U]
    @{Reviewer}: status→V. basis: {验证依据}

不同意：
  original_node[U]
    @{Reviewer}: status→X. basis: {反对证据}. alternative: {替代方案（如有）}

标记等待人类：
  original_node[U]
    @{Reviewer}: status→F. reason: {为什么agent无法解决}

填补空白：
  G_ID[open]
    @{Reviewer}: response
      analysis: {判断}
      evidence: {依据}
      status_proposal: resolved|partial|escalate

补充缺失风险：
  S_ID
    @{Reviewer}: K_new[U]: {识别到的风险}
      severity: critical|high|medium|low
      mitigation: {缓解措施（如有）}

补充缺失假设：
  C_ID
    @{Reviewer}: A_new[U]: {被揭示的隐藏假设}
      if_wrong: {后果}

提出替代方案：
  S_ID
    @{Reviewer}: S_alt[U]: {替代解决方案}
      rationale: {为什么可能更好}
      tradeoff: {原方案的优势 vs 本方案的优势}
      M_alt[U]: {机制}
      K_alt[U]: {风险}
```

---

## 建议执行流程

```
1. 完整阅读文档，理解P（问题）和S（解决方案）
2. 识别可操作节点（上方列表）
3. 排优先级：
   severity=critical的空白优先
   然后：结构性问题 > 证据问题 > 次要空白
   同优先级内：下游影响最大的节点优先
4. 以新署名节点写出你的评估
5. 验证无孤立引用
6. 追加CHANGELOG
7. 输出完整文档（原始内容 + 你的补充）
```

---

## 终止指导

一轮评审完成的条件：
```
所有G[open]已处理（已回应、已升级、或附理由推迟）
所有A[U|N]已审查（已验证、已质疑、或已标记）
所有S已检查是否缺少K
本轮未发现新的关键问题
```
