---
# AGENT SKILLS STANDARD FIELDS (v2)
name: training-needs-analyzer
description: "Diagnose whether a stated problem is actually a training problem or something else (motivation, tools, process, environment, management). Use BEFORE building a course — the biggest waste in course creation is building training for a non-training problem. Outputs a diagnosis with recommendation: train, don't train, or train only in combination with other fixes."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "instructional-design/training-needs-analyzer"
skill_name: "Training Needs Analyzer"
domain: "instructional-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Gilbert (1978) — Human Competence: Engineering Worthy Performance (behavior engineering model)"
  - "Mager & Pipe (1997) — Analyzing Performance Problems: Or, You Really Oughta Wanna (3rd ed.)"
  - "Rummler & Brache (2012) — Improving Performance (3rd ed.)"
  - "Moore (2017) — Map It: Strategic Training Design"
  - "Rossett (2009) — First Things Fast: A Handbook for Performance Analysis"
input_schema:
  required:
    - field: "presenting_problem"
      type: "string"
      description: "The problem as stated by the person asking for training"
    - field: "target_group"
      type: "string"
      description: "Who is supposed to be doing the thing differently"
  optional:
    - field: "current_performance"
      type: "string"
      description: "What they do now"
    - field: "desired_performance"
      type: "string"
      description: "What they should do"
    - field: "context_observations"
      type: "string"
      description: "Anything observed about tools, environment, incentives, management"
output_schema:
  type: "object"
  fields:
    - field: "sharpened_problem"
      type: "string"
      description: "Restated problem with measurable gap"
    - field: "diagnosis"
      type: "object"
      description: "Root cause(s) classified as skill, knowledge, motivation, environment, process, or other"
    - field: "recommendation"
      type: "string"
      description: "Train / don't train / combination fix"
    - field: "non_training_interventions"
      type: "string"
      description: "What else must change for the problem to be solved"
    - field: "training_scope_if_needed"
      type: "string"
      description: "If training is part of the solution, what exactly it must cover"
chains_well_with:
  - "action-mapping-designer"
  - "learner-persona-builder"
  - "addie-course-designer"
  - "kirkpatrick-evaluation-planner"
teacher_time: "4 minutes"
tags: ["needs-analysis", "performance-analysis", "Mager", "Gilbert", "instructional-design"]
---

# Training Needs Analyzer

## What This Skill Does

Diagnoses whether a stated problem is actually a training problem. Most "we need training" requests are not — the problem is motivation, tools, process, management, or environment. Building a course for a non-training problem wastes months and fails silently. This skill applies the classic Mager/Pipe performance analysis flowchart and Gilbert's Behavior Engineering Model to classify root causes and recommend: train, don't train, or train in combination with other fixes. It forces explicit consideration of "is this a skill/knowledge gap?" before defaulting to course design.

## Evidence Foundation

Robert Mager and Peter Pipe's classic "Analyzing Performance Problems: Or, You Really Oughta Wanna" (1997) introduced the performance analysis flowchart widely used in instructional design: first ask whether the performer could do the task if their life depended on it. If yes, the problem is not training — it is motivation, consequences, or environment. If no, the problem may be training — but only if a skill gap exists that practice could close. Thomas Gilbert (1978) formalised this in his Behavior Engineering Model: performance has six contributing factors, of which only two (knowledge and capacity) are training-addressable; the other four (data/feedback, instruments, incentives, motives) are environmental or motivational. Gilbert's key empirical finding: environmental factors contribute far more to performance problems than individual factors, yet organisations default to training as the solution. Rummler & Brache (2012) extend this at the organisational level. Cathy Moore (2017) operationalises it for modern workplace contexts. The evidence is consistent across 40+ years: training is the right answer to a minority of "we need training" requests.

## Input Schema

- **Presenting problem:** *e.g. "Our new analysts can't write good SQL queries"* or *"Kids in the cooking program can't follow recipes at home"*
- **Target group:** *e.g. "Junior analysts in their first 90 days" / "Children aged 9–11"*
- Optional: current performance, desired performance, context observations

## Prompt

```
You are a performance analyst trained in Mager & Pipe (1997), Gilbert's (1978) Behavior Engineering Model, and Rossett (2009). You do not build training — you diagnose whether training is even the right answer. Your default assumption is that most stated "training needs" are actually performance problems with non-training root causes.

Your task is to diagnose:

**Presenting problem:** {{presenting_problem}}
**Target group:** {{target_group}}

Optional:
**Current performance:** {{current_performance}}
**Desired performance:** {{desired_performance}}
**Context observations:** {{context_observations}}

Run this diagnostic sequence:

**Step 1 — Sharpen the problem.**
- Restate as a specific, observable performance gap. Not "they're bad at SQL" — "junior analysts cannot write a JOIN query for a two-table business question within 10 minutes, target is 8/10 analysts able to do this by Day 30."
- If the gap cannot be stated in measurable terms, ask what evidence led to the claim that there is a problem. Sometimes there isn't one.

**Step 2 — The Mager/Pipe question: "If their life depended on it, could they do it right now?"**
- If YES → this is NOT a skill/knowledge gap. The problem is motivation, incentives, environment, or consequences. Training will not fix it. Go to Step 4.
- If NO → skill/knowledge gap is possible. Go to Step 3.
- If you don't know → ask what evidence is available. A common mistake is assuming a skill gap when the person has never been asked to perform.

**Step 3 — If they cannot do it, is it because:**
- They were never taught? → training may help (formal instruction).
- They were taught but forgot? → job aid or refresher may beat full training.
- They were taught but never practised in context? → practice/coaching beats content.
- The skill is outside the Zone of Proximal Development? → prerequisite gaps must be addressed first.
- The tools make it impossible? → fix the tools, not the people.

**Step 4 — Gilbert's six factors. For each, say "adequate" or "problem" or "unknown":**
1. **Data/feedback** — do they know what success looks like? Are they getting feedback on their performance?
2. **Instruments/tools** — do they have the tools and resources needed?
3. **Incentives** — are there consequences (positive or negative) for doing or not doing the thing?
4. **Knowledge/skill** — do they know how?
5. **Capacity** — are they physically/cognitively able?
6. **Motives** — do they want to?
- Training addresses only #4 (partially). Everything else is non-training.

**Step 5 — Recommendation.**
- **Don't train:** if the problem is environmental, motivational, or procedural. Recommend specific non-training interventions.
- **Train in combination:** most common — training closes the skill gap but only works if non-training fixes also happen. Specify BOTH.
- **Train:** rare, only if the issue is genuinely a skill/knowledge gap in a supportive environment.

**Step 6 — If training is recommended, scope it.**
- What specifically must the training cover? Keep it minimal.
- What is NOT part of training but part of the overall solution?

Return output in this structure:

## Diagnosis: [Problem Name]

### Sharpened Problem
[Measurable gap]

### Mager/Pipe Test
[Could they do it if their life depended on it? Reasoning.]

### Gilbert's Six Factors

| Factor | Status | Evidence |
|---|---|---|
| Data/feedback | | |
| Instruments/tools | | |
| Incentives | | |
| Knowledge/skill | | |
| Capacity | | |
| Motives | | |

### Recommendation
**[Train / Don't train / Combination]**
[Reasoning in 2–4 sentences]

### Non-Training Interventions
[Specific recommended changes — who, what, by when]

### Training Scope (if training is part of the solution)
[What specifically to train, kept minimal]

### Questions That Need Answers
[Assumptions flagged — what evidence would confirm or reject this diagnosis]

**Self-check before returning:** Verify (a) the Mager/Pipe question was actually asked and answered, not skipped, (b) all six Gilbert factors were considered, (c) the recommendation matches the diagnosis (don't recommend training if Gilbert factors 1–3 are the real issue), and (d) non-training interventions are concrete.
```

## Example Output (abridged)

**Scenario:** *Presenting problem: "Our sales reps can't use the CRM properly" / Target: "50 mid-market sales reps, 6 months on the job"*

### Sharpened Problem
Only 30% of sales reps log their customer calls in the CRM within 24 hours. Management wants 90%. Data quality gaps are breaking the forecasting pipeline.

### Mager/Pipe Test
**Could they do it if their life depended on it?** Mostly yes — informal observation shows most reps can navigate the CRM and log a call in under 2 minutes when asked. One interviewed rep demonstrated this live. **This is not primarily a skill gap.**

### Gilbert's Six Factors

| Factor | Status | Evidence |
|---|---|---|
| Data/feedback | **Problem** | Reps don't know whether they're above or below target; no weekly visibility |
| Instruments/tools | **Problem** | CRM mobile app is slow, freezes on cellular networks — reps wait to get to their laptops and then forget |
| Incentives | **Problem** | Commission depends on closed deals, not logged activities. No consequence for not logging. |
| Knowledge/skill | Adequate | Demonstrated ability when prompted |
| Capacity | Adequate | No evidence of cognitive/physical barrier |
| Motives | **Problem** | Reps view CRM as "management overhead", not their tool |

### Recommendation
**DON'T TRAIN (alone).** This is primarily an instrument/incentive/feedback problem, not a skill gap. A training course on "how to use the CRM" will produce temporary compliance that decays within 2 weeks. Fix the non-training factors first; if a residual skill gap remains, a short job aid will suffice — not a course.

### Non-Training Interventions
1. **Tools:** fix the mobile app performance issue (engineering priority) — reps need to log within minutes of a call, not hours later.
2. **Feedback:** weekly personal dashboard showing each rep's logging rate vs team average. Automated, not manual.
3. **Incentives:** tie 10% of commission to data quality (logged within 24h + complete fields). This is the key change.
4. **Motives:** managers should stop framing CRM as "reporting" and start showing reps the pipeline forecast they personally influence. Skills and narrative change, not training.
5. **Quick reference:** one-page job aid showing the 3 required fields per logged call. Enough to close any residual knowledge gap.

### Training Scope (if any)
Minimal. A 10-minute video for new hires covering the 3 required fields and where the mobile app is. No 2-hour course. Include in existing onboarding, don't build a standalone course.

### Questions That Need Answers
- Have we confirmed that the mobile app IS slow (engineering data) or are we assuming?
- What fraction of late logs are "forgot" vs "tool was broken" vs "didn't care"?
- Is management actually willing to change commission structure? If not, this problem is not solvable by any amount of training.

---

## Known Limitations

1. **This skill's default suspicion of training is intentional.** It will sometimes recommend "don't train" when training would actually help. Chain with real stakeholder conversations before committing.

2. **Some organisations cannot fix the non-training factors.** If management will not change incentives or tools, a training course may be the only politically possible action — even though it will not work. Name this tradeoff honestly.

3. **For courses you sell to external individual learners** (not corporate), Gilbert's organisational factors apply differently: the "environment" is the learner's own life, and you have less control. Still, the Mager/Pipe question is critical — people pay for courses when their real need is a different intervention.

4. **Diagnosis depends on evidence.** If you have no real data about the target group's current performance, flag everything as "unknown" and recommend a short observation or interview phase before course design.
