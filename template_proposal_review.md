# TEMPLATE: PROPOSAL_REVIEW

requires: core_protocol_v4.0
purpose: guide_for_agent_evaluating_an_existing_proposal_and_writing_a_new_version_with_its_assessment

---

## WHAT_THIS_IS

you_receive_a_proposal_document(or_any_document_containing_claims_and_solutions).
you_read_it_fully.
you_write_a_new_complete_document_that_includes_the_original_content_plus_your_assessment.
your_contribution_is_signed_with_your_@{AgentName}.

---

## WHAT_TO_LOOK_FOR

```
1. G[open] gaps needing response
2. A[U] and A[N] untested or baseless assumptions
3. S nodes without K children (solutions missing risk analysis)
4. C nodes without E or with E[N] (unsupported claims)
5. missing dependencies (S that requires something not captured in D)
6. circular reasoning (C based_on A based_on C)
7. confidence inflation (high confidence without strong E)
8. scope gaps (what the proposal doesnt address but should)
```

---

## HOW_TO_EXPRESS_YOUR_ASSESSMENT

all_of_these_are_just_writing_new_signed_nodes.

```
agree_and_verify:
  original_node[U]
    @{Reviewer}: status→V. basis: {evidence_for_verification}

disagree:
  original_node[U]
    @{Reviewer}: status→X. basis: {counter_evidence}. alternative: {if_any}

flag_for_human:
  original_node[U]
    @{Reviewer}: status→F. reason: {why_agents_cannot_resolve}

fill_a_gap:
  G_ID[open]
    @{Reviewer}: response
      analysis: {judgment}
      evidence: {basis}
      status_proposal: resolved|partial|escalate

add_missing_risk:
  S_ID
    @{Reviewer}: K_new[U]: {identified_risk}
      severity: critical|high|medium|low
      mitigation: {if_any}

add_missing_assumption:
  C_ID
    @{Reviewer}: A_new[U]: {hidden_assumption_surfaced}
      if_wrong: {consequence}

propose_alternative:
  S_ID
    @{Reviewer}: S_alt[U]: {alternative_solution}
      rationale: {why_this_might_be_better}
      tradeoff: {what_original_gains_vs_what_this_gains}
      M_alt[U]: {mechanism}
      K_alt[U]: {risk}
```

---

## SUGGESTED_EXECUTION_FLOW

```
1. read full document, understand P(problem) and S(solutions)
2. identify actionable nodes(list above)
3. prioritize:
   severity=critical gaps first
   then: structural issues > evidence issues > minor gaps
   within same priority: nodes with most downstream impact first
4. write your assessment as new signed nodes
5. verify no orphan references
6. append CHANGELOG
7. output complete document(original + your additions)
```

---

## TERMINATION_GUIDANCE

a_review_round_is_complete_when:
```
all G[open] addressed(responded, escalated, or deferred_with_justification)
all A[U|N] examined(verified, contested, or flagged)
all S checked for missing K
no new critical issues identified in this round
```
