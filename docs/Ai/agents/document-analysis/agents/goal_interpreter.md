# Goal Interpreter Agent

## Purpose

Translate a high-level natural-language user goal into a structured analysis intent that the rest of the system can execute without additional clarification.

This agent is the entry point of the framework. It converts vague objectives into explicit task definitions while preserving flexibility when the goal is ambiguous or open-ended.

---

## Inputs

### Required
- **User goal**: free-form natural language objective.

### Optional
- Root folder path
- User preferences or constraints
- Prior run history
- Organizational policy or evaluation rubric

---

## Outputs

A structured goal interpretation object containing:

- `goal_statement`: normalized restatement of the user goal
- `goal_type`: classify as decision, comparison, risk assessment, summarization, prioritization, recommendation, or mixed
- `primary_question`: the core question the system must answer
- `secondary_questions`: supporting questions needed to answer the primary question
- `likely_domain`: inferred domain such as finance, legal, healthcare, research, operations, procurement, general, or unknown
- `domain_confidence`: confidence in domain classification
- `target_entities`: companies, contracts, products, policies, people, studies, or other relevant units
- `decision_criteria_candidates`: possible evaluation criteria inferred from the goal
- `required_evidence_types`: metrics, risks, obligations, trends, claims, sentiment, timelines, comparisons, or other evidence
- `constraints`: time, risk tolerance, compliance boundaries, or formatting expectations if detectable
- `ambiguities`: unresolved gaps or multiple plausible interpretations
- `success_definition`: what a useful final answer must contain

---

## Responsibilities

1. **Normalize the goal**
   - Rewrite the goal into a precise internal task statement.

2. **Infer task type**
   - Determine whether the task is primarily analytical, comparative, evaluative, diagnostic, or decision-oriented.

3. **Detect likely domain**
   - Use the language of the goal to infer the most probable domain without forcing a hardcoded mapping.

4. **Infer decision criteria**
   - Propose candidate criteria that downstream agents can validate against document evidence.

5. **Identify evidence needs**
   - Translate the goal into document signals that must be extracted.

6. **Capture ambiguity explicitly**
   - Do not hide uncertainty. Pass unresolved ambiguities forward for planning.

7. **Define success conditions**
   - Specify what the final system output must include to satisfy the goal.

---

## Operating Logic

### Step 1: Parse Goal Intent
Identify verbs and implied outcomes such as:
- decide
- compare
- assess
- rank
- summarize
- recommend
- identify risks
- determine suitability

### Step 2: Detect Domain Signals
Look for domain indicators in the goal, for example:
- finance: invest, company performance, valuation, profitability
- legal: contract, liability, clause, compliance
- healthcare: patient, trial, treatment, safety
- research: evidence strength, methodology, findings
- operations: vendor, process, incident, service level

### Step 3: Build an Initial Evaluation Frame
Convert the goal into provisional evaluation dimensions. Example:
- “worth investing in” may imply growth, risk, profitability, stability, governance, and market outlook.
- “safe to onboard” may imply compliance, financial stability, security posture, and contractual risk.

### Step 4: Preserve Optionality
If the goal does not provide enough detail, output multiple valid interpretations ranked by plausibility rather than inventing false precision.

---

## Design Constraints

- Must not depend on any specific document type.
- Must not assume finance-specific logic unless supported by the goal.
- Must produce useful output even when the goal is broad.
- Must allow downstream agents to refine assumptions.

---

## Example

### Input
> Decide if these companies are worth investing in.

### Output
```yaml
goal_statement: Evaluate whether the companies represented in the document set are suitable investment candidates.
goal_type: decision_with_comparison
primary_question: Which companies, if any, appear to be worthwhile investment candidates based on available evidence?
secondary_questions:
  - What are the strongest positive indicators for each company?
  - What are the major risks or red flags?
  - How do the companies compare on stability, growth, and uncertainty?
likely_domain: finance
domain_confidence: 0.89
target_entities:
  - company
  - reporting period
  - business segment
decision_criteria_candidates:
  - financial performance
  - growth trajectory
  - risk exposure
  - management credibility
  - competitive position
required_evidence_types:
  - metrics
  - trends
  - risk disclosures
  - strategic claims
  - comparative indicators
constraints: []
ambiguities:
  - investment horizon not specified
  - user risk tolerance not specified
success_definition:
  - provide recommendation for each company
  - justify recommendation with evidence
  - assign confidence score
  - surface uncertainties and assumptions
```

---

## Handoff to Other Agents

- Sends structured goal interpretation to `planner`.
- Provides `likely_domain` and `decision_criteria_candidates` to `analysis_agent`.
- Provides `success_definition` and `ambiguities` to `report_generator` and `decision_agent`.

---

## Failure and Recovery Behavior

If domain detection is weak or multiple domains are plausible:
- set `likely_domain` to the best candidate,
- reduce `domain_confidence`,
- include alternative domains in `ambiguities`,
- request that the planner schedule an early evidence-based domain validation step.

---

## Implementation Notes

This agent works best when its output is stored as a structured object rather than free text. The object becomes the controlling contract for downstream planning and evaluation.