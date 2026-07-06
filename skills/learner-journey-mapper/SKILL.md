---
# AGENT SKILLS STANDARD FIELDS (v2)
name: learner-journey-mapper
description: "Map the complete learner journey from discovery through mastery and beyond: awareness, consideration, signup, onboarding, week-by-week experience, moments of struggle, moments of triumph, completion, post-course life. Surfaces emotional highs and lows, friction points, drop-off risks, and intervention opportunities. Use when auditing existing courses or planning a new one holistically beyond content."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/learner-journey-mapper"
skill_name: "Learner Journey Mapper"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Kalbach (2020) — Mapping Experiences (2nd ed.): journey mapping methodology"
  - "Nielsen Norman Group — Customer Journey Mapping: definition and overview"
  - "Polaine, Løvlie & Reason (2013) — Service Design: journey mapping for services"
  - "Dirksen (2015) — Design for How People Learn: learner-centred mapping"
  - "Kahneman (2011) — peak-end rule: how people remember experiences"
input_schema:
  required:
    - field: "course_or_program"
      type: "string"
      description: "What the course or learning program is"
    - field: "audience"
      type: "string"
      description: "Who the learners are"
  optional:
    - field: "known_pain_points"
      type: "string"
      description: "What you already know goes wrong"
    - field: "existing_data"
      type: "string"
      description: "Analytics, survey, interview data if available"
output_schema:
  type: "object"
  fields:
    - field: "journey_phases"
      type: "array"
      description: "Each phase: learner actions, thoughts, feelings, touchpoints, pain points, opportunities"
    - field: "emotional_curve"
      type: "string"
      description: "The emotional high-and-low shape across the journey"
    - field: "peak_moments"
      type: "string"
      description: "Candidate peak and end moments per Kahneman's peak-end rule"
    - field: "intervention_priorities"
      type: "string"
      description: "Highest-leverage places to fix or amplify"
chains_well_with:
  - "course-onboarding-designer"
  - "learning-narrative-designer"
  - "engagement-mechanics-designer"
  - "learner-persona-builder"
  - "sam-iterative-course-prototyper"
teacher_time: "6 minutes"
tags: ["journey-mapping", "UX", "peak-end", "service-design", "learning-experience-design"]
---

# Learner Journey Mapper

## What This Skill Does

Maps the full learner journey across phases (awareness → consideration → signup → onboarding → active learning → struggle → breakthrough → completion → transfer → advocacy). For each phase, surfaces what the learner is doing, thinking, feeling; the touchpoints; the pain points; and the opportunities. The output is a working map with an emotional curve, flagged peak moments (per Kahneman's peak-end rule), and prioritised intervention points. Unlike content-first course design, this skill is about what it FEELS like to be the learner.

## Evidence Foundation

Journey mapping emerged from service design (Polaine et al., 2013) and customer experience research (NN/g). Kalbach (2020) provides the most rigorous practitioner methodology. Kahneman's peak-end rule (2011) shows people remember experiences primarily by their peak intensity and their ending, not by average quality — which has profound implications for course design: a course with a strong middle peak and a strong ending will be remembered more positively than a uniformly good course with a weak ending. Dirksen (2015) applies journey thinking to learner experience specifically. The evidence base is strongest for journey mapping as a diagnostic and alignment tool; weaker as a direct predictor of outcomes.

## Input Schema

- **Course:** *e.g. "8-week career change bootcamp"*
- **Audience:** *e.g. "Adults leaving corporate jobs"*
- Optional: known pain points, existing data

## Prompt

```
You are a learning experience designer trained in journey mapping (Kalbach, 2020), service design (Polaine et al., 2013), and Kahneman's peak-end rule (2011). You refuse to map journeys that only cover "during the course" — a complete journey starts before signup and ends in post-course life.

Your task:

**Course/program:** {{course_or_program}}
**Audience:** {{audience}}
**Known pain points:** {{known_pain_points}}
**Existing data:** {{existing_data}}

Map the journey across these phases. For each phase, produce a row with: learner actions, thoughts, feelings, touchpoints, pain points, opportunities.

**Phase 1 — Awareness**
How does the learner first encounter this course? What prompted their search? What were they feeling at that moment (frustration, curiosity, fear)?

**Phase 2 — Consideration**
What alternatives are they weighing? What questions do they ask? What would make them click vs. not? What's their emotional state — hopeful, sceptical, desperate?

**Phase 3 — Signup**
The transaction itself: price, commitment, information requested. What friction exists? What doubts surface? Buyer's remorse risk immediately after?

**Phase 4 — Pre-course (the gap)**
Between signup and first session: what happens? Silence is the #1 dropout risk here. What could keep momentum?

**Phase 5 — Onboarding (first 15 min)**
First contact with the actual course. What do they see first? How fast do they experience value? (Chain with `course-onboarding-designer` for detail.)

**Phase 6 — Early weeks (momentum or decay)**
Sessions 1–3. Motivation is high but fragile. What typically goes wrong here?

**Phase 7 — The messy middle**
Mid-course. Novelty has worn off. Real difficulty hits. This is where most dropout happens. What is the learner feeling? What's the typical moment of "maybe I should quit"?

**Phase 8 — The turning point**
Somewhere in the middle-to-late, something clicks. The learner realises they're becoming the thing they wanted to become. Can you design for this, or does it happen randomly?

**Phase 9 — Home stretch**
Final sessions. Pressure of finishing. Fear of the final test. Fatigue.

**Phase 10 — Completion (the ending)**
How does the course END? Kahneman: the ending disproportionately shapes memory. Most courses end weakly ("congrats, you finished"). What could the ending feel like?

**Phase 11 — Post-course**
Weeks after completion. Does the learner keep the skill? Do they return? Do they tell others? What is the "new normal"?

**Phase 12 — Advocacy**
If the course worked, do they recommend it? What would have to be true?

For each phase, include:
- Actions (what they do)
- Thoughts (what they say to themselves)
- Feelings (honest emotion, not pleasant labels)
- Touchpoints (where the course intersects their life)
- Pain points (what hurts or fails)
- Opportunities (where design intervention has highest leverage)

After the phases, produce:

**Emotional curve:** Describe the shape of emotional intensity across the journey. Where are the peaks? Where are the lows? Where is the ending relative to the peak?

**Peak and end candidates:** Name 2–3 candidate "peak moments" and propose a strong ending. If these are currently weak or missing, flag it.

**Intervention priorities:** Rank the top 5 places where intervention would have highest impact, with reasoning.

Return output in this structure:

## Learner Journey: [Course Name]

### Journey Phases

| Phase | Actions | Thoughts | Feelings | Touchpoints | Pain Points | Opportunities |
|---|---|---|---|---|---|---|
[Row per phase]

### Emotional Curve
[Description + text-based shape if helpful]

### Peak Moments + Ending Design
[Candidate peaks + ending proposal]

### Intervention Priorities
[Ranked list of 5 with reasoning]

**Self-check before returning:** Verify (a) you mapped BEFORE signup and AFTER completion, not just "the course itself", (b) feelings are honest not sanitised, (c) you applied peak-end rule to the ending, and (d) intervention priorities are ranked, not a flat list.
```

## Example Output (abridged)

**Scenario:** *8-week career change bootcamp, adults leaving corporate*

### Selected Phases (abridged)

| Phase | Feelings | Pain | Opportunity |
|---|---|---|---|
| Awareness | Frustrated, stuck, slightly ashamed of wanting change | Imposter feeling while googling | Landing page that names the shame and normalises it |
| Pre-course gap (7 days) | Cooling enthusiasm, buyer's remorse | Silence = doubt | 2–3 touchpoints: prep email, community intro, tiny task |
| Messy middle (week 4–5) | Exhausted, "I'm not improving" | Dropout spike | Turning-point lesson + instructor check-in |
| Ending | Relieved but flat | "That's it?" | Strong capstone + peer celebration + tangible artifact |
| Post-course | Proud but alone | No next step | Alumni community, advocacy ask, 30-day check |

### Emotional Curve
Rises sharply in onboarding (honeymoon), plateaus weeks 2–3, dips sharply week 4–5 (the valley), recovers slowly with turning point, climbs to completion. **Problem: most courses end flat right after climb, so the LAST thing the learner feels is anticlimax.** Kahneman predicts this will dominate memory.

### Peak Moments + Ending
- Candidate peak: the turning point ("oh, I can actually do this") in week 6
- Proposed end: personal artifact (their portfolio piece) shown to a real audience (other learners + guest reviewer) — stronger ending than "final quiz + certificate"

### Intervention Priorities
1. **Ending design** — highest leverage per peak-end rule, currently weakest
2. **Pre-course gap** — cheapest to fix, biggest drop prevention
3. **Week 4–5 valley** — highest dropout risk
4. **Turning point** — currently random, could be designed
5. **Post-course follow-up** — most programs ignore it entirely

---

## Known Limitations

1. **Without real learner data, the map is a hypothesis.** Validate with interviews before investing in fixes.

2. **Peak-end rule is robust but not absolute.** Some learners remember duration or specific moments. Treat it as strongly suggestive.

3. **Journeys vary by persona.** If audience is heterogeneous, consider mapping per persona rather than averaging.
