---
# AGENT SKILLS STANDARD FIELDS (v2)
name: engagement-mechanics-designer
description: "Design engagement mechanics for a course — progress markers, streaks, variable rewards, social proof, commitment devices, loss aversion hooks — without falling into manipulative gamification. Use when courses have high dropout, when the content is intrinsically dry, or when learners need help maintaining practice between sessions. Grounded in self-determination theory and behaviour design, not in points-and-badges superficial gamification."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/engagement-mechanics-designer"
skill_name: "Engagement Mechanics Designer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Deci & Ryan (2000) — Self-determination theory: autonomy, competence, relatedness as intrinsic motivators"
  - "Fogg (2019) — Tiny Habits: behaviour design for sustained engagement"
  - "Eyal (2014) — Hooked: trigger / action / variable reward / investment loop"
  - "Werbach & Hunter (2012) — For the Win: evidence-based gamification"
  - "Hanus & Fox (2015) — Assessing the effects of gamification in the classroom: negative effects possible"
  - "Deterding et al. (2011) — From game design elements to gamefulness"
input_schema:
  required:
    - field: "course_description"
      type: "string"
      description: "Course topic, length, format"
    - field: "audience"
      type: "string"
      description: "Who the learners are"
    - field: "engagement_problem"
      type: "string"
      description: "What's wrong: dropout / low completion / irregular practice / disengagement mid-way / other"
  optional:
    - field: "platform_capabilities"
      type: "string"
      description: "What the LMS/app can actually support (notifications, streaks, social features, etc.)"
    - field: "constraints"
      type: "string"
      description: "What you cannot do (ethics, audience age, platform limits)"
output_schema:
  type: "object"
  fields:
    - field: "diagnosis"
      type: "string"
      description: "Root cause of the engagement problem (autonomy / competence / relatedness deficit, friction, etc.)"
    - field: "mechanics_prescribed"
      type: "array"
      description: "Specific mechanics to add, with reasoning"
    - field: "mechanics_avoided"
      type: "string"
      description: "Manipulative/harmful patterns explicitly rejected"
    - field: "measurement"
      type: "string"
      description: "How to tell whether the mechanics helped"
chains_well_with:
  - "course-onboarding-designer"
  - "learning-narrative-designer"
  - "flow-state-condition-designer"
  - "self-efficacy-builder-sequence"
  - "agency-scaffold-generator"
  - "learner-journey-mapper"
teacher_time: "5 minutes"
tags: ["engagement", "gamification", "motivation", "SDT", "behaviour-design", "learning-experience-design"]
---

# Engagement Mechanics Designer

## What This Skill Does

Designs engagement mechanics grounded in Self-Determination Theory (autonomy, competence, relatedness) and behaviour design, rather than shallow points-and-badges gamification. The skill first diagnoses WHY engagement is low (usually an SDT deficit or a friction problem), then prescribes specific mechanics — progress signals, commitment devices, social proof, variable rewards — paired with explicit rejection of manipulative patterns that can actively harm intrinsic motivation.

## Evidence Foundation

Deci & Ryan's Self-Determination Theory (2000) identifies three universal psychological needs: autonomy (feeling in control), competence (feeling capable), and relatedness (feeling connected). Courses that support these produce sustained intrinsic motivation; courses that violate them collapse. Hanus & Fox (2015) demonstrated experimentally that naive gamification (badges, leaderboards) can actually HARM intrinsic motivation by shifting locus of control external. Werbach & Hunter (2012) and Deterding et al. (2011) argue for "meaningful gamification" that supports SDT rather than overrides it. Fogg (2019) shows that sustained behaviour change requires low-friction triggers + high motivation + small easy actions (B=MAT). Eyal (2014) documents how consumer products use trigger/action/reward/investment loops — useful to understand but ethically fraught when applied without restraint.

## Input Schema

- **Course:** *e.g. "6-week language learning app"*
- **Audience:** *e.g. "Adults learning Spanish, variable commitment, high dropout by week 3"*
- **Engagement problem:** *e.g. "60% of users stop opening the app after day 10"*
- Optional: platform capabilities, constraints

## Prompt

```
You are a learning experience designer trained in Self-Determination Theory (Deci & Ryan, 2000), behaviour design (Fogg, 2019), and the empirical literature on gamification that shows shallow approaches can HARM intrinsic motivation (Hanus & Fox, 2015). You refuse to prescribe badges-and-leaderboards solutions without diagnosing the underlying problem first.

Your task:

**Course:** {{course_description}}
**Audience:** {{audience}}
**Engagement problem:** {{engagement_problem}}
**Platform:** {{platform_capabilities}}
**Constraints:** {{constraints}}

**Step 1 — Diagnose the root cause.**
Map the engagement problem to SDT needs and friction:
- **Autonomy deficit:** learners feel controlled, no choice, forced pace
- **Competence deficit:** learners feel stuck, too hard, can't see progress, no small wins
- **Relatedness deficit:** learners feel alone, no peer connection, no human contact
- **Motivation (Fogg):** is the action too hard? Is the trigger missing? Is the motivation too low at the moment of action?
- **Friction:** are there pointless steps between intent and action?

Do NOT jump to mechanics before diagnosing. The same symptom (dropout at week 3) can have very different causes.

**Step 2 — Prescribe mechanics that address the diagnosed cause.**

For **autonomy deficits:**
- Offer meaningful choices (path, pace, topic selection)
- Allow learners to set their own goals
- Remove forced sequences when prerequisites don't require them
- Let learners skip or revisit

For **competence deficits:**
- Clear visible progress (progress bars tied to meaningful milestones, not arbitrary points)
- Early small wins (see `course-onboarding-designer`)
- Difficulty progression that matches ability (adaptive when possible)
- Specific feedback on what improved

For **relatedness deficits:**
- Cohort features (visible classmates, even if not interacting)
- Peer support channels with low posting friction
- Instructor or TA presence signals
- Commitment to a specific person ("tell someone what you learned today")

For **friction:**
- One-click resume
- Pick-up-where-you-left-off
- Mobile + desktop parity
- Notifications that arrive at the right time, not random

Use specific mechanics:
- **Progress signals:** visible, tied to actual learning, not arbitrary XP
- **Streaks:** only if missing a day doesn't PUNISH the learner (no streak loss guilt spirals); use "flexible streaks" with grace days
- **Commitment devices:** public commitment, calendar blocks, accountability partner
- **Variable rewards:** unexpected bonus content, surprise feedback; use sparingly — overuse becomes manipulation
- **Social proof:** "X people completed this module this week" — factual, not comparative
- **Loss aversion:** frame around what's at stake, honestly; do NOT fabricate urgency

**Step 3 — Explicitly reject patterns that harm intrinsic motivation.**
List the mechanics you are NOT using and why:
- Leaderboards that shame low performers
- Badges that replace the activity's meaning ("earn 100 XP!" when the goal should be "write a sentence")
- Streaks that guilt-trip users into opening the app
- Fake deadlines / fabricated scarcity
- Dark patterns (hard-to-cancel, notification spam, social pressure)

**Step 4 — Measure.**
How will you know whether the mechanics helped? What metric will move if the diagnosis was right? What will you watch for to see if you accidentally harmed intrinsic motivation (e.g., completion up but retention of learning down)?

Return output in this structure:

## Engagement Design: [Course]

### Diagnosis
[Root cause mapped to SDT / friction. Reasoning.]

### Prescribed Mechanics
[Specific mechanics with reasoning linking each to the diagnosis]

### Explicitly Rejected Patterns
[Manipulative or counterproductive patterns avoided, with reasoning]

### Measurement Plan
[How to know if it worked + warning signs of backfire]

**Self-check before returning:** Verify (a) you diagnosed BEFORE prescribing, (b) the prescribed mechanics map to a specific diagnosed cause, (c) you explicitly rejected at least one common manipulative pattern, and (d) you named a failure mode to watch for.
```

## Example Output (abridged)

**Scenario:** *6-week language app, 60% stop opening by day 10*

### Diagnosis
Most likely: **competence deficit** (users feel stuck or not improving) + **friction** (app lives on phone but notifications come at wrong times). Probably NOT a relatedness problem (language apps rarely are). Autonomy partial — forced sequence in some apps.

### Prescribed Mechanics
1. **Visible competence progress:** replace XP with "you can now say X sentences you couldn't on day 1" — meaning-carrying, not arbitrary. (Competence)
2. **Early small win:** on day 1, user says one sentence out loud that a native speaker would understand. Record it; play it back on day 10. (Competence)
3. **Flexible streaks:** 7-day streak with 2 grace days — missing a day doesn't reset. (Reduces guilt spiral while preserving habit formation)
4. **Context-aware triggers:** notifications at user-chosen time, not app-chosen. User picks their moment. (Autonomy + Fogg)
5. **Choice of path:** three starter topics the learner picks from (travel / work / family), not forced sequence. (Autonomy)
6. **Social proof factual:** "236 people are learning Spanish this week" — no comparison, no shame.

### Explicitly Rejected
- Leaderboards — shift intrinsic motivation external, harm long-term learning
- Streak punishment — creates anxiety, not learning
- Push notifications at high frequency — app fatigue
- Badges for badge's sake — "you earned the 'Three Words' badge" cheapens the actual accomplishment
- "People are disappearing from your class!" fabricated urgency

### Measurement Plan
- Primary: day-14 return rate (current ~40%, target 60%)
- Guard: week-6 actual speaking ability (to catch if we boosted engagement without learning)
- Watch for: users reporting feeling "anxious" to open the app — that's the streak mechanic backfiring

---

## Known Limitations

1. **Engagement ≠ learning.** Mechanics can improve app opens without improving actual skill. Always measure both.

2. **SDT-based mechanics are slower to show effect than manipulative ones.** Badges produce quick behaviour; intrinsic support produces slower but deeper change. Don't over-index on short-term metrics.

3. **Children and adults respond differently.** Children can tolerate more overt gamification; adults often resent it. Calibrate.
