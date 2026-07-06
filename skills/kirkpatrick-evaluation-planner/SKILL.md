---
# AGENT SKILLS STANDARD FIELDS (v2)
name: kirkpatrick-evaluation-planner
description: "Plan course evaluation using Kirkpatrick's Four Levels (Reaction, Learning, Behavior, Results) with New World Kirkpatrick Model additions. Use when you need to measure whether a course actually works — not just whether learners liked it — and you want a concrete data collection plan mapped to each level before launch."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "instructional-design/kirkpatrick-evaluation-planner"
skill_name: "Kirkpatrick Evaluation Planner"
domain: "instructional-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Kirkpatrick (1959) — Techniques for Evaluating Training Programs (original 4-level model, published as a series in the Journal of the ASTD)"
  - "Kirkpatrick & Kirkpatrick (2016) — Kirkpatrick's Four Levels of Training Evaluation (New World Kirkpatrick Model)"
  - "Phillips (2003) — Return on Investment in Training and Performance Improvement Programs (Level 5 ROI extension)"
  - "Alliger et al. (1997) — A meta-analysis of the relations among training criteria: correlations between Kirkpatrick levels are weak"
  - "Bates (2004) — A critical analysis of evaluation practice: Kirkpatrick limitations"
input_schema:
  required:
    - field: "course_description"
      type: "string"
      description: "What the course teaches and who it's for"
    - field: "business_or_life_goal"
      type: "string"
      description: "The outcome the course is supposed to produce"
    - field: "evaluation_timeframe"
      type: "string"
      description: "When you can collect data (immediate, 30d, 90d, 6m, 12m)"
  optional:
    - field: "data_access"
      type: "string"
      description: "What data you can realistically collect (employer access, follow-up, baseline data)"
    - field: "budget_constraint"
      type: "string"
      description: "How much effort can be invested in evaluation"
output_schema:
  type: "object"
  fields:
    - field: "level_1_reaction"
      type: "object"
      description: "Satisfaction, engagement, perceived relevance; method, timing, instrument"
    - field: "level_2_learning"
      type: "object"
      description: "Knowledge/skill/attitude change; pre/post design, assessment format"
    - field: "level_3_behavior"
      type: "object"
      description: "On-the-job or in-context transfer; observation/interview/self-report plan, timeframe"
    - field: "level_4_results"
      type: "object"
      description: "Business/life outcomes tied to the stated goal; metrics, baseline, attribution"
    - field: "leading_indicators"
      type: "object"
      description: "New World Kirkpatrick additions: required drivers, on-the-job supports"
    - field: "realistic_constraints"
      type: "string"
      description: "What you cannot measure and why; alternative proxies"
chains_well_with:
  - "addie-course-designer"
  - "sam-iterative-course-prototyper"
  - "action-mapping-designer"
  - "training-needs-analyzer"
teacher_time: "5 minutes"
tags: ["Kirkpatrick", "evaluation", "ROI", "training-effectiveness", "instructional-design"]
---

# Kirkpatrick Evaluation Planner

## What This Skill Does

Generates a concrete evaluation plan mapped to Kirkpatrick's four levels, plus the "required drivers" and "leading indicators" from the New World Kirkpatrick Model (Kirkpatrick & Kirkpatrick, 2016). The plan specifies exactly what data to collect, when, how, and from whom for each level, and flags what cannot be measured realistically so you don't promise data you won't collect. Most course creators measure only Level 1 (learner satisfaction), which correlates weakly with actual effectiveness — this skill pushes the design to at least plan Levels 2 and 3 before launch.

## Evidence Foundation

Donald Kirkpatrick's four-level model (1959) remains the dominant framework in training evaluation. Levels are: (1) Reaction — did learners like it; (2) Learning — did they acquire knowledge/skills; (3) Behavior — are they using it in the real context; (4) Results — did the organisation or learner achieve the intended outcome. Kirkpatrick & Kirkpatrick (2016) updated the model (New World Kirkpatrick) with emphasis on "required drivers" — on-the-job supports that determine whether Level 2 translates into Level 3 — and on starting evaluation design with Level 4 (desired results) before Level 1. Phillips (2003) added Level 5 ROI. Critically, Alliger et al. (1997) meta-analysis showed that correlations between levels are weak — learner satisfaction (Level 1) does not reliably predict learning (Level 2), and learning does not reliably predict behaviour (Level 3). This means measuring only Level 1 tells you almost nothing about whether the course works. Bates (2004) critiques the implicit assumption that Levels 1–4 form a causal chain; they do not. The evaluation plan should treat each level independently.

## Input Schema

- **Course description:** *e.g. "8-week SQL bootcamp for career changers"*
- **Business/life goal:** *e.g. "Pass data analyst SQL interview"*
- **Evaluation timeframe:** *e.g. "Can follow up at 30/90 days; cannot reach learners at 12 months"*
- Optional: data access, budget

## Prompt

```
You are an expert in training evaluation, trained in Kirkpatrick (1959), the New World Kirkpatrick Model (Kirkpatrick & Kirkpatrick, 2016), and Alliger et al.'s (1997) meta-analysis showing weak correlations between levels. You design evaluation plans that produce real data, not vanity metrics.

Your task is to design an evaluation plan for:

**Course:** {{course_description}}
**Goal:** {{business_or_life_goal}}
**Timeframe:** {{evaluation_timeframe}}

Optional:
**Data access:** {{data_access}}
**Budget:** {{budget_constraint}}

Apply the New World Kirkpatrick approach: START from Level 4 (desired results) and work backwards to Level 1, not forwards.

**Level 4 — Results**
- What observable outcome does the stated goal produce? Define in measurable terms (X% of graduates hired, Y% of children cooking independently, Z% reduction in errors).
- What baseline data is available to compare against?
- Attribution challenge: how will you separate the course's effect from other factors?
- If Level 4 cannot be measured, say so and propose the best available proxy.

**Level 3 — Behavior**
- What does using the skill look like in the real context? Describe observable behaviour.
- Who can observe it? (Manager, parent, peer, self-report, recorded artifact, platform telemetry)
- Timing: when does the behaviour first appear? When should it be stable?
- **Required drivers (New World Kirkpatrick):** What on-the-job support must exist for behaviour to happen? (Manager encouragement, tools, permission, practice opportunities.) If these are missing, Level 3 will fail regardless of course quality.
- Realistic instrument: 1 concrete method you can actually use.

**Level 2 — Learning**
- What specific knowledge, skills, and attitudes must be acquired?
- Pre/post design where possible (measure baseline).
- Assessment format: mock task, performance task, structured interview, portfolio — prefer performance over recall.
- Threshold: what counts as "learned enough"?

**Level 1 — Reaction**
- Keep it short. Useful items: relevance, engagement, confidence to apply, perceived obstacles.
- AVOID "overall satisfaction" as the only question — it correlates weakly with everything.
- Ask about specific friction points you can fix (pacing, clarity, support).

**Leading indicators (for iterating during delivery, before final evaluation):**
- What signals during the course predict Level 2/3 success? (Completion rates, specific exercise performance, engagement in live sessions, peer-teaching quality.)

**Realistic constraints:**
- For any level you cannot measure well, say so explicitly and propose a proxy or acknowledge the gap. Do not promise data you will not collect.

Return output in this structure:

## Evaluation Plan: [Course Name]

### Level 4 — Results
[Metric / baseline / attribution / when measured / realistic constraints]

### Level 3 — Behavior
[Observable behaviour / observer / timing / required drivers / instrument]

### Level 2 — Learning
[Target knowledge/skills/attitudes / pre-post design / assessment format / threshold]

### Level 1 — Reaction
[Specific questions / timing / use of data]

### Leading Indicators
[In-course signals to monitor]

### What You Cannot Measure (and Why)
[Explicit list of gaps with proxies or acknowledgement]

### Data Collection Schedule
[Single table: Level | What | When | From whom | How]

**Self-check before returning:** Verify (a) you started from Level 4 and worked backwards, (b) Level 3 includes required drivers (not just the behaviour), (c) Level 1 is not the only level planned, (d) you explicitly named what cannot be measured, and (e) the schedule is realistic within the stated timeframe and access.
```

## Example Output (abridged)

**Scenario:** *Course: "8-week SQL bootcamp for career changers" / Goal: "Pass junior data analyst SQL interview" / Timeframe: "30/90 days follow-up possible"*

### Level 4 — Results
- **Metric:** % of graduates who receive a junior data role offer within 90 days of completion
- **Baseline:** Industry benchmark ~20% for unstructured self-study (cite source); aim 60%+
- **Attribution:** Cannot cleanly separate course from candidate effort, market, resume quality. Use self-reported attribution and track interview outcomes specifically.
- **When:** Day 90 post-completion
- **Constraint:** Cannot verify offers independently; rely on survey + LinkedIn follow-up

### Level 3 — Behavior
- **Behaviour:** Writing SQL under live interview conditions; translating business questions to queries in real interviews
- **Observer:** Self-report via structured log; offer mock interviews at 30 days
- **Timing:** Expect first evidence at 15–30 days (first interviews)
- **Required drivers:** Learner actually applies to jobs (if not, Level 3 fails regardless). Need active job-search support: resume help, application cadence, interview scheduling. Plan a job-search micro-module as part of course, not optional.
- **Instrument:** Mock interview at Day 30 + self-report interview log at Day 30/90

### Level 2 — Learning
- **Target:** Write intermediate SQL (joins, GROUP BY, subqueries, CTEs) under time pressure
- **Pre-post:** Day 0 baseline query task (expect near-zero) vs Day 56 mock interview
- **Format:** Performance task with 5 interview-style questions, time-boxed
- **Threshold:** 4/5 correct within time limit

### Level 1 — Reaction
- Specific questions only:
  - "Rate confidence to write SQL in a real interview" (1–10, weekly)
  - "What was the single most confusing thing this week?" (open)
  - "What felt too slow or too fast?" (open)
- No "overall satisfaction" — use specific friction data to iterate

### Leading Indicators
- Exercise completion rates (weekly)
- Week 4–5 error patterns in JOINs (predicts overall success)
- Live session attendance (predicts cohort engagement → retention)

### What You Cannot Measure (and Why)
- Long-term career success beyond 90 days — no follow-up mechanism
- ROI in dollar terms — no access to offer salaries without intrusive survey
- Counterfactual (would they have been hired anyway) — no control group; acknowledge as limitation, not manufacture a number

### Data Collection Schedule

| Level | What | When | From whom | How |
|---|---|---|---|---|
| 1 | Confidence + friction | Weekly | All learners | 2-min form |
| 2 | SQL baseline | Day 0 | All learners | Performance task |
| 2 | SQL mock interview | Day 56 | All learners | Recorded mock |
| 3 | Mock interview | Day 30 post | Graduates | Live with data analyst |
| 3 | Interview log | Day 30 & 90 | Graduates | Structured survey |
| 4 | Offer outcome | Day 90 | Graduates | Survey + LinkedIn |

---

## Known Limitations

1. **Levels do not form a causal chain.** Alliger et al. (1997) showed correlations between levels are weak. Measuring Level 1 does not tell you Level 2, etc. Measure each level independently.

2. **Level 3 requires external access.** If you cannot reach graduates after the course ends, Level 3 will be self-report only and subject to survivor bias. Acknowledge this.

3. **Level 4 attribution is hard.** Without a control group, "did the course cause this?" is unanswerable. Be honest about attribution limits.

4. **Not all courses have a Level 4.** For some personal-growth or exploratory courses, there is no measurable real-world outcome. Don't force one — accept Level 2/3 as the realistic ceiling.
