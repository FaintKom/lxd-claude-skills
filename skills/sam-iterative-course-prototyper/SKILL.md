---
# AGENT SKILLS STANDARD FIELDS (v2)
name: sam-iterative-course-prototyper
description: "Design a course using Allen's Successive Approximation Model (SAM) — iterative, prototype-driven course development with Savvy Start, iterative design, and alpha/beta/gold cycles. Use when speed matters, the topic or audience is novel, or when traditional waterfall ADDIE would take too long and you need to get a working course in front of real learners fast."
disable-model-invocation: false
user-invocable: true
effort: high

# EXISTING FIELDS

skill_id: "instructional-design/sam-iterative-course-prototyper"
skill_name: "SAM Iterative Course Prototyper"
domain: "instructional-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Allen (2012) — Leaving ADDIE for SAM: An Agile Model for Developing the Best Learning Experiences"
  - "Allen & Sites (2012) — Leaving ADDIE for SAM Field Guide"
  - "Allen Interactions — SAM methodology white papers"
  - "Beck et al. (2001) — Manifesto for Agile Software Development (methodological ancestor of SAM)"
  - "Dirksen (2015) — Design for How People Learn (2nd ed.): iterative design principles"
input_schema:
  required:
    - field: "course_topic"
      type: "string"
      description: "What the course teaches"
    - field: "target_audience"
      type: "string"
      description: "Who will take it"
    - field: "business_or_learning_goal"
      type: "string"
      description: "Observable outcome the course must produce"
    - field: "time_to_launch"
      type: "string"
      description: "When the first version must be in front of real learners"
  optional:
    - field: "team_size"
      type: "string"
      description: "Solo designer, small team, full team"
    - field: "sme_access"
      type: "string"
      description: "Are subject-matter experts available and how often"
    - field: "constraints"
      type: "string"
      description: "Budget, tools, platform limitations"
output_schema:
  type: "object"
  fields:
    - field: "savvy_start"
      type: "object"
      description: "Initial brainstorm outputs: problem, audience, rough ideas, first prototype"
    - field: "iterative_design_loop"
      type: "object"
      description: "Prototype / review / revise cycle plan"
    - field: "alpha_build"
      type: "object"
      description: "First complete rough version plan"
    - field: "beta_build"
      type: "object"
      description: "Refined version with real learner data plan"
    - field: "gold_release"
      type: "object"
      description: "Final polished version criteria"
    - field: "iteration_cadence"
      type: "string"
      description: "How often cycles repeat and what triggers each"
chains_well_with:
  - "addie-course-designer"
  - "learner-persona-builder"
  - "training-needs-analyzer"
  - "action-mapping-designer"
  - "kirkpatrick-evaluation-planner"
teacher_time: "8 minutes"
tags: ["SAM", "iterative", "instructional-design", "agile-learning", "prototype"]
---

# SAM Iterative Course Prototyper

## What This Skill Does

Generates a Successive Approximation Model (SAM) plan for iterative, prototype-driven course development. Unlike ADDIE's phase-by-phase waterfall, SAM structures course creation as repeated cycles of design → prototype → review → revise, starting with a rapid "Savvy Start" brainstorm and moving through alpha (rough but complete), beta (refined with real learner data), and gold (polished release). SAM is especially valuable when the topic or audience is novel, speed matters, and the risk of discovering wrong assumptions late would be expensive. AI is valuable here because SAM requires disciplined scoping of each iteration — what to include, what to defer, when to ship — and that discipline is easy to lose under time pressure.

## Evidence Foundation

Michael Allen, a pioneer in interactive learning design, developed SAM (Allen, 2012; Allen & Sites, 2012) explicitly as a response to the waterfall limitations of classic ADDIE. SAM draws directly from Agile software development principles (Beck et al., 2001): rapid iteration, working prototypes over comprehensive documentation, continuous stakeholder collaboration, and willingness to revise throughout. SAM has two main versions: SAM1 (small-scale, 3 iterations) and SAM2 (large-scale projects, explicit preparation/design/development phases that each contain iteration loops). Empirical research specifically on SAM is thinner than ADDIE's long history, but the underlying Agile principles have extensive evidence from software development, and Dirksen (2015) independently argues that iterative prototyping with real learners is the single most reliable way to produce effective learning experiences. SAM's weakness is in contexts requiring formal sign-off at each stage (compliance, regulated industries) where ADDIE's explicit phases map better to organisational gates.

## Input Schema

The user must provide:
- **Course topic:** *e.g. "Public speaking basics for 10–12 year olds"*
- **Target audience:** *e.g. "Children 10–12, mixed confidence levels, Russian-speaking, online cohort"*
- **Business or learning goal:** *e.g. "Each child delivers a 3-minute personal story on camera by end of course"*
- **Time to launch (first version in front of real learners):** *e.g. "3 weeks from today"*

Optional:
- **Team size, SME access, constraints**

## Prompt

```
You are an expert instructional designer using Allen's SAM (Allen, 2012; Allen & Sites, 2012). You design courses iteratively, getting rough prototypes in front of real learners as fast as possible and revising based on observed performance — not committee feedback.

Your task is to design a SAM plan for:

**Course topic:** {{course_topic}}
**Target audience:** {{target_audience}}
**Business or learning goal:** {{business_or_learning_goal}}
**Time to launch:** {{time_to_launch}}

Optional:
**Team size:** {{team_size}}
**SME access:** {{sme_access}}
**Constraints:** {{constraints}}

Apply SAM with discipline. Do not try to produce a polished course in one pass — structure the work as a series of iterations, each with clear scope.

**Step 1 — Savvy Start**
- A 1–2 day concentrated brainstorm, ideally with stakeholders + SME + designer together. Outputs:
  - Problem statement (one sentence)
  - Success criterion (what will a successful learner do?)
  - Audience quick-profile (3 bullets)
  - 3–5 rough course ideas (different angles, not refinements of one idea)
  - ONE first prototype to build immediately — a single activity or module, not a full course
- The goal of the Savvy Start is a rough working prototype within 1–3 days, not a plan document.

**Step 2 — Iterative design loop**
- Repeat 3–5 times before moving to alpha:
  - **Prototype:** Build the smallest possible thing that tests a hypothesis about what will work.
  - **Review:** Put it in front of 1–3 real representative learners (not stakeholders). Observe them using it. Don't explain it.
  - **Revise:** Change what didn't work based on observation, not opinion.
- Each loop should take days, not weeks.
- Explicitly scope each loop: "In this iteration we are testing whether [X hypothesis]."

**Step 3 — Alpha build**
- Alpha = the complete course, end to end, but rough. All modules present, all activities functional, quality uneven. Alpha is ugly but it works.
- Alpha criteria: a learner can go from start to finish without the designer's help.
- Pilot alpha with ~5 representative learners. Observe what works and what doesn't.

**Step 4 — Beta build**
- Beta = refined alpha based on pilot data. Fix what broke, polish what worked, cut what didn't earn its place.
- Beta criteria: measurable performance against the stated goal with a real cohort, even if small.
- Consider beta the first "real" version and charge for it (if commercial) at a discount.

**Step 5 — Gold release**
- Gold = polished, production-quality. Only after beta has validated the design.
- Gold criteria: evidence the course reliably produces the stated outcome; visual/audio polish is appropriate for the audience and brand; support systems (onboarding, help, community) are in place.

**Step 6 — Iteration cadence after launch**
- Never "finish" a course in SAM. Schedule review cycles (quarterly or per-cohort).
- Define triggers for revision: performance drops, content becomes outdated, learner feedback patterns, new evidence from the field.

Critical SAM principles to apply throughout:
- **Real learners, not stakeholders, are your reviewers.** Stakeholder feedback tells you what they think will work; learner observation tells you what does.
- **Speed of iteration beats size of iteration.** Three small loops beat one big loop.
- **The first version will be wrong.** Plan for it. Don't try to prevent it.
- **Don't polish what might be cut.** No final graphics/audio/video until beta.

Return output in this structure:

## SAM Plan: [Course Title]

**Time to first real learner:** [target]
**Total to gold:** [estimated total from Savvy Start to gold]

### Savvy Start (Days 1–2)
[Problem / success criterion / audience quick-profile / 3–5 course angles / first prototype spec]

### Iterative Design Loop (Days 3–14 approx)
[Loop 1–5: each with hypothesis, prototype spec, review method, expected revision]

### Alpha Build
[Scope / what's in and out / pilot plan / pilot size / observation protocol]

### Beta Build
[Revisions expected / measurable criteria / cohort plan / pricing if commercial]

### Gold Release
[Polish criteria / launch plan / ongoing support]

### Iteration Cadence (post-launch)
[Review schedule / revision triggers / data to collect]

### Risks and Trade-offs
[What SAM is trading off for this speed; what to watch out for]

**Self-check before returning:** Verify (a) the first prototype exists within 1–3 days of Savvy Start, (b) real learners — not stakeholders — are the reviewers at each loop, (c) alpha is end-to-end before beta begins (not feature-complete module-by-module), (d) nothing is polished before beta, and (e) a post-launch iteration cadence is scheduled, not left implicit.
```

## Example Output (abridged)

**Scenario:** *Topic: "Public speaking for 10–12 year olds" / Goal: "Each child delivers a 3-min personal story on camera" / Duration to launch: "3 weeks"*

### Savvy Start (Days 1–2)

- **Problem:** Children this age freeze up on camera and produce either over-rehearsed robotic delivery or incoherent rambling. They need a structured way to find and tell a personal story.
- **Success criterion:** Each child records a 3-minute story with clear beginning/middle/end, visible comfort on camera, and at least one moment of genuine personality.
- **Audience quick-profile:** (1) mixed confidence (some love attention, some hide); (2) short attention, need short reps; (3) highly sensitive to peer reaction — safety matters.
- **Course angles considered:**
  1. Story-first (find your story, then speak it)
  2. Camera-first (get comfortable on camera, then add content)
  3. Game-based (improv games → story → delivery)
  4. Hero's journey template (structure-heavy)
  5. Show-and-tell modernised
- **First prototype (build by Day 3):** A single 30-minute Zoom session where kids play one warm-up game, watch one 60-sec story example, try telling a story about "a time I was surprised", and get peer reactions. Goal: see whether game-based warm-up reduces camera anxiety.

### Iterative Design Loop (Days 3–14)

**Loop 1 (Day 3–5)** — Hypothesis: game-based warm-up reduces freezing. Prototype: 30-min session above. Review: observe 4 kids; do they loosen up after the game? Revise: if yes, keep. If no, try camera-off warm-up.

**Loop 2 (Day 6–8)** — Hypothesis: a story template ("once I was X, then Y happened, now I Z") gives structure without killing personality. Prototype: 45-min session introducing template, kids write and deliver. Observe: do they sound robotic or natural?

**Loop 3 (Day 9–11)** — Hypothesis: peer feedback feels safe if done anonymously or with a fixed positive format ("one thing I liked, one thing that surprised me"). Prototype: full peer feedback round after delivery.

**Loop 4 (Day 12–14)** — Hypothesis: 3-minute target is achievable with 4 sessions. Prototype: abbreviated full arc across two sessions for one child.

### Alpha Build (Days 15–17)

End-to-end course: 4 sessions × 60 min. All activities functional but not yet polished. No branded materials, rough slides, placeholder homework sheets. Piloted with 5 children (ideally ones who are representative, not already-confident ones).

- **Pilot observation protocol:** Record sessions with parental consent. Note: where do kids disengage? Which warm-ups work? Does the story template help or constrain?

### Beta Build (Days 18–21, launch cohort Day 21)

Revisions based on alpha pilot:
- Cut what caused confusion or disengagement
- Adjust pacing
- Add a simple rubric for self-feedback
- Create branded worksheet for story template

Beta cohort: 8–10 children. Measure against success criterion (3-min story recorded). Target: 80% success.

### Gold Release (Week 5–6)

After beta cohort, polish: better slides, edited example videos from beta kids (with permission), parent guide, onboarding flow, community/support.

### Iteration Cadence

- Review after each cohort (every 6 weeks)
- Triggers for revision: <70% success rate, parent complaints, specific disengagement patterns
- Annual content refresh (story examples feel dated fast for kids)

### Risks and Trade-offs

- **Trading off:** formal needs analysis (no deep survey of 100 kids — we're learning from 4–5 in week 1)
- **Watch out for:** choosing atypical pilot kids (over-confident or over-shy skews results — aim for representative)
- **Biggest risk:** running out of time for beta iteration and shipping alpha as final — discipline is to NOT do this

---

## Known Limitations

1. **SAM requires access to real representative learners early.** If you cannot get 2–3 learners into a prototype session within a week, SAM's feedback loop breaks down and you drift back into ADDIE-by-default. Be honest about whether learner access is actually available.

2. **Stakeholder-driven organisations struggle with SAM.** If the client insists on reviewing a 50-page plan before any prototype, SAM does not work — either negotiate a different contract or use ADDIE.

3. **SAM works best for courses where you can observe learner performance directly.** For purely cognitive content (reading/watching), iteration is slower because you can't see struggle as easily. Build in explicit comprehension checks and think-alouds.

4. **"Done" is ambiguous in SAM.** Because SAM never truly finishes, teams can over-iterate. Define gold release criteria explicitly at the start, and honour them.

5. **Not appropriate for compliance/regulated training** where content must be approved before any learner sees it. Use ADDIE for those.
