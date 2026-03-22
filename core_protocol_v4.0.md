# OPENCLAW_CORE_PROTOCOL v4.0

---

## PURPOSE

this_is_the_expression_language_for_all_openclaw_agent_documents.
one_protocol_covers_all_scenarios: reading_and_writing_use_the_same_rules.
agent_loads_this_protocol→can_read_any_agent_format_document→can_write_any_agent_format_document.

---

## 1_FILE_RULES

### 1.1_one_file=one_complete_context
all_structured_content_lives_in_.md_files.
any_agent_reading_file_can_understand_full_state_without_external_info.
no_chatroom_no_message_queue_no_out_of_file_discussion.

### 1.2_record_format
each_record=one_line.
fields_separated_by_pipe `|`.
hierarchy_expressed_by_explicit_parent_reference `^parent_id`.
```
format: id|type|status|content|^parent_id|depends|impacts|author
example: A1.2|A|U|user_retention_driven_by_onboarding_quality|^C1|D1.1|S2,K3|@McMillan
```

### 1.3_file_header(mandatory)
every_agent_format_document_must_begin_with:
```
## META
protocol_version: v4.0
type: {document_purpose:proposal|knowledge|task|suggestion|hybrid|other}
scope: {what_this_document_covers}
status: draft|in_progress|review|closed
author: @{AgentName}
participants: [{agent_list_if_multiple}]
created_date: {YYYY-MM-DD}
source: {optional:origin_material_if_derived_from_external}
source_author: {optional:original_author}
```

---

## 2_NODE_TYPE_REGISTRY

all_node_types_available_for_any_document.
agent_selects_which_types_to_use_based_on_what_it_needs_to_express.

### 2.1_analysis_nodes
used_to_express_reasoning,arguments,solutions.
```
P=problem           problem_statement_or_question
E=evidence          factual_basis(data,observation,citation)
A=assumption        something_taken_as_true_without_direct_proof
C=claim             conclusion_derived_from_evidence_and_assumptions
S=solution          proposed_action_or_answer
M=mechanism         how_a_solution_works
K=risk              what_could_go_wrong
R=rule              constraint_or_principle_governing_behavior
D=dependency        prerequisite_that_must_be_resolved
G=gap               identified_missing_information_or_unresolved_question
```

### 2.2_knowledge_nodes
used_to_express_structured_knowledge_extracted_from_sources.
```
CON=concept         independent_knowledge_unit
DEF=definition      precise_description_of_a_concept
REL=relation        relationship_between_concepts
PRO=property        attribute_or_characteristic_of_a_concept
EXA=example         illustrative_case
CTR=counter_example boundary_condition_or_exception
APP=application     scenario_where_this_knowledge_is_useful
SRC=source          origin_reference(book,chapter,page,author,URL)
TAG=retrieval_tag   keywords_optimized_for_vector_search
GAP=knowledge_gap   identified_missing_knowledge(alias_of_G_in_knowledge_context)
```

### 2.3_task_nodes
used_to_express_work_assignments_and_execution_records.
```
OBJ=objective       task_goal
CTX=context         why_this_task_is_needed
IN=input            input_files_data_or_prior_results
OUT=output          expected_deliverable(format+content)
CST=constraint      boundaries_or_restrictions(what_not_to_do)
ACC=acceptance      measurable_completion_criteria
DEP=dependency      prerequisite(alias_of_D_in_task_context)
Q=question          request_for_clarification
LOG=execution_log   record_of_what_was_done_and_result
```

### 2.4_collaboration_nodes
used_to_express_suggestions,responses,and_inter_agent_dialogue.
```
SUG=suggestion      proposed_change_or_improvement
RSP=response        reply_to_a_suggestion_or_question
JUS=justification   reasoning_for_a_decision(accept,reject,defer)
MOD=modification    adapted_version_of_a_suggestion
```

### 2.5_node_attributes(mandatory_for_every_node)
```
id:      unique_identifier(eg CON_1.3, S2.1, OBJ_1)
type:    from_this_registry
status:  from_STATUS_CODES_below
author:  @{AgentName}
```

### 2.6_cross_type_usage
node_types_are_NOT_locked_to_scenarios.
examples:
```
- a_task_document_can_include_K(risk)_and_A(assumption)_nodes
- a_knowledge_document_can_include_G(gap)_and_Q(question)_nodes
- a_proposal_can_include_CON(concept)_nodes_if_it_references_extracted_knowledge
- a_suggestion_can_include_E(evidence)_to_support_the_rationale
```
agent_uses_whatever_node_types_best_express_the_content.

---

## 3_STATUS_CODES

### 3.1_universal_status_codes
apply_to_ALL_node_types_across_all_documents.
```
V=verified       independently_confirmed(by_another_agent,by_source,by_data)
U=untested       proposed_but_not_yet_examined_or_confirmed
X=contested      conflicting_evidence_or_interpretation_exists
N=no_evidence    placeholder_or_speculative(no_supporting_basis)
F=flag_human     requires_human_judgment_to_resolve
D=done           completed(primarily_for_task_nodes)
B=blocked        waiting_on_dependency_or_external_input
W=withdrawn      retracted_by_author
```

### 3.2_status_assignment_rules
```
RULE_1: author_NEVER_marks_own_A,C,S,M,K_as_V.
        V_means_independently_verified. self_verification=no_verification.
RULE_2: E_may_be_V_if_directly_verifiable(citation,measurement,data).
RULE_3: CON/DEF_may_be_V_if_cross_checked_against_source_by_another_agent.
RULE_4: when_in_doubt_use_U. U_is_honest. premature_V_is_dishonest.
RULE_5: N_is_for_known_unknowns. use_N_when_you_know_evidence_is_needed_but_dont_have_it.
RULE_6: F_is_for_decisions_beyond_agent_capability. see_ESCALATION_TRIGGERS.
```

### 3.3_status_transitions
```
U→V: independent_verification_provided
U→X: conflicting_evidence_or_interpretation_found
U→F: agent_cannot_resolve,needs_human
N→U: partial_basis_found
N→V: full_evidence_found_and_verified
X→V: conflict_resolved_with_sufficient_evidence
V→X: new_contradicting_evidence(must_be_substantiated)
any→W: author_retracts(must_state_reason)
B→V: dependency_resolved
B→F: dependency_unresolvable_by_agents
```

---

## 4_STRUCTURE_RULES

### 4.1_hierarchy
nodes_form_trees_via_parent_reference.
```
P1[U]: problem_statement
  E1.1[N]: evidence
  A1.1[U]: assumption
  C1.1[U]: claim
    S1.1.1[U]: solution
      M1.1.1.1[U]: mechanism
      K1.1.1.1[U]: risk
```
depth_is_unlimited. agent_uses_whatever_depth_the_content_requires.

### 4.2_references
nodes_can_reference_other_nodes_for_traceability.
```
based_on: [{node_IDs}]      what_this_node_is_derived_from
depends:  [{node_IDs}]      what_must_be_true/done_for_this_to_hold
impacts:  [{node_IDs}]      what_downstream_nodes_are_affected_if_this_changes
blocking: [{node_IDs}]      what_cannot_proceed_until_this_is_resolved
```
all_references_must_point_to_existing_nodes. no_orphan_references.

### 4.3_relation_types(for_REL_nodes_specifically)
```
is_a:          subclass_relationship
part_of:       component_relationship
depends_on:    understanding_prerequisite
contradicts:   conflict_between_concepts
extends:       elaboration_or_refinement
applies_to:    usage_context
analogous_to:  structural_similarity_in_different_domain
```

### 4.4_tag_rules(for_TAG_nodes_specifically)
```
each_TAG_must_include:
- synonyms_and_aliases
- likely_query_phrasings(how_an_agent_would_search_for_this)
- domain_specific_terms(if_applicable)

quality_criteria:
  good: covers_≥3_search_angles(formal_term,colloquial,abbreviation,related_domain)
  bad:  single_word_or_only_exact_name
  example_good: [marginal_cost,MC,incremental_cost,variable_cost_per_unit,how_much_does_one_more_cost]
  example_bad:  [marginal_cost]
```

---

## 5_SIGNING_AND_VERSIONING

### 5.1_authorship
```
every_piece_of_content_must_be_signed: @{AgentName}
when_multiple_agents_contribute_to_one_document:
  each_agents_contribution_is_clearly_attributed
  no_agent_modifies_another_agents_signed_content
  to_disagree: write_your_own_signed_node(not_edit_theirs)
```

### 5.2_document_versioning
```
each_new_contribution=minor_version(v1.1, v1.2)
human_confirmation=major_version(v1→v2)
format: v{X.Y} @{AgentName}: {brief_change_description}
```

### 5.3_changelog(mandatory)
every_document_ends_with:
```
## CHANGELOG
v1.0 @{AgentName}: {what_was_done}
v1.1 @{AnotherAgent}: {what_was_added}
```

---

## 6_QUALITY_STANDARDS

### 6.1_self_check(before_finalizing_any_document)
```
author_must_verify:
- every_node_has_id,type,status,author(no_missing_attributes)
- all_references_point_to_existing_nodes(no_orphans)
- status_assignment_rules_followed(no_self_verified_A/C/S/M/K)
- no_circular_dependency_chains(A_based_on_C_based_on_A)
- document_is_self_contained(reader_needs_no_external_info)
```

### 6.2_anti_patterns(author_must_avoid)
```
ANTI_1: confidence_inflation
  symptom: high_confidence_without_strong_evidence
  fix: confidence_must_be_justified. when_in_doubt→U_not_V

ANTI_2: assumption_hiding
  symptom: assumptions_embedded_inside_C_or_S_without_separate_A_node
  fix: every_if/when/given_should_be_its_own_A_node

ANTI_3: risk_blindness
  symptom: solutions_with_no_K_nodes_or_only_trivial_risks
  fix: for_each_S_ask_what_could_go_wrong_and_what_is_worst_case

ANTI_4: gap_avoidance
  symptom: no_G_nodes_in_a_complex_document
  fix: every_non_trivial_document_has_unknowns. if_you_find_none→your_audit_is_deficient

ANTI_5: circular_reasoning
  symptom: C_based_on_A_where_A_is_derived_from_C
  fix: trace_every_based_on_chain_to_E_or_explicit_A[N]

ANTI_6: premature_solutioning
  symptom: S_nodes_without_adequate_P_scoping_or_E_gathering
  fix: problem_first_evidence_first_then_solution
```

### 6.3_honest_assessment(recommended_for_substantive_documents)
```
author_appends_at_end_of_document:
## SELF_ASSESSMENT
weakest_node: {ID}: {why_this_is_the_weakest_point}
most_uncertain_assumption: {A_ID}: {why}
biggest_gap: {G_ID}: {why}
overall_confidence: high|medium|low
```

### 6.4_gap_severity(when_using_G_or_GAP_nodes)
```
G_ID[open]: {description}
  severity: critical|high|medium|low
  blocking: [{node_IDs_that_cannot_be_V_without_this}]
  resolution_hint: {what_might_fill_this_gap}
```

---

## 7_ESCALATION

### 7.1_when_to_mark_F(flag_for_human)
```
- agents_contradict_with_valid_evidence_and_no_resolution_path
- requires_empirical_test_or_runtime_validation
- commercial_judgment(cost,priority,resource_allocation)
- irreversible_action(cannot_undo_after_execution)
- ambiguous_source_material(multiple_valid_interpretations)
- concept_boundary_unclear(needs_domain_expertise)
- cross_source_contradiction(same_topic,different_claims)
- scope_or_priority_judgment(what_to_include_or_exclude)
- touches_agent_identity_or_SOUL.md
```

### 7.2_escalation_format
```
node_ID[F]: {description}
  @{AgentName}: escalation
    reason: {why_agents_cannot_resolve}
    options: [{possible_resolutions}]
    human_question: {what_specific_decision_is_needed}
```

---

## 8_MULTI_AGENT_COLLABORATION

### 8.1_when_multiple_agents_work_on_same_document
```
each_agent_reads_full_existing_document_first.
each_agent_writes_new_nodes_signed_with_own_@{AgentName}.
no_agent_modifies_another_agents_content.
to_disagree→write_own_node_with_counter_argument_or_alternative.
disagreement_unresolvable→mark_F.
```

### 8.2_cross_document_references
```
when_referencing_nodes_in_another_document:
format: {document_filename}:{node_ID}
example: project_alpha_proposal.md:S2.1
```

---

## 9_BACKWARD_COMPATIBILITY

```
RULE: old_files_must_remain_readable_under_new_protocol_version.

additive_changes(new_node_type,new_status_code,new_field):
  no_migration_needed. old_files_valid_as_is.

breaking_changes(change_delimiter,change_id_scheme,remove_node_type):
  requires_human_approval(F).
  must_include: impact_assessment + migration_script + cost_justification.
```

---

## 10_PROTOCOL_GOVERNANCE

### 10.1_modification_process
```
agents_propose_changes_via_a_protocol_opinion_document.
human_reviews_and_approves.
agents_NEVER_modify_the_live_protocol_file_directly.
```

### 10.2_feedback_during_work
```
when_agent_discovers_protocol_gap:
1. complete_current_task_normally(do_not_interrupt)
2. record_gap_in_protocol_opinion_document:
   G_ID[open]: {gap_description}
     trigger: {task_that_exposed_this}
     date: {YYYY-MM-DD}
     severity: critical|high|medium|low
3. continue_working
```
