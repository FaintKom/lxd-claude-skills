---
# AGENT SKILLS STANDARD FIELDS (v2)
name: course-onboarding-designer
description: "Design the first 15 minutes of a course — the critical window where learners decide whether to keep going. Produces a step-by-step onboarding flow: welcome, orientation, first win, expectation-setting, support channel introduction, and the first real action. Use when designing any self-paced course where early dropout is a risk (most of them)."
disable-model-invocation: false
user-invocable: true
effort: low

# EXISTING FIELDS

skill_id: "learning-experience-design/course-onboarding-designer"
skill_name: "Course Onboarding Designer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Dirksen (2015) — Design for How People Learn (2nd ed.): early engagement principles"
  - "Nielsen Norman Group — various reports on first-use UX and task completion"
  - "Kizilcec, Piech & Schneider (2013) — Deconstructing disengagement: MOOC dropout patterns (first-week dominates)"
  - "Pintrich (2003) — A motivational science perspective on the role of student motivation"
  - "Bandura (1997) — Self-Efficacy: The Exercise of Control (early success and efficacy)"
input_schema:
  required:
    - field: "course_topic"
      type: "string"
      description: "What the course teaches"
    - field: "audience"
      type: "string"
      description: "Learners and their starting state"
    - field: "delivery_platform"
      type: "string"
      description: "LMS, app, email drip, video-only, etc."
  optional:
    - field: "known_dropout_points"
      type: "string"
      description: "If you have data from previous cohorts about where people leave"
    - field: "tone"
      type: "string"
      description: "Warm / professional / playful"
output_schema:
  type: "object"
  fields:
    - field: "fifteen_minute_flow"
      type: "array"
      description: "Minute-by-minute or step-by-step onboarding sequence"
    - field: "first_win"
      type: "string"
      description: "The specific small success the learner achieves in the first 10 minutes"
    - field: "support_setup"
      type: "string"
      description: "How learners know where to get help before they need it"
    - field: "dropout_risks_mitigated"
      type: "string"
      description: "What this onboarding design prevents"
chains_well_with:
  - "learning-narrative-designer"
  - "learner-journey-mapper"
  - "self-efficacy-builder-sequence"
  - "lesson-opening-designer"
  - "engagement-mechanics-designer"
  - "mayer-multimedia-principles-designer"
teacher_time: "3 minutes"
tags: ["onboarding", "first-impression", "dropout-prevention", "UX", "learning-experience-design"]
---

# Course Onboarding Designer

## What This Skill Does

Designs the first 15 minutes of a course — welcome, orientation, first real action, first small win, expectation-setting, and support-channel introduction. Most dropout in self-paced courses happens in the first session; the first 10 minutes are the highest-leverage moment in the whole course. A well-designed onboarding replaces "here's the syllabus" with "here's you doing the first thing and succeeding." AI is valuable here because course creators are too close to their own content to see what a new learner experiences in minute 1.

## Evidence Foundation

Kizilcec, Piech & Schneider (2013) analysed MOOC dropout patterns across hundreds of thousands of learners and found first-week dropout dominates: if learners don't engage meaningfully in the first session, most never return. Bandura (1997) established that early successes build self-efficacy, which is the single strongest predictor of persistence. Pintrich (2003) synthesises motivational factors showing that expectancy of success (established early) determines effort expenditure. Nielsen Norman Group's UX research on first-use experiences consistently shows the same pattern across apps, games, and learning products: users form durable judgments within the first few minutes. Dirksen (2015) argues onboarding is not a module of the course — it IS the course from the learner's perspective until they decide it's worth continuing.

## Input Schema

- **Topic:** *e.g. "Beginner watercolour painting, online"*
- **Audience:** *e.g. "Adults who haven't painted since childhood, anxious about being bad"*
- **Platform:** *e.g. "Self-paced video LMS"*
- Optional: known dropout points, tone

## Prompt

```
You are a learning experience designer focused on first-use experience. You know that most dropout happens in the first session and that the first 10 minutes determine whether the learner returns. You refuse to start courses with syllabi, introductions, or "about me" videos.

Your task:

**Topic:** {{course_topic}}
**Audience:** {{audience}}
**Platform:** {{delivery_platform}}
**Known dropout points:** {{known_dropout_points}}
**Tone:** {{tone}}

Design the first 15 minutes. Allocate time:

**Minute 0–1: Arrival**
- What does the learner see in the first 10 seconds? It should NOT be a welcome video, NOT a syllabus, NOT "my story". It should be a direct hook tied to why they are here.
- Remove all friction: no accounts to set up mid-flow, no downloads, no "read this first".

**Minute 1–3: Orientation**
- Tell the learner, in ONE sentence, what they will be able to do by the end of this first session. Not by the end of the course — by the end of 15 minutes.
- Tell them what they need to do it (materials, tools, mental state). Keep the list to 1–3 items. If it's longer, reconsider the session.

**Minute 3–10: First real action → first small win**
- The learner DOES something. Not watches, not reads — does.
- The action must be small enough to succeed and real enough to feel like a genuine taste of the skill.
- By minute 10, the learner has a concrete result they can point to: a query that ran, a paragraph written, a dish cooked, a story recorded, a line drawn.
- Celebrate it briefly, then move on. No extended applause — that feels patronising.

**Minute 10–13: Expectation-setting (now that they've earned it)**
- NOW you can tell them what the course is, how long it takes, how it's structured. They have context because they've just experienced a mini version of it.
- Not a syllabus — a promise: "In 6 sessions, you will be able to [specific thing]. Here's the shape of the journey."

**Minute 13–14: Support**
- How will they get help when they get stuck? (They will.) Show it before they need it. Low-friction channel, visible default.
- If there's a community, introduce it with the minimum friction to join.

**Minute 14–15: Exit commitment**
- Not "see you next time" — a concrete action they take BEFORE closing the tab. This could be scheduling next session, sharing their first win, writing what they want to get from the course, or something similarly small and specific.

Critical anti-patterns to avoid:
- Starting with a welcome video longer than 60 seconds
- "About the instructor" content before any value delivered
- Long terms-of-use or expectation-setting before the first action
- Requiring signups or additional accounts mid-flow
- Overly scripted "celebrate yourself" moments that feel hollow
- Asking learners to "introduce themselves" in a forum as the first action (social anxiety = dropout)

Return output in this structure:

## Onboarding Flow: [Course Name]

### Minute-by-Minute

| Time | What happens | What the learner sees/does | Why |
|---|---|---|---|
[Rows per segment]

### First Win (minute 10)
[What specific small result the learner has by minute 10]

### Support Setup
[How and when support is introduced]

### Dropout Risks Mitigated
[What this design prevents, with reasoning]

### Anti-Patterns Explicitly Avoided
[Specific traps this onboarding avoids]

**Self-check before returning:** Verify (a) the first 10 seconds are not a welcome video, (b) the learner does something real by minute 10, (c) expectation-setting happens AFTER the first win, not before, (d) support is introduced before it's needed, and (e) there is a concrete exit commitment, not a vague goodbye.
```

## Example Output (abridged)

**Scenario:** *Beginner watercolour for adults anxious about being bad / self-paced LMS*

### Minute-by-Minute

| Time | What happens | Learner does/sees | Why |
|---|---|---|---|
| 0:00 | Single full-screen image: a messy watercolour with the caption "This was my first one too." | Sees + reads (3 sec) | Instant identification, normalise mess |
| 0:10 | "Today: you will make one mark on paper with water and pigment. That's it. That's the whole first session." | Reads | Radically low stakes |
| 0:30 | "You need: 1 brush, 1 colour, 1 piece of paper, water. Nothing else. Pause the video and get them. I'll wait." | Pauses, gathers | Real action, friction removal done by the learner |
| 3:00 | Demo: instructor loads brush, makes one wet stroke, narrates what they notice | Watches (90 sec) | Worked example with visible reasoning |
| 4:30 | "Your turn. Load your brush. Make one stroke. It will be weird. That's fine." | Does it | First real action |
| 7:00 | "Now look at it. See how the edge is fuzzy? That's the water doing something you didn't tell it to. Welcome to watercolour." | Looks + reads | Reframes "mess" as the actual medium |
| 8:00 | "Make three more strokes. Any direction. Don't think." | Does it | Practice reps, still low stakes |
| 10:00 | "You just painted. Photograph your paper. That's today's result. Keep it." | Photographs | First win is physical and keepable |
| 10:30 | "Over 6 sessions you'll learn to control the water, mix colours, and paint a simple landscape. It's 15 minutes per session. That's the whole course." | Reads | Expectation after proof |
| 13:00 | "Stuck or want to share? Here's our community link. No introductions needed — just post your stroke photo if you want." | Sees link | Low-friction support |
| 14:00 | "Before you close: text one person that you painted today. That's all." | Does it | Public commitment, small |

### First Win
A photograph of four brush strokes on paper, with the instructor's frame: "This is the actual medium — water doing things you didn't tell it to."

### Support Setup
Community link introduced at minute 13 with explicit low-friction invitation. No required introductions. Posting a photo is the minimum viable contribution.

### Dropout Risks Mitigated
- "I'm too bad to even try" → addressed in minute 0 with the messy first-attempt image
- "I don't have the right materials" → materials list is 4 items, all basic
- "The instructor is too good" → instructor shows fuzzy edges as the point, not a mistake
- "I don't know what to expect" → expectation-setting AFTER first success, not before
- Social anxiety from forced intros → explicitly waived

### Anti-Patterns Avoided
- No 2-minute welcome video
- No "about me" segment
- No materials list longer than 4 items
- No forced introduction in community
- No syllabus before first brush stroke

---

## Known Limitations

1. **First win must be genuine.** A fake or trivial "win" is worse than no win. Learners detect condescension.

2. **15 minutes is a target, not a rule.** Some skills need 5 minutes, some 20. The principle is: first real action early, first win before syllabus, support before need.

3. **Onboarding reduces dropout but does not eliminate it.** Learners also drop out because of life circumstances, misaligned expectations, or content that doesn't match the promise. Onboarding closes one hole, not all.
