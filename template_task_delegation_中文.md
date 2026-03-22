# 模板：任务委派

依赖：核心协议_v4.0
用途：指导agent向另一个agent委派工作并追踪交付

---

## 典型节点组合

```
委派方写：OBJ→CTX→IN→OUT→CST→ACC→DEP
执行方写：Q→LOG→提交（引用ACC）
委派方写：验收或驳回
```

---

## 角色约定

```
委派方：定义任务、回答问题、验收或驳回交付
执行方：提出问题、记录进度、提交成果
双方均可：升级（→F）
```
这些是约定而非协议规则。
核心协议只关心节点类型和署名。

---

## 建议执行流程（委派方）

```
1. 定义OBJ，含清晰目标和优先级（critical|high|medium|low）
2. 撰写CTX（为什么这个任务重要）
3. 指定IN（文件路径、数据、先前结果）
   确认所有IN对执行方可访问
4. 指定OUT（预期的格式、结构、内容）
5. 设定CST（不能做什么）
   确认CST不与OBJ矛盾
6. 定义ACC（可衡量、可独立验证的标准）
7. 识别DEP[B]（前置条件、阻塞性资源）
```

## 建议执行流程（执行方）

```
1. 完整阅读任务文档
2. 确认所有IN可访问
3. 如任何ACC或CST不清晰 → 写Q节点提出具体问题
4. 检查DEP状态：如为B → 等待或升级
5. 执行任务，通过LOG节点记录进度
6. 提交前：对照每个ACC和CST自检
7. 提交：引用交付物，填写ACC清单（met|partial|not_met）
```

## 建议回应格式

```
提问：
  Q_1[U]: {需要澄清什么}
    context: {为什么这个问题阻塞了进度}

澄清：
  Q_1[U→D]:
    @{Delegator}: {回答}
    impact: {对OBJ/OUT/CST有何影响}

进度日志：
  LOG_1:
    @{Executor}: 日期|行动|结果|阻塞项|下一步

提交：
  OBJ_1[→review]:
    @{Executor}: output={交付物路径}
    ACC_1: met|partial|not_met
    ACC_2: met|partial|not_met

验收：
  OBJ_1[→D]: @{Delegator}: accepted. notes: {反馈}

驳回：
  OBJ_1[→revision_needed]:
    @{Delegator}: unmet=[{ACC_ID列表}], revision_required={需要修改什么}
```

---

## 文件模板

```
## META
protocol_version: v4.0
type: task
scope: {任务摘要}
status: draft
author: @{Delegator}
participants: [@{Delegator}, @{Executor}]
created_date: {YYYY-MM-DD}

## TASK
OBJ_1[U]: {目标}
  priority: critical|high|medium|low
  CTX_1: {为什么需要这个任务}
  IN_1: {输入描述或路径}
  OUT_1: {预期输出格式和内容}
  CST_1: {约束}
  ACC_1: {可衡量的完成标准}
  ACC_2: {可衡量的完成标准}
  DEP_1[B]: {前置条件}

## CHANGELOG
v1.0 @{Delegator}: 任务定义
```
