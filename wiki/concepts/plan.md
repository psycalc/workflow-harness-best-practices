---
title: Plan
type: concept
tags: [product, technical, roadmap]
created: 2026-04-14
updated: 2026-04-24
sources: [autoreasearch.md, wiki/concepts/compatibility-level-boundaries.md, wiki/concepts/sociological-compatibility-analogues.md, wiki/concepts/neuroscience-compatibility-bridges.md, wiki/concepts/typology-code-conventions.md, wiki/concepts/weight-calibration.md]
---

# Cognitive Matchmaker: Product and Technical Plan

Below is the plan as it should be built in a real product, not as a beautiful idea in a prompt.

First, the key point: this project cannot be started with the thought "let's make a smart matchmaker on LLM and it will understand everything on its own." The correct path is different: first, a strict multi-agent pipeline is built, then a limited MVP with a human in the loop, then verification of whether the simulation predicts real compatibility, and only after that—partial autonomy.

I would build this as a system of 4 layers:

1. Orchestration layer — who runs the pipeline and monitors state
2. Cognition layer — agents that interview, model, simulate, evaluate
3. Evidence layer — where facts, hypotheses, transcripts, risks, and explanations are stored
4. Action layer — what the user sees: shortlist, explanation, date questions, logistics

Below is the detailed plan.

## 0. Methodology Update — 2026-04-24

This plan remains the product/technical roadmap, but the project methodology has become stricter since the first draft.

Current governing pages:

- [[compatibility-level-boundaries]] — normative boundary between strategic, operational, and tactical compatibility
- [[sociological-compatibility-analogues]] — sociology as external context and scenario layer
- [[neuroscience-compatibility-bridges]] — neuroscience as process-level bridge and validation-task inspiration
- [[typology-code-conventions]] — canonical and localized type-code rules
- [[weight-calibration]] — scoring weights and aggregation remain open research questions

### Current compatibility ontology

| Level | Primary typology | What it owns | Short question |
|-------|------------------|--------------|----------------|
| **Strategic** | Temporistics | temporal worldview, life trajectory, meaning horizon | same road? |
| **Operational** | Psychosophy | action priorities, pressure, role distribution, execution | can we run life together? |
| **Tactical** | Socionics | information exchange, situation reading, communication repair | do our interfaces fit? |

Typologies are **not oracles**, but they are now treated as **primary lenses at their own compatibility level**. Cross-system similarities are annotations, not translations.

### Secondary evidence layers

Sociology and neuroscience are not additional typologies.

| Layer | Use | Not allowed |
|-------|-----|-------------|
| **Sociology** | social context, moderators, scenario realism, anti-misattribution | treating class/gender/institution as type |
| **Neuroscience** | cognitive-affective mechanism analogies, stress/fatigue caveats, validation-task design | brain-region/type mapping or neuro-babble |

### Data/code policy

Store typology data as canonical structured values and render display codes by language. Do not rely on ambiguous display strings alone. See [[typology-code-conventions]].

### Unofficial grouping policy

Unofficial groupings, such as Psychosophy sextas/quadras, may support explanation and broad role-fit hints. They should not be treated as original Afanasyev doctrine or as primary scoring inputs unless separately validated.

---

## 1. Project Goal

### What the product should do:

- Not show the user 300 profiles, but deliver 1–5 strong candidates per week with a clear explanation of why exactly they were selected
- Evaluate not only interests and bio, but how the pair potentially behaves in stressful and significant scenarios
- Give not a "magic verdict," but a hypothesis with an evidence packet: what the model saw, where the risks are, what to verify in real life
- Reduce cognitive load and swipe fatigue

### What the product should NOT do:

- Should not make hard exclusions based solely on typologies
- Should not present simulation as truth about a person
- Should not immediately write to people, negotiate, and make decisions without transparent consent
- Should not be built on opaque scraping without consent

---

## 2. Basic Architecture Principle

We need not one "super-agent," but an orchestra of specialized agents with clear contracts.

The correct scheme is:

```
User / Candidate Data
→ Intake Agents
→ Profiling Agents
→ Twin Builder
→ Scenario Planner
→ Simulation Agents
→ Observer / Judge Ensemble
→ Risk & Compliance Agent
→ Shortlist Composer
→ Human Review / Optional Concierge Actions
→ Feedback Loop / Retraining
```

Key idea: each agent is responsible for only one stage. No "one LLM does everything itself."

---

## 3. Agent Set

I would create 12 main agents.

### 3.1 Orchestrator Agent

This is the main workflow manager.

**What it does:**
- Accepts new onboarding, new candidate, weekly run, feedback event
- Runs the correct sub-agents
- Monitors statuses, retries, profile versioning
- Stops the pipeline if confidence is low or there are red flags

**Input:** event type + entity id

**Output:** state transition + list of tasks for other agents

This is essentially the control plane of the entire system.

---

### 3.2 Onboarding Interview Agent

This is the deep interview agent with the user.

**Task:**
Not just collecting a form, but extracting real relationship-relevant patterns:
- Values
- Boundaries
- Fears
- Typical conflicts
- Communication style
- Expectations from marriage/relationships
- Attitude toward money, children, career, relocation, faith, family

It should work on an adaptive interview tree:
- If it sees an important signal — goes deeper
- If the answer is superficial — reformulates
- If an area is critical — asks for an example from the past

**Output** is not text "user is such-and-such," but a structured object: `UserEvidenceProfile v1`

Example fields:
- identity_constraints
- relationship_goals
- non_negotiables
- conflict_patterns
- attachment_signals
- decision_style
- stress_response
- faith_values
- family_model
- time_horizon
- money_norms
- relocation_attitude
- child_preferences
- communication_style
- self_reported_typology
- confidence_per_field

---

### 3.3 Evidence Normalizer Agent

This agent converts raw responses into normalized facts and hypotheses.

**Why it is needed:**
- People respond lengthily, contradictorily, emotionally
- LLMs like to make overly confident conclusions
- Need to separate "fact," "self-description," "hypothesis," "unknown"

It splits data into 4 layers:
- observed facts
- self-claims
- model inferences
- unknowns

This is critical for safety and explainability.

---

### 3.4 Typology Mapping Agent

This agent separately works with temporistics, psychosophy, and socionics.

But very importantly: it should not be a source of truth.
It should output not a type as dogma, but a distribution of hypotheses.

For example:
```
socionics_hypotheses = [typeA: 0.41, typeB: 0.27, typeC: 0.13...]
psychosophy_hypotheses = [...]
temporistics_hypotheses = [...]
```

And alongside:
- which statements support this
- which statements contradict this
- confidence
- what should be validated in reality

This is how typologies become feature generators, not an "oracle."

---

### 3.5 Candidate Intake Agent

This agent accepts candidates from any source:
- Profile in the app
- Invited pool
- Manual import
- Partner matchmaking pool
- Human curator input

It should:
- Normalize profiles
- Check for consent
- Remove garbage
- Mark missing fields
- Create `CandidateEvidenceProfile`

For MVP, I would not touch mass dating apps at all.
Better to start with a closed consent-based pool, otherwise you'll drown in ethics, platforms, and bad data.

---

### 3.6 Twin Builder Agent

This is one of the main agents.

**Its task:** From the evidence profile, build a controllable digital twin.

Twin is not just a prompt.
It is a structured simulation policy.

It builds:
- core motives
- decision heuristics
- emotional triggers
- style constraints
- red lines
- preferred repair strategies
- uncertainty zones

The twin must be able to respond not "beautifully," but consistently.

**Output:**
- `UserTwinSpec`
- `CandidateTwinSpec`

Important: twin spec must be machine-readable, not just narrative text.

Example:
```json
{
  "conflict_style": {"mode": "withdraw_then_return", "confidence": 0.72},
  "money_decision_style": {"mode": "conservative_budgeting", "confidence": 0.81},
  "relocation_priority": {"mode": "relationship_over_career", "confidence": 0.49},
  "strategic_values": [...]
}
```

---

### 3.7 Scenario Planner Agent

This agent selects which specific scenarios to run for the pair.

Don't need to run 50 identical dialogues.
Need sparse high-information scenarios.

It should select scenarios by matrix:

**Stakes:**
- low / medium / high

**Domains:**
- money
- career
- faith
- family
- children
- location
- sexual boundaries
- social life
- caregiving
- failure
- illness
- jealousy
- time allocation
- conflict recovery

**Axes:**
- ambiguity
- time pressure
- value clash
- role asymmetry
- external stress

For a pair, not 3 random scenarios, but a scenario pack: `ScenarioPack v1`

Example:
1. Career relocation under time pressure
2. Family budget after unexpected loss
3. Conflict about weekly routine / church / relatives
4. Response to partner burnout
5. Long-term vision alignment

---

### 3.8 Simulation Director Agent

This agent runs simulations and ensures they are high quality.

**Its work:**
- Do multiple runs
- Change seeds
- Try role reversal
- Increase stress
- Run counterfactual branches
- Stop explicitly invalid simulations
- Mark unstable outputs

One simulation = garbage.
Need an ensemble.

I would do at least:
- 5–10 scenarios per pair
- 3–5 runs per scenario
- Total: 15–50 transcripts per pair

Because one LLM dialogue can easily drift into randomness.

---

### 3.9 Pair Simulation Agents

Technically these are two agents:
- User Twin Agent
- Candidate Twin Agent

They play not a "smart assistant," but a specific person within twin spec constraints.

**Rules:**
- Not have access to the final score
- Not know the desired outcome
- Not specifically adapt for the sake of compatibility
- Follow internal policy, not pleasing mode

Otherwise, everything turns into synthetic politeness.

---

### 3.10 Observer / Judge Ensemble Agent

This is the most important analytical layer.

I would not make one judge.
Need an ensemble of 3–4 evaluative roles:

1. **Strategic Judge** — Looks at values, motives, life vector

2. **Operational Judge** — Looks at role distribution, joint problem solving, complementarity

3. **Tactical Judge** — Looks at micro-dynamics of communication:
   - escalation
   - repair attempts
   - listening
   - interrupting
   - defensiveness
   - withdrawal
   - co-regulation

4. **Skeptic / Red-Team Judge** — Tries to prove the match is bad and looks for weaknesses in the model

This is critical because without a red-team judge, the system will hallucinate compatibility.

**Output:**
- `CompatibilityAssessment`
- `RiskFlags`
- `EvidenceMoments`
- `ConfidenceScore`
- `Contradictions`

---

### 3.11 Explanation Composer Agent

This agent converts the technical output into human-readable form.

It does:
- Match card
- Plain-language explanation
- Why this candidate
- Why not others
- What to validate offline
- Questions for first date
- What could go wrong

Example of a good explanation:
"In scenarios about money and stress, you both did not fall into contempt or silent withdrawal. You have different decision-making styles: one closes uncertainty faster, the other explores options longer. But in 4 out of 5 runs, you found a working compromise. Main risk — different urgency on the relocation topic."

This is the evidence packet.

---

### 3.12 Feedback & Learning Agent

After real contact or date, it collects reality.

**What to collect:**
- Whether there was mutual interest
- Whether there was a feeling "the model missed"
- Whether conflict patterns matched
- Whether there was desire to continue
- What turned out to be critical but the model didn't notice

Without this, the project does not exist.
Otherwise, it's just a beautiful simulation without ground truth.

---

## 4. Entities and Artifacts

Strict data model is needed.

### Core Entities

**Profile & Evidence:**
- `RawProfileItems` — Raw input data from user/candidate intake
- `UserEvidenceProfile` / `CandidateEvidenceProfile` — Incremental evidence collected during onboarding
- `EvidenceProfileNormalized` — Normalized facts, self-claims, hypotheses, unknowns
- `EvidenceClaim` — Individual claim with evidence span and confidence

**Typology:**
- `TypologyHypotheses` — Probability distributions over types (not single labels)
- `ConsentPolicyVersions` — Versioned consent policies

**Twin:**
- `TwinSpec` — Structured simulation policy for a person (machine-readable JSON)

**Scenario:**
- `ScenarioCatalog` — Repository of available scenario templates
- `ScenarioPack` — Selected scenarios for a specific pair
- `ScenarioInstance` — Single instantiated scenario for simulation

**Simulation:**
- `SimulationBatch` — Collection of runs for one pair
- `SimulationRun` — Single simulation execution
- `TranscriptStore` / `Transcript` — Simulation transcript records (JSONL)

**Assessment:**
- `JudgeAssessment` — Output from Judge Ensemble
- `RiskFlag` — Individual risk indicator
- `RubricVersion` — Version of scoring rubric
- `CompatibilityAssessment` — Final structured assessment

**Output & Feedback:**
- `MatchCard` — Human-readable match summary with evidence
- `ShortlistDigest` — Weekly digest for user
- `FeedbackEvent` — Real-world outcome feedback
- `FeedbackLabel` — Processed feedback labels

**Governance & Compliance:**
- `ConsentRecord` — Consent for data processing
- `PolicyConstraints` — Current policy rules
- `AuditLog` — Immutable audit trail

**System:**
- `ModelVersion` — Version of prompts, schemas, policies
- `UserProfileIndex` / `CandidateIndex` — Searchable profile indices
- `UserSessionTranscript` — Current session interview transcript
- `WorkflowState` — Pipeline execution state

This is important because without structured entities, you cannot:
- Do explainability
- Recalculate scores
- Version profiles
- Build offline evaluation
- Conduct A/B

---

## 5. End-to-End Pipeline

**Step 1. User Onboarding**
User goes through interview.
System collects evidence, not just a form.

**Step 2. Normalization**
Responses are cleaned, turned into facts, hypotheses, unknowns.

**Step 3. Level-Specific Typology Extraction**
Typologies are used as primary lenses at their own compatibility levels, but not as total truth:

- Temporistics → strategic evidence
- Psychosophy → operational evidence
- Socionics → tactical evidence

Each output must include support, contradiction, uncertainty, and confidence. Secondary systems may annotate or flag risks, but must not directly translate constructs across systems.

**Step 3a. Social Context Profiling**
Sociological variables are collected as external moderators: class, education, labor-market constraints, household labor, gender scripts, migration/war context, family networks, religion/institutions, and local dating-market constraints. These should prevent structural constraints from being misread as personality or type defects.

**Step 3b. Cognitive-Affective Moderator Profiling**
Neuroscience-informed variables are collected only behaviorally: stress, fatigue, time pressure, ambiguity, emotional escalation, repair capacity, perspective-taking demand, and cognitive load. No brain-type mapping is allowed.

**Step 4. User Twin Build**
User's digital twin version is assembled.

**Step 5. Candidate Intake**
Candidates are imported, go through the same normalization.

**Step 6. Candidate Twin Build**
Twin is built for each candidate.

**Step 7. Pair Pre-filtering**
Quick filter by hard constraints:
- age range
- geography
- relationship status
- children / marriage / faith / non-negotiables
- consent
- Critical incompatibilities

**Step 8. Scenario Planning**
For each pair, an individual pack of high-information scenarios is selected.

**Step 9. Multi-run Simulation**
Pair is run through many scenarios and repeats.

**Step 10. Judge Ensemble Scoring**
Judges output layer-by-layer assessment and risks. The ensemble should include:

- strategic judge — Temporistics primary
- operational judge — Psychosophy primary
- tactical judge — Socionics primary
- social context / structural attribution judge
- cognitive-affective moderator judge
- skeptic / safety judge

**Step 11. Explanation Synthesis**
Human-readable explanation is assembled.

**Step 12. Weekly Ranking**
User receives shortlist of 1–5 candidates.

**Step 13. Real-world Validation**
After communication / date, feedback is collected.

**Step 14. Recalibration**
Model, weights, scenarios, twin-building are improved on real outcome data.

---

## 6. How to Calculate Score

I would not make one score "87/100".
That's too fake.

Important: the aggregation formula is **provisional**. The current project position is that level scoring should follow [[compatibility-level-boundaries]], while final weighting and aggregation must be empirically calibrated through [[weight-calibration]].

Need at least 5 numbers:

1. strategic alignment
2. operational complementarity
3. tactical interaction stability
4. risk severity
5. confidence / evidence sufficiency

**Current staged logic should be:**

```
strategic_alignment       = score from Temporistics
operational_fit           = score from Psychosophy
tactical_interaction_fit  = score from Socionics

secondary_context = {
  sociology_moderators,
  cognitive_affective_moderators,
  contradiction_flags,
  evidence_confidence
}

final_match_score = calibrated_aggregation(
  strategic_alignment,
  operational_fit,
  tactical_interaction_fit,
  reciprocal_interest_prior,
  risk_penalty,
  secondary_context
)
```

Do **not** hard-code the weights as a final doctrine until real-world outcome calibration exists. Earlier weights like `0.40 / 0.30 / 0.20 / 0.10` are placeholders, not project law.

But separately required:
- confidence_score
- hard_stop_flags
- contradiction_flags between levels
- structural_context_flags
- stress/fatigue/moderator flags

**Example hard stop:**
- Contradiction on children
- Incompatibility on faith
- Critical divergence in marriage expectations
- Dangerous signs of control / aggression / manipulation

High "chemistry" should not override hard incompatibility.

---

## 7. How to Make Simulations Non-Toy

This is where things most often break.

To make simulation not be beautiful chatter, these rules are needed:

1. **Structured world state** — Scenario must have world state: income, deadline, constraints, relatives, health, time, solution options.

2. **Goal tension** — Each twin in scenario must have real competing priorities.

3. **Memory continuity** — If there was a conflict at the beginning, its consequences must remain in subsequent turns.

4. **Stress modulation** — Ability to intensify stress and see how dynamics change.

5. **Multi-run variance tracking** — If outcome fluctuates too much, confidence must drop.

6. **Counterfactual probes** — Need to run "what if conditions are worse," "what if one has fewer resources."

7. **Anti-pleasing constraints** — Agents should not artificially smooth conflict for a beautiful result.

8. **Social realism constraints** — Scenarios should include structural context when relevant: class/status mismatch, housing, shift work, migration, military service, family obligations, care load, gender-role expectations, income volatility, or institutional pressure. See [[sociological-compatibility-analogues]].

9. **Cognitive-affective moderators** — Scenarios should vary fatigue, sleep debt, time pressure, ambiguity, perceived criticism, emotional escalation, repair opportunity, and perspective-taking demand. See [[neuroscience-compatibility-bridges]].

10. **Observable markers only** — Judges should score observable repair, escalation, withdrawal/re-entry, turn-taking, perspective-taking accuracy, and decision ownership. They should not infer hidden neural causes.

---

## 8. What MVP to Build First

Correct MVP is very narrow.

Don't do immediately:
- Full autonomy
- Calendar booking
- Venue search
- Outreach
- Mass pool
- Auto-writing to candidates

**First MVP should be:**

```
MVP-1:
- Deep onboarding
- SocialContextProfile v1
- Candidate intake from curated consent-based pool
- Twin builder
- 5–7 scenarios
- at least 2 sociology-informed scenarios
- stress/fatigue/repair moderators in scenarios
- Judge ensemble
- structural context review
- Manual human review
- 1–3 recommendations per week
- Feedback collection after date
```

**MVP goal:**
Not "make people say wow," but verify 3 things:

1. Is such explanation actually useful to the user?
2. Is shortlist really better than random or ordinary choice?
3. Do simulation outputs correlate with reality at all?

MVP should explicitly separate explanation labels:

- personal preference
- interaction pattern
- typology-level hypothesis
- structural constraint
- stress/fatigue state effect
- unknown / needs real-world validation

---

## 9. Phased Plan

### Phase 0. Research & Problem Framing
**Duration:** 2–4 weeks

**What to do:**
- Fix product hypothesis
- Describe outcomes
- Define sensitive domains
- Fix legal boundaries
- Make data contracts
- Define compatibility boundary rules: Strategic/Temporistics, Operational/Psychosophy, Tactical/Socionics
- Define which secondary layers are allowed only as context: sociology and neuroscience
- Define typology code and localization policy
- Mark scoring aggregation as provisional until calibration

**Artifacts:**
- PRD
- Risk register
- Ontology of compatibility
- Typology boundary policy
- Social context profile schema
- Cognitive-affective moderator schema
- Code/terminology convention
- Scenario taxonomy
- Data schema draft
- Evaluation plan v1

---

### Phase 1. Human-in-the-Loop Concierge Prototype
**Duration:** 4–8 weeks

**What to do:**
- Onboarding agent
- Candidate intake
- Manual or semi-automatic twin building
- 5 basic scenarios
- Judge ensemble
- Manual shortlist assembly

**Goal:** Understand which explanations are useful and where the model talks nonsense.

---

### Phase 2. Simulation Platform MVP
**Duration:** 6–10 weeks

**What to do:**
- Scenario compiler
- Simulation runner
- Logging of transcripts
- Variance tracking
- Confidence computation
- Explanation composer

**Goal:** Get a stable simulation pipeline.

---

### Phase 3. Evaluation Loop
**Duration:** 8–12 weeks

**What to do:**
- Real feedback collection
- Post-date surveys
- Compare predicted vs actual
- Ablation tests:
  - without typologies
  - only hard constraints
  - with typologies
  - with simulation
  - without simulation

**Goal:** Prove that at least some part of the system actually gives signal.

---

### Phase 4. Partial Automation
**Duration:** 6–8 weeks

**What to do:**
- Orchestrator
- Automatic weekly digest
- Best scenarios by segments
- Basic risk gate
- Versioned twin specs

---

### Phase 5. Concierge Action Layer
**Duration:** 4–6 weeks

**What to do:**
- Questions for first date
- Scheduling assist
- Venue suggestions
- Consent workflows

---

### Phase 6. Scaling & Personalization
**Duration:** ongoing

**What to do:**
- Segment-specific models
- Personalized scenario selection
- Bandit-style scenario prioritization
- Retrieval of similar past outcomes
- Human coach escalation for edge cases

---

## 10. Tech Stack I Would Choose

I won't tie to hype, will give practical scheme.

**Backend:**
- Python + FastAPI

**Workflow orchestration:**
- Temporal or Prefect
- I would more likely take Temporal if the project really becomes event-driven and stateful

**Database:**
- PostgreSQL
- Because strict relational model, auditability, and versioning are needed

**Vector store:**
- pgvector at start
- Don't need to drag a separate vector zoo too early

**Cache / queue:**
- Redis

**LLM layer:**
- Abstraction over multiple models
- Can't tie the entire product to one model

**Observability:**
- OpenTelemetry
- Structured logs
- Trace per workflow
- Prompt/version tracing

**Evaluation storage:**
- Separate tables for runs, judges, outcomes, confidence drift

**Policy / guardrails:**
- Separate rule engine, not inside the prompt

**Frontend:**
- At the start, web dashboard for operator + user digest UI

---

## 11. How Agent Memory Should Look

Cannot just give all agents the entire history.
Need multi-layered memory.

1. **Raw memory** — Raw interviews, forms, notes

2. **Structured memory** — Facts, features, hypotheses

3. **Working memory** — What the agent needs for a specific task

4. **Audit memory** — Which model version, which prompt, which inputs, which output

Very important:
Agents should see only the necessary minimum.
Otherwise there will be leakage, bias, and unnecessary hallucination coupling.

---

## 12. What Checks Each Agent Must Have

Each output must have mandatory fields:

- claim
- evidence span
- confidence
- alternative interpretation
- missing information
- safe-to-use flag

Then the system will not just "be smart," but will really hold epistemic discipline.

---

## 13. How to Validate Scientifically, Not by Feelings

Need almost research-grade approach here.

### 13.1 Offline Validation
Take historical cases or pilot pairs and see if the system predicts:
- First date
- Second date
- Mutual interest
- Continued communication
- Model vs reality divergence

### 13.2 Online Pilot
Small group of users.
Compare:
- Ordinary search
  vs
- Concierge shortlist

### 13.3 Ablation
Check the contribution of each layer:
- Only form
- Form + hard filters
- Form + typologies
- Form + simulations
- Form + simulations + judge ensemble

### 13.4 Calibration
If the system puts 0.8 compatibility, does it really lead to positive outcome more often than 0.4?
If not — score is garbage.

### 13.5 Human Qualitative Review
Independent reviewer reads transcript and says:
Is this plausible or synthetic nonsense?

---

## 14. Main Project Risks

### 14.1 LLM twins will be plausible but not predictive
This is risk number one.

### 14.2 Typologies will give beautiful narrative but weak signal
That's why they must be optional features, not foundational truth.

### 14.3 Judges will amplify bias
That's why a skeptical judge and human review are needed.

### 14.4 User will start perceiving the system as "reading souls"
Need to very strictly formulate output as hypothesis, not diagnosis.

### 14.5 Automated exclusions will be legally and ethically dangerous
Need human override and clear consent.

### 14.6 Model will start recommending too "convenient," not best pairs
Because LLMs love social smoothness.
Need stress scenarios and red-team probing.

---

## 15. Privacy, Ethics, Compliance

This is not "we'll add later." It's in the core.

Must have immediately:

- Explicit informed consent
- Clear list of what data is used
- Right to delete profile
- Retention policy
- Don't store extra
- Audit log on automated decisions
- Explanation of how shortlist is formed
- Ban on hidden scraping without agreement
- Ban on sensitive inferences without sufficient grounds
- Human-in-the-loop for disputed cases

Especially important:
If the system excludes someone, it must be clear why.
And the reason should sound like a product reason, not "AI decided."

---

## 16. How Weekly Product Output Should Look

User should see not a mess but a compact digest.

**For each candidate:**

1. Why you got into shortlist
2. Where you have strong compatibility
3. Where you have possible risk
4. What needs to be verified in real life
5. 3–5 questions for first date
6. Overall confidence
7. What the system doesn't know yet

**Example card structure:**
- match summary
- top strengths
- top risks
- key simulation moments
- validation questions
- suggested date style
- confidence level

---

## 17. Where Human Is Needed in the Loop

At the start, almost everywhere.

**Human operator needed:**
- After onboarding — check profile coherence
- After twin build — check if the model drifted
- After scoring — check red flags and nonsense
- Before shortlist output — final sanity check
- After feedback — label model errors

Only then can a human be removed from part of steps.

---

## 18. What Team Is Needed

**Minimum:**

- 1 product lead
- 1 research / behavioral design lead
- 1 LLM engineer
- 1 backend engineer
- 1 frontend engineer
- 1 data / evaluation engineer
- 1 privacy / policy advisor (part-time)
- 1 human concierge / operator for pilot

**If doing seriously, also needed:**
- Conversation designer
- Safety evaluator
- QA for prompts and workflows
- Sociology researcher / social-science advisor
- Neuroscience or cognitive-science advisor
- Ontology / taxonomy steward for typology codes, role families, and crosswalks
- Career / labor-market taxonomy specialist if civilian and military role-fit remain in scope

The current research team should distinguish:

- typology experts — internal compatibility hypotheses
- sociology expert — social context and structural moderators
- neuroscience expert — process analogies and validation-task design
- role taxonomy experts — ESCO/O*NET/ISCO, civilian role families, and military role families

---

## 19. What I Would Do in First 30 Days

**Week 1:**
- Fix product hypothesis
- Gather compatibility ontology
- Describe data model
- Decide what data is acceptable at all

**Week 2:**
- Gather interview framework
- Gather 20–30 high-information questions
- Describe 15–20 scenarios
- Make evidence profile format

**Week 3:**
- Make twin spec schema
- Make judge rubric
- Make transcript format
- Make explanation format

**Week 4:**
- Raise prototype pipeline:
  - onboarding
  - normalization
  - twin build
  - 3 scenarios
  - judge ensemble
  - manual digest

**Goal of first month:**
Get 5–10 test cases end-to-end.

---

## 20. What I Would Definitely NOT Do

- Would not build immediately a "universal love machine"
- Would not make hard ranking solely on typologies
- Would not make fully autonomous messaging
- Would not make black-box final score without evidence
- Would not dive immediately into large dating marketplace
- Would not believe the first beautiful demo
- Would not translate constructs directly across systems, e.g. `1E = Fe` or `Future = Ni`
- Would not use sociology to label types or treat class/gender/institution as personality
- Would not use neuroscience language to imply brain-type localization
- Would not present unofficial groupings such as Psychosophy sextas/quadras as original canonical doctrine

---

## 21. Most Correct Product Formula

If completely compressed:

**Core of project = not match by profile, but match by simulated joint behavior under constraint**

But practically:

**Value only exists if simulation → shortlist quality → real-world positive outcomes**

Otherwise it's just an interesting intellectual toy.

Current broader product formula:

```text
Core engine = evidence-based simulation of joint behavior under constraint

Primary compatibility lenses:
  Strategic   -> Temporistics
  Operational -> Psychosophy
  Tactical    -> Socionics

External context layers:
  Sociology    -> structural moderators and scenario realism
  Neuroscience -> cognitive-affective mechanism analogies and validation tasks

Downstream applications:
  dating compatibility
  composite profile explanation
  civilian career role-fit
  military role-family advising
```

---

## 22. Recommended First Architecture Version

Most sane v1:

```
Orchestrator
→ Onboarding Interview Agent
→ Evidence Normalizer
→ Level-Specific Typology Mapping
  → Strategic / Temporistics
  → Operational / Psychosophy
  → Tactical / Socionics
→ Social Context Profiler
→ Cognitive-Affective Moderator Profiler
→ Twin Builder
→ Candidate Intake Agent
→ Scenario Planner
→ Simulation Director
→ Twin A / Twin B
→ Judge Ensemble
  → Strategic Judge
  → Operational Judge
  → Tactical Judge
  → Structural Context Judge
  → Cognitive-Affective Moderator Judge
  → Skeptic / Safety Judge
→ Explanation Composer
→ Human Reviewer
→ Weekly Digest
→ Feedback Agent
```

This is already serious enough to verify the idea, and not yet so crazy to drown.

---

## 23. Honest Conclusion

Idea is strong, but real value of the project is not in "typologies + agents," but in three things:

1. Good compatibility ontology
2. High-quality high-stakes scenarios
3. Strict validation against real-world outcomes

After the 2026-04-24 methodology update, the ontology must also maintain hard boundaries:

- typologies are primary only within their own compatibility level
- sociology and neuroscience are external context/validation layers, not type systems
- scoring weights are provisional until calibrated
- user-facing explanations must separate typology, social structure, state effects, and unknowns

If done weakly, you get a pseudoscientific dating oracle.
If done strongly, you might get a new class of agentic matchmaking systems.

---

Next, I can immediately deploy this in the form of:
- Technical architecture with services and API
- Roadmap for 3–6 months with sprint tasks
- Complete list of agents with prompt contracts and JSON input/output schemas
