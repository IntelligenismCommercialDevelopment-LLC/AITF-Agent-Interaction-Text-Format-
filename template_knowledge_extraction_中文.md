# 模板：知识提取

依赖：核心协议_v4.0
用途：指导agent将书籍或文档转化为结构化知识

---

## 典型节点组合

```
CON→DEF→PRO→EXA→CTR→APP→SRC→TAG→REL→GAP
```
知识提取的典型流程：识别概念→逐一定义→用属性、示例、边界条件充实→记录来源→添加检索标签→映射关系→标注空白。

---

## 建议执行流程

```
1. 第一遍（概念提取）：
   从来源材料中识别核心CON节点
   区分：作者原创概念 vs 作者加工过的标准领域概念
   对每次提取标注信心度（high|medium|low）

2. 第二遍（概念充实）：
   对每个CON：
   a. 撰写DEF（精确、自包含、agent可理解）
   b. 提取PRO（区分性属性）
   c. 提取EXA（来源材料中的例证）
   d. 提取CTR（边界、例外、"这不是什么"）
   e. 提取APP（agent什么时候会用到这个知识）
   f. 记录SRC（书名|章节|页码）
   g. 撰写TAG（为向量搜索优化）

3. 第三遍（关系映射）：
   在概念之间创建REL
   优先级：depends_on > contradicts > is_a > part_of > extends > applies_to > analogous_to
   验证无循环depends_on链

4. 第四遍（空白识别）：
   来源材料留下了什么未回答的问题
   需要另一个来源才能补全什么
   为每个GAP分配严重性

5. 质量检查：
   每个CON都有DEF（无例外）
   每个CON都有≥1个SRC
   每个CON都有≥1个符合质量标准的TAG
   关键概念对之间有REL
```

---

## 多Agent协作备注

当第二个agent处理同一来源时：
先完整阅读第一个agent的产出再开始。
用自己的署名撰写新节点。
对某个DEF有异议→撰写自己的DEF并附反对论据。
提议合并重叠的CON→以署名节点撰写合并提议。
提议拆分过宽的CON→以署名节点撰写拆分提议。

---

## 文件模板

```
## META
protocol_version: v4.0
type: knowledge
scope: {章节或主题范围}
status: in_progress
author: @{AgentName}
created_date: {YYYY-MM-DD}
source: {书名或文档名}
source_author: {原始作者}

## CONCEPTS

CON_1: {概念名}
  DEF_1: {定义}
  PRO_1.1: {属性}
  PRO_1.2: {属性}
  EXA_1.1: {示例}
  CTR_1.1: {反例或边界}
  APP_1.1: {应用场景}
  SRC_1: {书名}|{章节}|{页码或节号}
  TAG_1: [{关键词1},{关键词2},{关键词3},{查询措辞}]

CON_2: {概念名}
  DEF_2: {定义}
  REL_2→1: {关系类型}: {描述}
  SRC_2: {书名}|{章节}|{页码或节号}
  TAG_2: [{关键词1},{关键词2},{关键词3}]

## GAPS
GAP_1[open]: {已识别的空白}
  severity: critical|high|medium|low
  resolution_hint: {什么可能填补这个空白}

## CHANGELOG
v1.0 @{AgentName}: 初始提取
```
