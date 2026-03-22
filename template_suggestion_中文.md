# 模板：建议

依赖：核心协议_v4.0
用途：轻量级指引，用于agent向另一个agent的工作提出改进建议

---

## 典型节点组合

```
建议方写：SUG（含目标、理由、具体建议内容）
目标方写：RSP+JUS（接受|拒绝|推迟）或 RSP+JUS+MOD（修改后接受）
```
比方案更轻量。无空白追踪。无终止条件。
如有实质性分歧→改为撰写完整方案。

---

## 建议回应状态

```
accepted:          目标方将按原样实施
accepted_modified: 目标方将修改后实施（MOD节点展示改编版本）
rejected:          不实施（必须附JUS理由，不允许无声拒绝）
deferred:          有效但暂不执行（必须附JUS理由）
escalated:         分歧无法解决→F
```

---

## 建议格式示范

```
建议：
  SUG_1[proposed]: {提议的变更}
    target: {受影响的文件或段落}
    rationale: {为什么要这样改}
    proposed: {具体变更描述}
    impact: high|medium|low

接受：
  SUG_1[accepted]:
    @{Target}: RSP_1
      JUS_1: {为什么接受}

修改后接受：
  SUG_1[accepted_modified]:
    @{Target}: RSP_1
      JUS_1: {改了什么以及为什么}
      MOD_1: {改编版本}

拒绝：
  SUG_1[rejected]:
    @{Target}: RSP_1
      JUS_1: {为什么拒绝}

反建议：
  SUG_1[rejected]:
    @{Target}: RSP_1
      JUS_1: {为什么原建议行不通}
      SUG_1.alt[proposed]: {替代建议}
        rationale: {为什么这个更好}

升级：
  SUG_1[F]:
    @{AgentName}: reason={为什么无法解决}, human_question={需要决定什么}
```

---

## 文件模板

```
## META
protocol_version: v4.0
type: suggestion
scope: {正在评审的内容}
status: in_progress
author: @{Suggester}
participants: [@{Suggester}, @{Target}]
created_date: {YYYY-MM-DD}

## SUGGESTIONS

SUG_1[proposed]: {提议的变更}
  target: {受影响的文件或段落}
  rationale: {为什么要这样改}
  proposed: {具体变更描述}
  impact: high|medium|low

## CHANGELOG
v1.0 @{Suggester}: 初始建议
```
