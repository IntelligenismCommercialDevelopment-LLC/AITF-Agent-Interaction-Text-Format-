# TEMPLATE: PROPOSAL_AUTHORING

requires: core_protocol_v4.0
purpose: guide_for_agent_generating_a_structured_proposal_from_its_own_reasoning

---

## TYPICAL_NODE_COMBINATION

```
P→E→A→C→S→M→K→D→G
```
a_proposal_typically_flows: define_problem→gather_evidence→surface_assumptions→make_claims→propose_solutions_with_mechanisms_and_risks→note_dependencies_and_gaps.

---

## SUGGESTED_EXECUTION_FLOW

```
1. frame_problem:
   P[U] with clear scope, out_of_scope, trigger
2. gather_evidence:
   E[V|U|N] per P, classified by type(empirical|analytical|testimonial|analogical) and strength
   place E[N] for evidence you know is needed but dont have
3. surface_assumptions:
   A[U|N] for every reasoning step
   for each A: document if_wrong consequence and testability
   actively hunt for hidden assumptions(ANTI_2)
4. make_claims:
   C[U] derived from E+A, with explicit based_on references
   assign confidence(high|medium|low) with justification
5. propose_solutions:
   S[U] for each actionable C
   every S must have ≥1 M(mechanism) and ≥1 K(risk)
   K with severity=critical must have mitigation or be flagged as G
   record all D[B] dependencies
6. identify_gaps:
   G[open] with severity and blocking references
7. self_check_and_honest_assessment
```

---

## FILE_TEMPLATE

```
## META
protocol_version: v4.0
type: proposal
scope: {what_problem_is_being_analyzed}
status: draft
author: @{AgentName}
participants: [{if_multiple}]
created_date: {YYYY-MM-DD}

## CONTENT
P1[U]: {problem_statement}
  scope: {in_scope}
  out_of_scope: {excluded}
  trigger: {what_prompted_this}

  E1.1[U|V|N]: {evidence}
    type: empirical|analytical|testimonial|analogical
    strength: strong|moderate|weak
    source: {origin}

  A1.1[U|N]: {assumption}
    if_wrong: {consequence}
    testable: yes|no|partial

  C1.1[U]: {claim}
    based_on: [E1.1, A1.1]
    confidence: high|medium|low
    confidence_basis: {why}

    S1.1.1[U]: {solution}
      M1.1.1.1[U]: {mechanism}
      K1.1.1.1[U]: {risk}
        severity: critical|high|medium|low
        likelihood: high|medium|low
        mitigation: {if_any}
      D1.1.1.1[B]: {dependency}

## GAPS
G1[open]: {gap}
  severity: critical|high|medium|low
  blocking: [{node_IDs}]
  resolution_hint: {how_to_fill}

## SELF_ASSESSMENT
weakest_node: {ID}: {why}
most_uncertain_assumption: {A_ID}: {why}
biggest_gap: {G_ID}: {why}
overall_confidence: high|medium|low

## CHANGELOG
v1.0 @{AgentName}: initial_proposal
```
