# TEMPLATE: KNOWLEDGE_EXTRACTION

requires: core_protocol_v4.0
purpose: guide_for_agent_converting_books_or_documents_into_structured_knowledge

---

## TYPICAL_NODE_COMBINATION

```
CON→DEF→PRO→EXA→CTR→APP→SRC→TAG→REL→GAP
```
knowledge_extraction_typically_flows: identify_concepts→define_each→enrich_with_properties_examples_boundaries→record_source→add_retrieval_tags→map_relationships→note_gaps.

---

## SUGGESTED_EXECUTION_FLOW

```
1. first_pass(concept_extraction):
   identify core CON nodes from source material
   distinguish: authors_novel_concepts vs standard_domain_concepts
   assign confidence(high|medium|low) to each extraction

2. second_pass(concept_enrichment):
   for each CON:
   a. write DEF(precise, self_contained, agent_readable)
   b. extract PRO(distinguishing attributes)
   c. extract EXA(illustrative cases from source)
   d. extract CTR(boundaries, exceptions, what_this_is_NOT)
   e. extract APP(when would an agent use this knowledge)
   f. record SRC(book|chapter|page)
   g. write TAG(optimized for vector search)

3. third_pass(relationship_mapping):
   create REL between concepts
   priority: depends_on > contradicts > is_a > part_of > extends > applies_to > analogous_to
   verify no circular depends_on chains

4. fourth_pass(gap_identification):
   what the source leaves unanswered
   what would need another source to complete
   assign severity to each GAP

5. quality_check:
   every CON has DEF(no exceptions)
   every CON has ≥1 SRC
   every CON has ≥1 TAG meeting quality criteria
   key concept pairs have REL
```

---

## MULTI_AGENT_NOTE

when_second_agent_processes_same_source:
read_first_agents_output_fully_before_starting.
write_own_nodes_with_own_signature.
to_disagree_with_a_DEF→write_own_DEF_with_counter_argument.
to_propose_merging_overlapping_CONs→write_merge_proposal_as_signed_node.
to_propose_splitting_overbroad_CON→write_split_proposal_as_signed_node.

---

## FILE_TEMPLATE

```
## META
protocol_version: v4.0
type: knowledge
scope: {chapters_or_topic_range}
status: in_progress
author: @{AgentName}
created_date: {YYYY-MM-DD}
source: {book_or_document_name}
source_author: {original_author}

## CONCEPTS

CON_1: {concept_name}
  DEF_1: {definition}
  PRO_1.1: {property}
  PRO_1.2: {property}
  EXA_1.1: {example}
  CTR_1.1: {counter_example_or_boundary}
  APP_1.1: {application_scenario}
  SRC_1: {book}|{chapter}|{page_or_section}
  TAG_1: [{keyword1},{keyword2},{keyword3},{query_phrasing}]

CON_2: {concept_name}
  DEF_2: {definition}
  REL_2→1: {relation_type}: {description}
  SRC_2: {book}|{chapter}|{page_or_section}
  TAG_2: [{keyword1},{keyword2},{keyword3}]

## GAPS
GAP_1[open]: {identified_gap}
  severity: critical|high|medium|low
  resolution_hint: {what_might_fill_this}

## CHANGELOG
v1.0 @{AgentName}: initial_extraction
```
