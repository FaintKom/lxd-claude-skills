---
# AGENT SKILLS STANDARD FIELDS (v2)
name: action-mapping-designer
description: "Design a workplace or professional training course using Cathy Moore's Action Mapping: start from a measurable business or life goal, identify the specific behaviours that would achieve it, then design practice activities around those behaviours (not content dumps). Use when building courses for adults changing careers, onboarding, professional skills, or any training where 'what learners will DO differently' matters more than 'what they will know'."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "instructional-design/action-mapping-designer"
skill_name: "Action Mapping Designer"
domain: "instructional-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Moore (2017) — Map It: The Hands-On Guide to Strategic Training Design"
  - "Moore — Action Mapping process (https://blog.cathy-moore.com/action-mapping-a-visual-approach-to-training-design/)"
  - "Thalheimer (2006) — Spacing learning events over time"
  - "Clark & Mayer (2016) — e-Learning and the Science of Instruction (relevant to practice-first design)"
  - "Dirksen (2015) — Design for How People Learn: behaviour-first instructional design"
input_schema:
  required:
    - field: "measurable_goal"
      type: "string"
      description: "The business or life goal the training must move — must be observable and ideally quantifiable"
    - field: "target_audience"
      type: "string"
      description: "Who is doing the behaviour"
  optional:
    - field: "current_performance"
      type: "string"
      description: "What people are doing now (the baseline)"
    - field: "known_barriers"
      type: "string"
      description: "What stops people doing the right thing (knowledge, skill, motivation, environment, tools)"
    - field: "constraints"
      type: "string"
      description: "Time, budget, platform, compliance requirements"
output_schema:
  type: "object"
  fields:
    - field: "measurable_goal"
      type: "string"
      description: "Restated and sharpened goal"
    - field: "actions"
      type: "array"
      description: "Specific observable behaviours that would achieve the goal"
    - field: "action_filters"
      type: "object"
      description: "Which actions are knowledge/skill gaps (needing practice) vs environment/motivation (needing non-training fixes)"
    - field: "practice_activities"
      type: "array"
      description: "For each training-relevant action, a realistic practice activity that mirrors the real context"
    - field: "minimum_content"
      type: "string"
      description: "The smallest amount of content needed to support the practice activities"
    - field: "not_training_recommendations"
      type: "string"
      description: "Non-training fixes for barriers that training cannot solve"
chains_well_with:
  - "training-needs-analyzer"
  - "learner-persona-builder"
  - "kirkpatrick-evaluation-planner"
  - "addie-course-designer"
  - "sam-iterative-course-prototyper"
teacher_time: "6 minutes"
tags: ["action-mapping", "Cathy-Moore", "behaviour-first", "workplace-training", "instructional-design"]
---

# Action Mapping Designer

## What This Skill Does

Generates an Action Map: a behaviour-first course design that starts from a measurable goal, identifies the specific observable actions that would achieve it, separates knowledge/skill gaps (which training can fix) from motivation/environment gaps (which it cannot), and designs realistic practice activities rather than content dumps. The output is deliberately content-minimal — only the content needed to support practice is included. AI is valuable here because most course designers default to "content first" thinking and struggle to stay behaviour-first without a disciplined process.

## Evidence Foundation

Cathy Moore (2017) developed Action Mapping as a reaction to the dominant "content dump" approach in workplace training. The core insight is that training often fails not because content is missing but because the design focuses on transmitting information rather than producing behaviour change. Action Mapping draws on behaviour analysis, human performance technology (Gilbert, 1978), and the distinction between skill gaps and environment/motivation gaps. Thalheimer (2006) shows that spaced, realistic practice outperforms massed content exposure. Clark & Mayer (2016) demonstrate that practice with feedback is the single most powerful element of effective e-learning, and that content without practice produces minimal behaviour change. Dirksen (2015) independently argues for behaviour-first design. Action Mapping's strongest evidence base is in corporate and professional training for adults; it is equally applicable to skill-based courses for children, though the behaviours are framed differently.

## Input Schema

Required:
- **Measurable goal:** *e.g. "Junior data analysts pass the SQL portion of their first technical interview" or "Children independently prepare a simple meal at home at least once a week"*
- **Target audience:** *e.g. "Adults 25–40 switching into data roles" or "Children 9–11, first cooking experience"*

Optional:
- **Current performance, known barriers, constraints**

## Prompt

```
You are an expert workplace/skills instructional designer trained in Cathy Moore's Action Mapping (Moore, 2017). You design courses around observable behaviours, not content. You refuse to include content that does not directly support a specific action learners need to perform.

Your task is to design an action map for:

**Measurable goal:** {{measurable_goal}}
**Target audience:** {{target_audience}}

Optional:
**Current performance:** {{current_performance}}
**Known barriers:** {{known_barriers}}
**Constraints:** {{constraints}}

Apply Action Mapping with discipline:

**Step 1 — Sharpen the goal.**
- Restate the goal so it is observable and measurable. Not "understand SQL" but "write a working query answering a business question within 10 minutes." Not "be safe in the kitchen" but "prepare a simple meal without injury or fire, at least once per week."
- If the goal as stated is not measurable, rewrite it. If it cannot be made measurable, flag it.

**Step 2 — Identify actions.**
- List the specific observable behaviours that, if done well, would achieve the goal. These are things learners DO, not things they know.
- Each action must be observable by an outside observer. "Understands X" is NOT an action. "Explains X to a peer in their own words" IS an action.
- 5–10 actions is a normal range. Fewer if the goal is narrow.

**Step 3 — Filter actions: training vs non-training.**
- For each action, ask: why aren't people doing this already? Choose one or more:
  - **Skill gap** (don't know how) → training can help via practice
  - **Knowledge gap** (don't know what/when) → training can help via job aids + minimal practice
  - **Motivation gap** (don't care, scared, no incentive) → training usually cannot fix this; flag for non-training intervention
  - **Environment gap** (tools missing, time blocked, process broken) → training cannot fix this; flag for non-training intervention
- Actions that are ONLY motivation or environment problems must NOT drive course content. Acknowledge them and recommend non-training fixes.

**Step 4 — Design practice activities for training-relevant actions.**
- For each skill/knowledge-gap action, design a realistic practice activity that mirrors the real context. Ask: "What is the context where they actually do this? What does success look like? What mistakes are typical?"
- Practice should include: a realistic scenario, a decision or action the learner must take, specific feedback mapped to common errors.
- Avoid quiz-style "which of these is correct" — prefer open-ended "here is a situation, what do you do?" with feedback.
- Practice difficulty should approach real-world difficulty by the end.

**Step 5 — Identify minimum content.**
- What content MUST exist to support the practice activities? List only that.
- Content that does not directly support a practice activity is cut.
- For adult professional learning, prefer job aids, cheat sheets, and reference material over linear content.

**Step 6 — Flag non-training recommendations.**
- For any motivation/environment gap identified in Step 3, write a concrete non-training recommendation (e.g., "provide a printed SQL reference card", "change the tool to auto-lint queries", "managers should model the behaviour").

Return your output in this structure:

## Action Map: [Goal Title]

### Sharpened Goal
[Measurable restatement]

### Actions
[Numbered list of observable behaviours]

### Filter: Which Actions Need Training?

| # | Action | Skill | Knowledge | Motivation | Environment | Training Can Help? |
|---|---|---|---|---|---|---|
[One row per action]

### Practice Activities
[For each training-relevant action: realistic scenario + decision + feedback on common errors]

### Minimum Content
[Only the content needed to support the practice activities]

### Not-Training Recommendations
[For motivation/environment gaps: concrete non-training fixes]

**Self-check before returning:** Verify (a) the goal is observable and measurable, (b) every action is an observable behaviour (not "understand" or "know about"), (c) motivation/environment gaps are NOT driving course content, (d) every practice activity mirrors the real context with typical mistakes, and (e) content is minimal — only what supports practice.
```

## Example Output (abridged)

**Scenario:** *Goal: "Children 9–11 independently prepare a simple meal at home at least once a week" / Audience: "First-time cooks, home setting, parents supervising initially"*

### Sharpened Goal
Within 8 weeks, each child independently prepares at least one simple meal per week at home, with no injury, no burned food, and reasonable cleanup.

### Actions
1. Read a simple recipe and list what's needed before starting
2. Wash hands and tie back hair before touching food
3. Use a knife to cut soft vegetables safely (claw grip, stable board)
4. Turn stove burners on/off and adjust heat correctly
5. Crack an egg without shell in the bowl
6. Judge when food is "done" using simple signs (colour, smell, texture)
7. Turn off the stove and clean the area at the end
8. Decide when to ask an adult for help vs. handle alone

### Filter

| # | Action | Skill | Know | Motiv | Env | Training? |
|---|---|---|---|---|---|---|
| 1 | Read recipe & prep | ✓ | ✓ | | | Yes — practice |
| 2 | Hygiene before | ✓ | ✓ | | | Yes — habit practice |
| 3 | Knife skills | ✓ | | | | Yes — hands-on practice |
| 4 | Stove control | ✓ | ✓ | ✓ (fear) | | Partly — also needs confidence building |
| 5 | Crack egg | ✓ | | | | Yes — practice |
| 6 | Judge doneness | ✓ | ✓ | | | Yes — practice with feedback |
| 7 | Cleanup | ✓ | | ✓ (boring) | | Partly — also needs motivation |
| 8 | Ask for help | ✓ | | ✓ | | Partly — needs permission culture |

### Practice Activities (abridged)

- **Knife skills:** Provided soft vegetables (banana, boiled potato, cucumber), claw-grip demo, child cuts 10 pieces, adult watches for grip and stability, corrects only these two things. Common errors: fingers flat, board slipping.
- **Stove control:** Child lights burner, boils water, turns off — three times, with adult watching only. Adult names what the child did right. Common errors: leaving the burner on high, walking away.
- **Judging doneness:** Three cooked eggs at different stages (runny, soft, hard) — child labels which is which. Then cooks one themselves aiming for "soft". Feedback: what to look for next time.

### Minimum Content
- One-page illustrated recipe card per target meal (visual, minimal text)
- Knife claw-grip photo
- "Ask for help if…" short list on fridge
- No lecture content. No slides.

### Not-Training Recommendations
- **Motivation (cleanup):** family agreement — "whoever cooks does not clean" (parent cleans)
- **Motivation (stove fear):** parent models confidence, starts with induction if available, celebrates small wins
- **Permission culture (asking for help):** parent explicitly says "asking is smart, not weak" and models asking for help themselves
- **Environment:** step stool, child-sized knife, non-slip board, accessible ingredients

---

## Known Limitations

1. **Action Mapping assumes the goal is legitimately a training problem.** If the real issue is motivation, tools, or process, Action Mapping will correctly refuse to build content — but stakeholders may expect a course anyway. Be prepared to push back.

2. **The "filter" step depends on accurate diagnosis.** If you wrongly label a skill gap as a motivation gap (or vice versa), the map will be wrong. Chain with `training-needs-analyzer` if diagnosis is uncertain.

3. **For knowledge-heavy domains** (e.g., medical terminology), strict behaviour-first design can feel forced. You can still use Action Mapping but acknowledge that retention of foundational vocabulary is a legitimate prerequisite sub-action.

4. **Action Mapping produces lean courses.** Stakeholders used to content-heavy training may perceive the output as "too light." Include a brief explanation of why content was cut.
