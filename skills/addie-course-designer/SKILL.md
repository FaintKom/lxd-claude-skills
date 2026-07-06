---
# AGENT SKILLS STANDARD FIELDS (v2)
name: addie-course-designer
description: "Design a complete training course using the full ADDIE framework (Analyze, Design, Develop, Implement, Evaluate). Use when creating a new course from scratch for adults (career change, reskilling, corporate training) or for children learning new skills, and you need a systematic phase-by-phase plan with deliverables for each stage."
disable-model-invocation: false
user-invocable: true
effort: high

# EXISTING FIELDS

skill_id: "instructional-design/addie-course-designer"
skill_name: "ADDIE Course Designer"
domain: "instructional-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Branson et al. (1975) — Interservice Procedures for Instructional Systems Development (IPISD): the original ADDIE model developed at Florida State University for U.S. Army"
  - "Molenda (2003) — In search of the elusive ADDIE model: historical overview"
  - "Dick, Carey & Carey (2014) — The Systematic Design of Instruction (8th ed.): systems approach aligned with ADDIE"
  - "Branch (2009) — Instructional Design: The ADDIE Approach"
  - "Allen (2012) — Leaving ADDIE for SAM: critique and contextual use of ADDIE"
input_schema:
  required:
    - field: "course_topic"
      type: "string"
      description: "What the course teaches (subject, skill, or competency)"
    - field: "target_audience"
      type: "string"
      description: "Who will take the course (adults reskilling, children learning new skill, age/level/context)"
    - field: "business_or_learning_goal"
      type: "string"
      description: "The observable outcome or change the course must produce"
    - field: "course_duration"
      type: "string"
      description: "Total available time (hours, weeks, self-paced length)"
  optional:
    - field: "delivery_format"
      type: "string"
      description: "Live online / in-person / self-paced / blended"
    - field: "constraints"
      type: "string"
      description: "Budget, tools, SMEs available, deadlines, platform limitations"
    - field: "prior_knowledge"
      type: "string"
      description: "What learners already know coming in"
output_schema:
  type: "object"
  fields:
    - field: "phase_1_analyze"
      type: "object"
      description: "Needs analysis, learner analysis, context analysis, task analysis"
    - field: "phase_2_design"
      type: "object"
      description: "Learning objectives, assessment strategy, course blueprint, sequence"
    - field: "phase_3_develop"
      type: "object"
      description: "Content creation plan, materials checklist, prototype schedule"
    - field: "phase_4_implement"
      type: "object"
      description: "Rollout plan, facilitator prep, learner onboarding, logistics"
    - field: "phase_5_evaluate"
      type: "object"
      description: "Formative + summative evaluation plan tied to the stated goal"
chains_well_with:
  - "learner-persona-builder"
  - "training-needs-analyzer"
  - "kirkpatrick-evaluation-planner"
  - "action-mapping-designer"
  - "backwards-design-unit-planner"
  - "scope-and-sequence-designer"
  - "gagne-nine-events-lesson-designer"
teacher_time: "10 minutes"
tags: ["ADDIE", "instructional-design", "course-design", "curriculum", "systematic-design"]
---

# ADDIE Course Designer

## What This Skill Does

Generates a complete ADDIE-based course plan with concrete deliverables for each of the five phases: Analyze (needs, learners, context, tasks), Design (objectives, assessments, blueprint, sequence), Develop (content plan, materials list, prototype milestones), Implement (rollout, facilitator prep, logistics), and Evaluate (formative and summative measures tied to the goal). ADDIE is the most widely used systematic instructional design framework and provides the canonical checklist for course creation. AI is valuable here because holding all five phases in mind simultaneously — and ensuring they align — is cognitively demanding, and most course creators skip phases (jumping straight from topic to content) which produces courses that are disconnected from actual learner needs and measurable outcomes.

## Evidence Foundation

ADDIE originated in 1975 at Florida State University's Center for Educational Technology under a contract with the U.S. Army (Branson et al., 1975) as the Interservice Procedures for Instructional Systems Development (IPISD). Molenda (2003) traces how the label "ADDIE" emerged later as a colloquial name for the systems approach documented in IPISD, and argues there is no single authoritative ADDIE model — rather, a family of systematic design processes. Dick, Carey & Carey (2014) provide the most rigorous systems-approach version, emphasising alignment between analysis, objectives, assessments, and instruction. Branch (2009) provides the most practitioner-friendly modern treatment, framing ADDIE as iterative rather than strictly waterfall. Allen (2012) critiques strict waterfall ADDIE as too slow for modern contexts and proposes SAM as an iterative alternative — but acknowledges ADDIE's value as a systematic checklist that ensures no phase is skipped. The evidence base for each individual phase (needs analysis, objective writing, assessment alignment, evaluation) is strong; ADDIE's contribution is the insistence that all phases be performed and aligned.

## Input Schema

The user must provide:
- **Course topic:** *e.g. "SQL fundamentals for career-changers entering data analytics"*
- **Target audience:** *e.g. "Adults 25–40, no technical background, changing careers from marketing/HR, motivated but anxious about math"*
- **Business or learning goal:** *e.g. "Graduates can write intermediate SQL queries to pass a junior data analyst technical interview within 3 months of finishing the course"*
- **Course duration:** *e.g. "8 weeks, ~6 hours/week self-paced + weekly live session"*

Optional:
- **Delivery format**
- **Constraints**
- **Prior knowledge**

## Prompt

```
You are an expert instructional designer with deep knowledge of the ADDIE framework (Branson et al., 1975; Dick, Carey & Carey, 2014; Branch, 2009). You design courses systematically, working through all five phases and ensuring alignment between them. You do not jump straight to content — you always ground course design in learner analysis and measurable outcomes.

Your task is to design a course for:

**Course topic:** {{course_topic}}
**Target audience:** {{target_audience}}
**Business or learning goal:** {{business_or_learning_goal}}
**Course duration:** {{course_duration}}

Optional context (use if provided, ignore if marked "not provided"):
**Delivery format:** {{delivery_format}}
**Constraints:** {{constraints}}
**Prior knowledge:** {{prior_knowledge}}

Apply ADDIE rigorously. For each phase, produce concrete deliverables, not generic descriptions.

**Phase 1 — Analyze**
- **Needs analysis:** What performance gap does this course close? What is the current state vs. the desired state? Is training the right solution or would a job aid, process change, or tool do better? (If training is not the right solution, flag it.)
- **Learner analysis:** Demographics, prior knowledge, motivation, barriers, learning preferences, technical access. For adult learners changing careers: anxieties, time constraints, identity concerns. For children learning new skills: developmental readiness, attention span, prior exposure.
- **Context analysis:** Where and when will learning happen? What is the environment? What technology is available? What are the social/cultural factors?
- **Task analysis:** Break the target performance into component knowledge, skills, and attitudes. What must learners be able to DO by the end? List observable behaviours.

**Phase 2 — Design**
- **Terminal learning objectives:** 3–6 observable, measurable objectives using action verbs (Bloom's revised taxonomy). Format: "By the end of the course, learners will be able to [verb] [object] [condition] [criterion]."
- **Assessment strategy:** How will you measure that each objective is met? Align every objective to at least one assessment. Include both formative (during) and summative (end) assessments.
- **Course blueprint:** A table mapping modules/units to objectives, assessments, and estimated time. Ensure every objective is addressed.
- **Sequencing rationale:** Why this order? Justify the sequence using learning principles (prerequisites, cognitive load, spacing, simple-to-complex).
- **Alignment check:** Verify every objective → assessment → content → activity chain.

**Phase 3 — Develop**
- **Content creation plan:** For each module, list the artifacts to produce (videos, readings, exercises, slides, quizzes). Note owner and effort estimate.
- **Materials checklist:** What needs to exist before Phase 4 can begin?
- **Prototype and pilot:** Recommend one module to prototype first and test with 2–3 representative learners before building the rest.
- **Quality checks:** What review steps (SME review, instructional review, pilot) are required?

**Phase 4 — Implement**
- **Rollout plan:** Onboarding sequence, cohort vs self-paced logistics, pacing expectations, support channels.
- **Facilitator preparation:** If live or blended, what does the instructor need? Run-of-show, talking points, FAQ, escalation paths.
- **Learner onboarding:** First 15 minutes of the learner experience — how do they know what to do, where to go, how to ask for help?
- **Risk mitigation:** What could go wrong? (Dropout, technical issues, pacing, disengagement.) Plan for each.

**Phase 5 — Evaluate**
- **Formative evaluation (during development and pilot):** How will you know the course works before full rollout? (Pilot feedback, observation, iteration.)
- **Summative evaluation (post-delivery):** How will you measure success against the stated goal? Map to Kirkpatrick's 4 levels if appropriate: Reaction (satisfaction), Learning (knowledge/skill gain), Behavior (transfer to real context), Results (business/life outcome). For a career-change course: did graduates get jobs? For children: did they acquire and use the skill outside the course?
- **Iteration plan:** After the first cohort, what data will you collect? What will trigger revisions?

Return output in this structure:

## Course Design: [Course Title]

**Audience:** [from input]
**Goal:** [from input]
**Duration:** [from input]

### Phase 1 — Analyze
[needs / learner / context / task analysis as subsections]

### Phase 2 — Design
[objectives / assessment strategy / blueprint table / sequencing rationale / alignment check]

### Phase 3 — Develop
[content plan / materials checklist / prototype recommendation / quality checks]

### Phase 4 — Implement
[rollout / facilitator prep / onboarding / risk mitigation]

### Phase 5 — Evaluate
[formative / summative (Kirkpatrick-mapped) / iteration plan]

### Alignment Audit
[Single table: Objective → Assessment → Module → Activities. Flag any gaps.]

**Self-check before returning:** Verify (a) every terminal objective is measurable and observable, (b) every objective is assessed and taught, (c) the evaluation plan measures the stated goal (not just satisfaction), (d) Phase 1 actually informed Phase 2 (you did not skip analysis), and (e) the course is feasible within the stated duration.
```

## Example Output (abridged)

**Scenario:** *Topic: "SQL fundamentals for career changers" / Audience: "Adults 25–40, no tech background, switching from marketing/HR" / Goal: "Pass junior data analyst SQL interview within 3 months" / Duration: "8 weeks, 6h/week"*

### Phase 1 — Analyze (summary)

- **Needs:** Gap = cannot write intermediate SQL under interview conditions. Training IS the right solution (skill gap, not motivation or process). A job aid will not suffice because interviews test recall under pressure.
- **Learner:** High motivation (career change), high anxiety (math/tech imposter syndrome), limited time (evenings/weekends), needs early confidence wins, needs explicit connection to career outcomes, learns best with concrete examples from familiar business domains (marketing funnels, HR data) before abstract CS examples.
- **Context:** Self-paced at home + weekly live cohort session. Laptop + free SQL sandbox (e.g., SQLite, DB Fiddle). Must accommodate full-time jobs.
- **Task analysis (behaviours):** Read a schema / write SELECT with WHERE, ORDER BY, LIMIT / aggregate with GROUP BY, HAVING / join two tables (INNER, LEFT) / use subqueries and CTEs / explain a query aloud / debug a broken query / translate a business question into SQL.

### Phase 2 — Design (summary)

**Objectives:**
1. Given a schema diagram and a business question, write a correct SELECT query using WHERE, ORDER BY, and LIMIT within 5 minutes.
2. Given two related tables, write a query joining them using the correct join type (INNER/LEFT) and explain why.
3. Given messy aggregated data requirements, write GROUP BY + HAVING queries answering business questions.
4. Given a broken query, identify the error and fix it within 3 attempts.
5. Given a plain-English business question typical of junior analyst interviews, translate it into working SQL within 10 minutes.

**Blueprint:**

| Week | Module | Objectives | Assessment | Time |
|---|---|---|---|---|
| 1 | SELECT basics | 1 | 10-query exercise set | 6h |
| 2 | Filtering & sorting | 1 | Business question → query | 6h |
| 3 | Aggregation | 3 | Marketing funnel analysis | 6h |
| 4 | JOINs part 1 | 2 | HR dataset joining task | 6h |
| 5 | JOINs part 2 + subqueries | 2, 3 | Multi-table analysis | 6h |
| 6 | Debugging | 4 | Broken query lab | 6h |
| 7 | Interview translation | 5 | Timed mock questions | 6h |
| 8 | Mock interview + feedback | 1–5 | Recorded mock interview | 6h |

**Sequencing rationale:** Early wins in Week 1 (confidence); spaced retrieval of SELECT throughout; JOINs split across two weeks because it's the highest-failure topic; debugging deliberately late (requires prior fluency); mock interview as summative performance task.

### Phase 3 — Develop (summary)

- **Artifacts per week:** ~20-min video lecture, worked examples notebook, practice exercises with auto-grading, live session run-of-show, Slack support.
- **Prototype:** Build Week 3 (Aggregation) first — it's mid-difficulty and lets you test both the content style and the auto-grading pipeline. Pilot with 3 marketers.
- **Quality checks:** SME review by working data analyst, instructional review for clarity, pilot feedback loop.

### Phase 4 — Implement (summary)

- **Rollout:** Cohort-based (Monday start), weekly live session Wednesday evening, async practice rest of week.
- **Onboarding:** Day 1 = 15-min welcome video + environment setup + "first query" win within 10 minutes.
- **Risks:** Dropout around Week 4–5 (JOINs difficulty) — mitigation: extra support session, celebration of Week 3 completion, peer study groups.

### Phase 5 — Evaluate (summary)

- **Formative:** Weekly completion rates, exercise error patterns, live-session question log.
- **Summative (Kirkpatrick):**
  - Level 1 (Reaction): post-course NPS + qualitative survey
  - Level 2 (Learning): Week 8 mock interview performance (pass rate target: 80%)
  - Level 3 (Behavior): at 30/60/90 days post-course, how many graduates are applying SQL in real interviews or jobs?
  - Level 4 (Results): at 90/180 days, how many secured a data analyst role? (Tied to stated goal.)
- **Iteration:** After Cohort 1, review dropout points, error patterns, and Level 3/4 data. Revise modules with highest error rates.

### Alignment Audit

| Objective | Assessment | Module | Activities |
|---|---|---|---|
| 1 | Weeks 1,2 exercises + mock | 1, 2 | Worked examples, practice ✓ |
| 2 | Weeks 4,5 tasks + mock | 4, 5 | Video, joining lab ✓ |
| 3 | Week 3 task + mock | 3 | Funnel analysis lab ✓ |
| 4 | Week 6 lab + mock | 6 | Debugging exercises ✓ |
| 5 | Week 7,8 + mock | 7, 8 | Timed questions, mock interviews ✓ |

No gaps detected.

---

## Known Limitations

1. **ADDIE in its classic waterfall form is slow.** For rapidly changing content or when the topic is novel to the designer, use SAM iterative model (`sam-iterative-course-prototyper`) instead or in combination — build a prototype early, iterate, then apply ADDIE's evaluation rigour at the end.

2. **The analyze phase is only as good as the data available.** If you cannot interview real target learners, flag your learner analysis as a set of hypotheses to validate during the pilot rather than treating it as ground truth.

3. **Kirkpatrick Level 3 and Level 4 evaluation is hard and often skipped.** If you do not plan these measures before launch, you will almost certainly never collect them. Define data collection mechanisms as part of Phase 5, not after rollout.

4. **For short courses or micro-learning, full ADDIE is overkill.** For a 30-minute course, collapse phases into a one-page plan. ADDIE's value scales with course complexity and stakes.
