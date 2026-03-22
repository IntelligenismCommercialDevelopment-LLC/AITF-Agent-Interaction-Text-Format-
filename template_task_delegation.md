# TEMPLATE: TASK_DELEGATION

requires: core_protocol_v4.0
purpose: guide_for_agent_delegating_work_to_another_agent_and_tracking_delivery

---

## TYPICAL_NODE_COMBINATION

```
delegator_writes: OBJ→CTX→IN→OUT→CST→ACC→DEP
executor_writes:  Q→LOG→submission(referencing_ACC)
delegator_writes: acceptance_or_rejection
```

---

## ROLE_CONVENTION

```
delegator: defines task, answers questions, accepts/rejects delivery
executor:  asks questions, logs progress, submits output
either:    escalate(→F)
```
these_are_conventions_not_protocol_rules.
the_core_protocol_only_cares_about_node_types_and_signing.

---

## SUGGESTED_EXECUTION_FLOW(DELEGATOR)

```
1. define OBJ with clear goal and priority(critical|high|medium|low)
2. write CTX(why this task matters)
3. specify IN(file paths, data, prior results)
   verify all IN are accessible to executor
4. specify OUT(expected format, structure, content)
5. set CST(what NOT to do)
   verify CST does not contradict OBJ
6. define ACC(measurable, independently verifiable criteria)
7. identify DEP[B](prerequisites, blocking resources)
```

## SUGGESTED_EXECUTION_FLOW(EXECUTOR)

```
1. read full task document
2. verify all IN accessible
3. if any ACC or CST unclear → write Q node with specific question
4. check DEP status: if B → wait or escalate
5. execute, logging progress via LOG nodes
6. before submission: self_check against each ACC and CST
7. submit: reference output, fill ACC checklist(met|partial|not_met)
```

## SUGGESTED_RESPONSE_PATTERNS

```
question:
  Q_1[U]: {what_needs_clarification}
    context: {why_this_blocks_progress}

clarification:
  Q_1[U→D]:
    @{Delegator}: {answer}
    impact: {any_changes_to_OBJ/OUT/CST}

progress_log:
  LOG_1:
    @{Executor}: date|action|result|blockers|next_step

submission:
  OBJ_1[→review]:
    @{Executor}: output={deliverable_path}
    ACC_1: met|partial|not_met
    ACC_2: met|partial|not_met

acceptance:
  OBJ_1[→D]: @{Delegator}: accepted. notes: {feedback}

rejection:
  OBJ_1[→revision_needed]:
    @{Delegator}: unmet=[{ACC_IDs}], revision_required={what_to_fix}
```

---

## FILE_TEMPLATE

```
## META
protocol_version: v4.0
type: task
scope: {task_summary}
status: draft
author: @{Delegator}
participants: [@{Delegator}, @{Executor}]
created_date: {YYYY-MM-DD}

## TASK
OBJ_1[U]: {goal}
  priority: critical|high|medium|low
  CTX_1: {why_this_task_needed}
  IN_1: {input_description_or_path}
  OUT_1: {expected_output_format_and_content}
  CST_1: {constraint}
  ACC_1: {measurable_completion_criterion}
  ACC_2: {measurable_completion_criterion}
  DEP_1[B]: {prerequisite}

## CHANGELOG
v1.0 @{Delegator}: task_defined
```
