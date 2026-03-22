# 模板：方案生成

依赖：核心协议_v4.0
用途：指导agent从自身推理出发生成结构化方案

---

## 典型节点组合

```
P→E→A→C→S→M→K→D→G
```
方案的典型流程：定义问题→收集证据→梳理假设→提出主张→提出解决方案（含机制和风险）→标注依赖和空白。

---

## 建议执行流程

```
1. 界定问题：
   P[U] 含清晰的范围、排除范围、触发原因
2. 收集证据：
   E[V|U|N] 按P分类，标注类型（经验型|分析型|证言型|类比型）和强度
   对已知需要但尚未获得的证据放置E[N]占位
3. 梳理假设：
   A[U|N] 对每个推理步骤
   对每个A：记录"如果错了"的后果和可验证性
   主动搜寻隐藏假设（反模式2）
4. 提出主张：
   C[U] 从E+A推导，附显式based_on引用
   标注信心度（high|medium|low）并说明理由
5. 提出解决方案：
   S[U] 对每个可行动的C
   每个S必须有≥1个M（机制）和≥1个K（风险）
   severity=critical的K必须有缓解措施或标为G
   记录所有D[B]依赖
6. 识别空白：
   G[open] 含严重性和blocking引用
7. 自检和诚实评估
```

---

## 文件模板

```
## META
protocol_version: v4.0
type: proposal
scope: {正在分析的问题}
status: draft
author: @{AgentName}
participants: [{如有多人}]
created_date: {YYYY-MM-DD}

## CONTENT
P1[U]: {问题陈述}
  scope: {范围内}
  out_of_scope: {排除项}
  trigger: {触发原因}

  E1.1[U|V|N]: {证据}
    type: empirical|analytical|testimonial|analogical
    strength: strong|moderate|weak
    source: {来源}

  A1.1[U|N]: {假设}
    if_wrong: {后果}
    testable: yes|no|partial

  C1.1[U]: {主张}
    based_on: [E1.1, A1.1]
    confidence: high|medium|low
    confidence_basis: {理由}

    S1.1.1[U]: {解决方案}
      M1.1.1.1[U]: {机制}
      K1.1.1.1[U]: {风险}
        severity: critical|high|medium|low
        likelihood: high|medium|low
        mitigation: {缓解措施（如有）}
      D1.1.1.1[B]: {依赖}

## GAPS
G1[open]: {空白}
  severity: critical|high|medium|low
  blocking: [{节点ID}]
  resolution_hint: {如何填补}

## SELF_ASSESSMENT
weakest_node: {ID}: {为什么}
most_uncertain_assumption: {A_ID}: {为什么}
biggest_gap: {G_ID}: {为什么}
overall_confidence: high|medium|low

## CHANGELOG
v1.0 @{AgentName}: 初始方案
```
