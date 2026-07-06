---
# AGENT SKILLS STANDARD FIELDS (v2)
name: merrill-first-principles-lesson-designer
description: "Design a lesson using Merrill's Five First Principles of Instruction: problem-centred, activation of prior experience, demonstration, application, integration. Use when you want a research-derived, problem-first lesson structure — especially good for skill-based learning where the goal is real-world transfer, not content coverage."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "instructional-design/merrill-first-principles-lesson-designer"
skill_name: "Merrill First Principles Lesson Designer"
domain: "instructional-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Merrill (2002) — First Principles of Instruction, Educational Technology Research and Development 50(3)"
  - "Merrill (2013) — First Principles of Instruction: Identifying and Designing Effective, Efficient, and Engaging Instruction"
  - "Frick, Chadha, Watson, Wang, & Green (2009) — Evaluation of a key scale for measuring the First Principles of Instruction"
  - "Lo & Hew (2017) — A critical review of flipped classroom challenges in K-12 education: the role of First Principles"
input_schema:
  required:
    - field: "target_skill"
      type: "string"
      description: "The real-world skill or task learners need to be able to do"
    - field: "learner_profile"
      type: "string"
      description: "Age/level/prior experience"
    - field: "duration"
      type: "string"
      description: "Total lesson time"
  optional:
    - field: "authentic_problem"
      type: "string"
      description: "A specific real-world problem that requires the skill"
    - field: "common_errors"
      type: "string"
      description: "Typical mistakes or misconceptions"
output_schema:
  type: "object"
  fields:
    - field: "problem_progression"
      type: "array"
      description: "Sequence of increasingly complex real-world problems"
    - field: "activation_plan"
      type: "string"
      description: "How learners connect to prior experience"
    - field: "demonstration_plan"
      type: "string"
      description: "How the skill is shown (worked examples, modelling)"
    - field: "application_plan"
      type: "string"
      description: "Guided → independent practice with feedback"
    - field: "integration_plan"
      type: "string"
      description: "Transfer to own context, reflection, peer teaching"
chains_well_with:
  - "gagne-nine-events-lesson-designer"
  - "action-mapping-designer"
  - "addie-course-designer"
  - "worked-example-fading-designer"
  - "retrieval-practice-generator"
teacher_time: "4 minutes"
tags: ["Merrill", "first-principles", "problem-centred", "lesson-design", "instructional-design"]
---

# Merrill First Principles Lesson Designer

## What This Skill Does

Generates a lesson or short unit structured around Merrill's Five First Principles of Instruction: (1) learning is promoted when learners work on real-world problems; (2) learning is promoted when existing knowledge is activated as a foundation for new knowledge; (3) learning is promoted when new knowledge is demonstrated; (4) learning is promoted when new knowledge is applied by the learner; (5) learning is promoted when new knowledge is integrated into the learner's world. Merrill distilled these from a review of the major instructional design theories and identified the common core. Unlike Gagné's linear nine events, Merrill's principles centre on a problem progression — learners work on a sequence of increasingly difficult authentic problems, with activation, demonstration, application, and integration supporting each problem.

## Evidence Foundation

M. David Merrill (2002, 2013) reviewed major instructional design theories (Gagné, van Merriënboer, Schank, Jonassen, Clark, Collins, and others) and identified five principles that appear across all effective instructional approaches. Rather than proposing a new theory, Merrill argued these are the "first principles" — the minimum core any effective instruction must honour. Frick et al. (2009) developed and validated a measurement scale showing that lessons rated higher on the First Principles produced measurably better learning outcomes. The problem-centred principle is the most distinctive: Merrill is explicit that content-first or topic-first designs consistently underperform problem-first designs for skill transfer. The principles align with Cognitive Apprenticeship, 4C/ID (four-component instructional design, van Merriënboer), and goal-based scenarios.

## Input Schema

- **Target skill:** *e.g. "Cook a simple meal safely alone"*
- **Learner profile:** *e.g. "Children 9–11, first cooking experience"*
- **Duration:** *e.g. "4 weekly sessions × 60 min"*
- Optional: authentic problem, common errors

## Prompt

```
You are an instructional designer trained in Merrill's First Principles of Instruction (Merrill, 2002; 2013). You design lessons around real-world problem progressions, not content topics.

Your task is to design a lesson/mini-unit for:

**Target skill:** {{target_skill}}
**Learner profile:** {{learner_profile}}
**Duration:** {{duration}}

Optional:
**Authentic problem:** {{authentic_problem}}
**Common errors:** {{common_errors}}

Apply the five First Principles in order:

**Principle 1 — Problem-centred.**
- Define the real-world task the learner should be able to do. Not a topic ("kitchen safety") but a task ("prepare one simple meal alone").
- Design a progression of 3–5 problems of increasing complexity. The first problem is simple enough to succeed with basic scaffolding; the last approaches real-world complexity.
- Every lesson segment is in service of solving one of these problems.

**Principle 2 — Activation of prior experience.**
- How will you bring learners' existing knowledge to the surface? Specific question, recall of a related experience, analogy, or simple diagnostic task.
- For adults: usually work/life experience is abundant — explicitly connect to it.
- For children: prior experience is narrower — use family/home/play analogies.

**Principle 3 — Demonstration.**
- Show how the skill is performed. Worked examples, think-aloud modelling, video of expert performance, step-by-step walkthrough.
- Demonstration must show the skill in context — solving an actual problem, not abstract rules.
- Make reasoning visible: "I am checking X because Y."

**Principle 4 — Application.**
- Learners solve a problem themselves. Start with heavy scaffolding and fade it. Coaching, hints, corrective feedback.
- Feedback should be specific and timely, naming both what is right and what to fix.
- Move through the problem progression: each application problem is harder than the last, with less scaffolding.

**Principle 5 — Integration.**
- Learner transfers the skill to their own context. "Where could you use this in your own life? Show me."
- Public demonstration or teaching to a peer strengthens integration.
- Reflection: what was hard, what is still unclear, what will you try next?

Return your output in this structure:

## Lesson/Unit: [Skill Name]

**For:** [learner profile]
**Duration:** [total]

### Problem Progression (Principle 1)
[3–5 problems, listed simple-to-complex. For each: what is the problem, why it's a step up from the previous, what success looks like.]

### Session Plan
[For each session or segment:]
- **Activation** (Principle 2): concrete activity
- **Demonstration** (Principle 3): concrete activity with visible reasoning
- **Application** (Principle 4): problem the learner solves, scaffolding level, feedback method
- **Integration** (Principle 5): transfer + reflection + peer sharing

### Scaffolding Fade Plan
[How scaffolding decreases across the problem progression — what is supported in problem 1 that must be independent by problem 5.]

**Self-check before returning:** Verify (a) there is a concrete problem progression, not a topic list, (b) all five principles appear in each session, (c) demonstration shows reasoning, not just procedure, (d) scaffolding actually fades across problems, and (e) integration produces transfer to the learner's own context, not just practice within the course.
```

## Example Output (abridged)

**Scenario:** *Skill: "Cook one simple meal alone" / Learners: children 9–11, first cooking / 4 × 60 min*

### Problem Progression
1. **Problem 1:** Make a cold breakfast (yogurt + berries + granola). No stove, no knife, no heat. Success: eaten, no mess left.
2. **Problem 2:** Make buttered toast with jam. Uses toaster (heat), butter knife. Success: no burn, toast not burned.
3. **Problem 3:** Make scrambled eggs. Uses stove, pan, whisking, timing. Success: eggs cooked, pan cleaned.
4. **Problem 4:** Make pasta with simple tomato sauce. Uses boiling water, timing two things at once, tasting. Success: pasta cooked, sauce warm, served hot.
5. **Problem 5 (integration):** Choose and prepare any simple meal at home, alone, photograph the result. Success: child names what they made and why they chose it.

### Session 1 (Problem 1 — cold breakfast)

- **Activation:** "What have you eaten this week that had no cooking? Tell me one thing." Connects to existing breakfast habits.
- **Demonstration:** Teacher prepares one bowl on camera, thinking aloud: "I'm washing my hands first because my hands touched everything. I'm putting yogurt in first because it's the base. I'm adding berries on top because they look nice."
- **Application:** Each child prepares their own bowl at home on camera. Teacher gives feedback: "Good hand wash. Next time try tilting the spoon away from you."
- **Integration:** "Next time you want a snack this week, could you do this yourself? Send me a photo." Reflection: one thing that felt easy, one thing that felt weird.

### Session 3 (Problem 3 — scrambled eggs)

- **Activation:** "Last week you made toast with the toaster. The stove is different — it stays hot. What is scary about a stove? What is safe?"
- **Demonstration:** Watch a slow-motion video of eggs cooking. Teacher narrates: "See how they go from runny to firm? I'm stirring so they don't burn on the bottom. I turn off the heat BEFORE they're fully done because they keep cooking."
- **Application:** Child cooks eggs while parent watches. Teacher joins live if possible. Feedback: stove control, stirring technique, knowing when to stop.
- **Integration:** "When could you make eggs this week on your own? Tell a family member you'd like to try."

### Scaffolding Fade

| Problem | Teacher on call? | Adult watching? | Written steps? | Time support? |
|---|---|---|---|---|
| 1 Cold breakfast | Yes | Yes | Full checklist | None |
| 2 Toast | Yes | Yes | Short list | None |
| 3 Eggs | Live joins | Yes | Key reminders | Teacher calls out stages |
| 4 Pasta | On standby | Near | Ingredient list only | Child uses own timer |
| 5 Free choice | Not expected | Background | Child plans | Child decides |

---

## Known Limitations

1. **Problem progression is hard for abstract topics.** For pure knowledge domains (history dates, scientific vocabulary), forcing a problem-centred structure can feel artificial. Still possible, but look for "use the knowledge" problems (explain, argue, predict).

2. **Principle 5 (integration) is the most skipped.** Without explicit transfer to the learner's own context, skills decay. Do not treat integration as optional.

3. **Scaffolding fade must be planned, not improvised.** The default is to keep scaffolding constant — be deliberate about what you are removing and when.

4. **For very short lessons** (<30 min), a full problem progression is unrealistic. Use one problem with the five principles around it; chain lessons together for progression.
