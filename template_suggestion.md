# TEMPLATE: SUGGESTION

requires: core_protocol_v4.0
purpose: lightweight_guide_for_agent_proposing_improvements_to_another_agents_work

---

## TYPICAL_NODE_COMBINATION

```
suggester_writes: SUG(with_target,rationale,proposed_change)
target_writes:    RSP+JUS(accept|reject|defer) or RSP+JUS+MOD(accept_modified)
```
lighter_than_proposal. no_gap_tracking. no_termination_conditions.
for_substantive_disagreements→write_a_full_proposal_instead.

---

## SUGGESTED_RESPONSE_STATUSES

```
accepted:          target_will_implement_as_is
accepted_modified: target_will_implement_with_changes(MOD_node_shows_adapted_version)
rejected:          not_implemented(JUS_required, no_silent_rejection)
deferred:          valid_but_not_now(JUS_required)
escalated:         disagreement_unresolvable→F
```

---

## SUGGESTED_PATTERNS

```
suggestion:
  SUG_1[proposed]: {proposed_change}
    target: {file_or_section_affected}
    rationale: {why_this_change}
    proposed: {specific_change_description}
    impact: high|medium|low

acceptance:
  SUG_1[accepted]:
    @{Target}: RSP_1
      JUS_1: {why_accepted}

modified_acceptance:
  SUG_1[accepted_modified]:
    @{Target}: RSP_1
      JUS_1: {what_changed_and_why}
      MOD_1: {adapted_version}

rejection:
  SUG_1[rejected]:
    @{Target}: RSP_1
      JUS_1: {why_rejected}

counter_suggestion:
  SUG_1[rejected]:
    @{Target}: RSP_1
      JUS_1: {why_original_doesnt_work}
      SUG_1.alt[proposed]: {alternative_suggestion}
        rationale: {why_this_is_better}

escalation:
  SUG_1[F]:
    @{AgentName}: reason={why_unresolvable}, human_question={what_to_decide}
```

---

## FILE_TEMPLATE

```
## META
protocol_version: v4.0
type: suggestion
scope: {what_is_being_reviewed}
status: in_progress
author: @{Suggester}
participants: [@{Suggester}, @{Target}]
created_date: {YYYY-MM-DD}

## SUGGESTIONS

SUG_1[proposed]: {proposed_change}
  target: {file_or_section_affected}
  rationale: {why_this_change}
  proposed: {specific_change_description}
  impact: high|medium|low

## CHANGELOG
v1.0 @{Suggester}: initial_suggestions
```
