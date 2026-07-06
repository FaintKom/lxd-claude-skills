---
# AGENT SKILLS STANDARD FIELDS (v2)
name: gagne-nine-events-lesson-designer
description: "Design a single lesson or training session using Gagné's Nine Events of Instruction: gain attention, inform objectives, stimulate recall, present content, provide guidance, elicit performance, provide feedback, assess performance, enhance retention and transfer. Use when building a 30–90 minute standalone lesson (live or self-paced) and want a proven structure that maps to each step of human learning."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "instructional-design/gagne-nine-events-lesson-designer"
skill_name: "Gagné Nine Events Lesson Designer"
domain: "instructional-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Gagné (1985) — The Conditions of Learning and Theory of Instruction (4th ed.)"
  - "Gagné, Wager, Golas & Keller (2005) — Principles of Instructional Design (5th ed.)"
  - "Driscoll (2005) — Psychology of Learning for Instruction (Gagné contextualised)"
  - "Reigeluth (1999) — Instructional-Design Theories and Models Vol. II"
input_schema:
  required:
    - field: "lesson_objective"
      type: "string"
      description: "What learners will be able to do by the end (one or two observable outcomes)"
    - field: "learner_profile"
      type: "string"
      description: "Age/level/prior knowledge"
    - field: "lesson_duration"
      type: "string"
      description: "Total time available"
    - field: "delivery_mode"
      type: "string"
      description: "Live / self-paced / blended"
  optional:
    - field: "known_misconceptions"
      type: "string"
      description: "Common mistakes or prior-knowledge gaps to address"
    - field: "available_media"
      type: "string"
      description: "Video/slides/physical materials available"
output_schema:
  type: "object"
  fields:
    - field: "nine_events"
      type: "array"
      description: "Each event as concrete activity with timing and rationale"
    - field: "total_timing"
      type: "string"
      description: "Running time across events"
    - field: "materials_list"
      type: "string"
      description: "Everything needed to run the lesson"
chains_well_with:
  - "merrill-first-principles-lesson-designer"
  - "addie-course-designer"
  - "explicit-instruction-sequence-builder"
  - "lesson-opening-designer"
  - "checking-for-understanding-protocol-designer"
  - "retrieval-practice-generator"
teacher_time: "4 minutes"
tags: ["Gagne", "nine-events", "lesson-design", "instructional-design"]
---

# Gagné Nine Events Lesson Designer

## What This Skill Does

Generates a single-lesson plan structured around Gagné's Nine Events of Instruction, with concrete activity, timing, and rationale for each event. Each event maps to a distinct cognitive process required for learning, so skipping events (which most lesson designers do — typically cutting stimulating recall, providing guidance, and enhancing retention) leaves predictable gaps. AI is valuable here because tracking all nine events without drift is hard, and the "cut for time" instinct tends to eliminate the events that matter most for long-term retention.

## Evidence Foundation

Robert Gagné (1985; Gagné, Wager, Golas & Keller, 2005) developed the Nine Events of Instruction from his Conditions of Learning theory, which identifies distinct internal cognitive processes that must occur for learning to happen. Each event in the external instruction is designed to trigger a specific internal process: gaining attention triggers selective perception; informing of objectives triggers expectancy; stimulating recall triggers retrieval of prerequisite knowledge into working memory; presenting content triggers selective perception and encoding; providing guidance triggers semantic encoding; eliciting performance triggers response generation; providing feedback reinforces and corrects; assessing performance retrieves and cues; enhancing retention promotes transfer. Driscoll (2005) situates Gagné within cognitivist learning theory; Reigeluth (1999) documents its enduring influence on instructional design. Evidence for each event individually is strong (attention, retrieval practice, scaffolded guidance, feedback, spaced retention all have independent research bases). The sequencing is less empirically tested than the individual events, but the nine-event structure remains the most widely taught lesson framework in instructional design education.

## Input Schema

- **Lesson objective:** *e.g. "Learners can write a simple SELECT with WHERE clause for a given table and question"*
- **Learner profile:** *e.g. "Adult career changers, zero SQL prior knowledge, some Excel"*
- **Duration:** *e.g. "60 minutes live"*
- **Delivery mode:** *e.g. "Zoom cohort session"*
- Optional: known misconceptions, available media

## Prompt

```
You are an expert instructional designer trained in Gagné's Conditions of Learning and the Nine Events of Instruction (Gagné, 1985; Gagné et al., 2005). You design lessons that honour all nine events, not just "present content + do some practice."

Your task is to design a lesson for:

**Objective:** {{lesson_objective}}
**Learner profile:** {{learner_profile}}
**Duration:** {{lesson_duration}}
**Delivery mode:** {{delivery_mode}}

Optional:
**Known misconceptions:** {{known_misconceptions}}
**Available media:** {{available_media}}

Design concrete activities for all nine events. Do not skip any. Allocate time for each. The default mistake is to spend 80% on Event 4 (present content) — resist this.

**Event 1 — Gain Attention (1–3 min)**
- Hook that makes the objective feel worth caring about. Not clickbait — genuine curiosity or need.
- For adults: show the pain or reward. For children: surprise or puzzle.

**Event 2 — Inform Learners of Objectives (1–2 min)**
- State what they will be able to do by the end, in learner language.
- Show success criteria so they know what "got it" looks like.

**Event 3 — Stimulate Recall of Prior Learning (2–5 min)**
- Activate relevant prerequisite knowledge into working memory.
- Do NOT skip this — it is the event most often cut and the one that most reliably improves new learning.
- Use a retrieval question, a familiar analogy, or a quick task.

**Event 4 — Present the Content (variable; minimise)**
- Introduce the new material with worked examples, clear visuals, and connection to what was just recalled.
- Keep segments short; avoid lectures longer than 10 minutes without interaction.

**Event 5 — Provide Learning Guidance (variable)**
- Scaffolding: analogies, worked examples, think-alouds, hints.
- This is how you turn presentation into understanding.

**Event 6 — Elicit Performance (variable; substantial time)**
- Have learners actually DO the thing, in a low-stakes form first.
- Not "discuss" or "reflect" — produce an observable response.

**Event 7 — Provide Feedback (ongoing)**
- Specific feedback on their response — what was right, what to fix, why.
- Immediate rather than delayed where possible.

**Event 8 — Assess Performance (variable)**
- An independent task that demonstrates whether the objective was met.
- Not just for grading — for the learner to know where they stand.

**Event 9 — Enhance Retention and Transfer (3–5 min)**
- Spaced retrieval plan, transfer prompt ("where else could you use this?"), takeaway reference.
- This is the second most-cut event and the one that most determines whether learning survives the week.

Return your output in this structure:

## Lesson Plan: [Topic]

**Objective:** [restated]
**Duration:** [total]
**Mode:** [mode]

### Timing Budget
| Event | Time | Activity |
|---|---|---|
[Summary row per event]

### Detailed Events
[Each event: concrete activity, materials, teacher script snippet or prompt, why this event in this form]

### Materials
[Everything needed to run the lesson]

### After the Lesson (Event 9 extension)
[Specific retention plan — what happens 1 day later, 1 week later]

**Self-check before returning:** Verify (a) all nine events are present with concrete activities (not placeholders), (b) Event 4 does not consume more than ~30% of total time, (c) Events 3 and 9 are not skipped, and (d) Event 6 produces an observable response, not just "discussion".
```

## Example Output (abridged)

**Scenario:** *Objective: "Write a SELECT with WHERE for a given table and question" / Adult career changers, zero SQL / 60 min / Zoom*

### Timing Budget
| Event | Time | Activity |
|---|---|---|
| 1 Attention | 3 | "Guess the data" challenge |
| 2 Objectives | 2 | Show success criteria |
| 3 Recall | 5 | Excel filter analogy quiz |
| 4 Present | 12 | SELECT/WHERE walkthrough |
| 5 Guidance | 8 | Worked examples with think-aloud |
| 6 Perform | 15 | 5 practice queries live |
| 7 Feedback | (ongoing) | Per-query feedback in chat |
| 8 Assess | 10 | Independent 3-query task |
| 9 Retain | 5 | Retention plan + cheat sheet |

### Detailed Events (abridged)

**Event 1 — Gain Attention (3 min):** Show a messy Excel file of 5,000 customer records. Ask: "How long to find all customers in Moscow who spent more than 10,000₽?" Wait for groans. Then: "SQL does this in 2 seconds. Let's learn how today."

**Event 3 — Stimulate Recall (5 min):** "You've all used Excel filters. When you filter, you tell Excel: show me rows WHERE column X equals Y. That's exactly what SQL's WHERE does. Quick: in Excel, how would you filter this table for city = Moscow?" Collect 2 answers. "Good — now we'll do the same thing in SQL syntax, and then something Excel cannot easily do."

**Event 6 — Elicit Performance (15 min):** 5 progressively harder queries. Learners type into shared SQL sandbox. Order: (1) all rows, (2) filter by city, (3) filter by amount, (4) both, (5) both + order by amount. After each, pair-share what they tried.

**Event 9 — Enhance Retention (5 min):** One-page cheat sheet. Homework: 3 new questions tomorrow morning (spaced retrieval). "Where else could this be useful in your current job? Think of one dataset you work with." Transfer prompt.

### Materials
- Messy Excel file for hook
- SQL sandbox link
- Worked example slides (3 slides max)
- 5 practice queries (Event 6) + 3 assessment queries (Event 8)
- One-page cheat sheet PDF

### After the Lesson
- **Day +1:** Email with 3 spaced retrieval queries
- **Day +3:** Slack reminder: "Try one query on your own data"
- **Week +1:** Week 2 lesson opens with 2 retrieval questions from this lesson

---

## Known Limitations

1. **Nine events is a lot for a short lesson.** For 30-minute sessions, you must be disciplined — default is to cut Events 3 and 9, which is exactly wrong. Better to cut content depth in Event 4.

2. **The event sequence is not sacred.** Gagné himself acknowledged events can overlap or reorder. What matters is that each cognitive process is triggered, not that events happen in strict linear order.

3. **For discovery or inquiry lessons**, Events 4 and 5 look different (content emerges from exploration). The nine events still apply but the teacher's role shifts.

4. **Gagné's framework is cognitivist** and lighter on motivation/emotion than modern frameworks. Pair with `flow-state-condition-designer` or `self-efficacy-builder-sequence` when affective factors matter.
