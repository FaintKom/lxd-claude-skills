---
# AGENT SKILLS STANDARD FIELDS (v2)
name: microlearning-architect
description: "Design a microlearning path: break a larger topic into 3–7 minute self-contained learning objects, sequence them with spaced practice, and build the connective tissue (prerequisites, assessments, retrieval) that makes short bursts add up to real skill. Use when building mobile-friendly, on-demand, or just-in-time learning for busy adults or short-attention children, or when long courses show high dropout mid-way."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/microlearning-architect"
skill_name: "Microlearning Architect"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Torgerson & Iannone (2019) — Designing Microlearning (ATD Press)"
  - "Hug (2005) — Micro Learning and Narration (original theoretical framing)"
  - "Kapp & Defelice (2019) — Microlearning: Short and Sweet"
  - "Thalheimer (2006) — Spacing learning events over time (evidence base for microlearning's spacing claims)"
  - "Giurgiu (2017) — Microlearning an evolving elearning trend (empirical review)"
input_schema:
  required:
    - field: "topic"
      type: "string"
      description: "What the overall content is about"
    - field: "target_skill_or_knowledge"
      type: "string"
      description: "What learners should be able to do or know"
    - field: "audience"
      type: "string"
      description: "Who will consume it, and their time/context constraints"
    - field: "delivery_channel"
      type: "string"
      description: "Mobile app, email drip, LMS, Slack, SMS, etc."
  optional:
    - field: "available_time_per_session"
      type: "string"
      description: "How much time learners have per micro-unit (default 3–7 min)"
    - field: "constraints"
      type: "string"
      description: "Tools, budget, existing content to repurpose"
output_schema:
  type: "object"
  fields:
    - field: "micro_unit_inventory"
      type: "array"
      description: "Each micro-unit: title, objective, duration, format, assessment, prerequisites"
    - field: "sequence_and_spacing"
      type: "object"
      description: "Order, spacing schedule, branching/paths if any"
    - field: "connective_tissue"
      type: "string"
      description: "How units connect — entry, recap, retrieval, progress signals"
    - field: "anti_patterns_avoided"
      type: "string"
      description: "Microlearning traps the design deliberately avoids"
chains_well_with:
  - "spaced-practice-scheduler"
  - "retrieval-practice-generator"
  - "mayer-multimedia-principles-designer"
  - "mobile-first-learning-designer"
  - "action-mapping-designer"
  - "cognitive-load-analyser"
teacher_time: "5 minutes"
tags: ["microlearning", "bite-sized", "mobile-learning", "spacing", "learning-experience-design"]
---

# Microlearning Architect

## What This Skill Does

Designs a path of short (3–7 minute) self-contained learning units that together produce a real skill or knowledge outcome. Microlearning fails when it is treated as "chunked long course" — the result is fragmented content without the glue of prerequisite structure, retrieval, or spaced practice. This skill produces a micro-unit inventory, a sequence with spacing schedule, and the connective tissue (recaps, retrieval prompts, progress markers) that distinguishes real microlearning from "a long course cut into slices."

## Evidence Foundation

Microlearning as a deliberate design approach is more recent than its marketing suggests, but its underlying mechanisms have strong evidence: spacing effect (Thalheimer, 2006; Cepeda et al., 2006), retrieval practice (Roediger & Karpicke, 2006), and limited working memory (Sweller et al., 2011). Torgerson & Iannone (2019) provide the most rigorous practitioner framing, explicitly distinguishing microlearning from "short videos" — real microlearning requires (a) learning objects that are self-contained, (b) each tied to a specific objective, (c) spaced and revisited over time, (d) combined with retrieval. Kapp & Defelice (2019) document both successes and failures: microlearning works for specific, application-focused objectives but fails for complex conceptual learning. Giurgiu (2017) reviews the empirical literature and concludes evidence is "moderate and growing" — it works when designed around learning science, not when used as a buzzword.

## Input Schema

- **Topic:** *e.g. "Customer objection handling for SaaS sales reps"*
- **Target skill:** *e.g. "Respond to common objections with a structured framework"*
- **Audience:** *e.g. "New sales reps, 0–6 months, between customer calls, mobile"*
- **Delivery channel:** *e.g. "Slack + mobile web"*
- Optional: session time, constraints

## Prompt

```
You are an expert in microlearning design, trained in Torgerson & Iannone (2019), Kapp & Defelice (2019), and the underlying spacing/retrieval evidence (Thalheimer, 2006; Roediger & Karpicke, 2006). You design real microlearning, not "short videos". You refuse to design microlearning for content that needs long-form treatment.

Your task:

**Topic:** {{topic}}
**Target skill/knowledge:** {{target_skill_or_knowledge}}
**Audience:** {{audience}}
**Delivery channel:** {{delivery_channel}}
**Session time:** {{available_time_per_session}}
**Constraints:** {{constraints}}

**Step 0 — Is this actually microlearnable?**
- Microlearning works for: specific application skills, vocabulary/facts, procedure steps, decision rules, just-in-time reference, habit reinforcement.
- Microlearning fails for: complex conceptual understanding, problem-solving in novel domains, topics where prerequisites interact heavily, deep mindset shifts.
- If the target is in the "fails" category, say so and recommend a different format. Do not force microlearning onto unsuitable content.

**Step 1 — Decompose into self-contained units.**
- Each unit must have ONE objective. If it has two, split it.
- Each unit must be consumable in the stated session time without external prerequisites (or the prerequisites are themselves earlier units).
- Each unit must produce an observable behaviour or testable knowledge output.

**Step 2 — Sequence and space.**
- Order units by prerequisite dependency.
- Add spacing: Unit 2 does not follow Unit 1 the next minute; it follows after a delay that allows forgetting to begin (hours to days depending on context).
- Include spaced retrieval of earlier units: Unit 5 should ask a retrieval question from Unit 1.

**Step 3 — Design the connective tissue.**
- **Entry cue:** what prompts the learner to start a unit? (Slack message, app notification, calendar nudge, just-in-time trigger.)
- **Recap:** each unit starts with a 10-second retrieval from the previous unit, NOT a summary.
- **Progress signal:** learner can see where they are in the path without opening a dashboard.
- **Exit cue:** each unit ends with a commitment — "try X in your next call" — not "see you tomorrow".

**Step 4 — Format each unit.**
- Mix formats to match content and context: quick video, text + image, interactive choice, mini-scenario, flashcard drill, voice note, practice task.
- Do NOT default to video for everything.

**Step 5 — Assessment and measurement.**
- Every 3–4 units, include a low-stakes performance check that is retrieval, not recognition.
- Track completion, but primarily track whether the behaviour shows up in the real context.

**Step 6 — Anti-patterns to explicitly avoid.**
- Long course sliced into small pieces with no retrieval between
- Videos with no practice
- All units consumed in one sitting (defeats spacing)
- Text-heavy units on mobile
- Units without a clear next action
- Platform lock-in preventing just-in-time access

Return output in this structure:

## Microlearning Path: [Topic]

### Suitability Check
[Is this microlearnable? Reasoning.]

### Micro-Unit Inventory

| # | Title | Objective | Time | Format | Prerequisite | Assessment |
|---|---|---|---|---|---|---|
[Row per unit]

### Sequence and Spacing
[Order + spacing schedule: Unit 1 day 0, Unit 2 day 1, Unit 3 day 3, retrieval of 1 in unit 4, etc.]

### Connective Tissue
- Entry cue: [...]
- Recap format: [...]
- Progress signal: [...]
- Exit commitment: [...]

### Anti-Patterns Avoided
[Specific traps this design avoids, with reasoning]

**Self-check before returning:** Verify (a) Step 0 was actually performed (you considered whether microlearning is appropriate), (b) each unit has exactly one objective, (c) spacing is built in (not all units back-to-back), (d) retrieval of earlier units appears in later units, and (e) format is varied, not all video.
```

## Example Output (abridged)

**Scenario:** *Customer objection handling for SaaS reps / Delivery: Slack / Session: 3–5 min between calls*

### Suitability Check
Fits: objection handling is procedural + decision-based + practiced in discrete moments. ✅

### Micro-Unit Inventory (abridged)

| # | Title | Objective | Time | Format | Prerequisite | Assessment |
|---|---|---|---|---|---|---|
| 1 | Recognise the 5 common objections | Name them when heard | 3 min | Card-flip drill | — | Match 5/5 |
| 2 | "Too expensive" response framework | Use Acknowledge → Reframe → Evidence | 5 min | Scripted scenario | 1 | Type response |
| 3 | "We already have X" | Use same framework, specific pivot | 4 min | Voice note + reply | 1, 2 | Voice response |
| 4 | Retrieval: which objection? | Name it in 5 sec | 2 min | Audio clip quiz | 1 | Retrieve from 1 |
| 5 | "Send me more info" (hiding) | Qualify vs drop | 5 min | Branching scenario | 1–3 | Choose path |

### Sequence and Spacing
- Day 0 morning: Unit 1
- Day 0 after first call: Unit 2
- Day 1: Unit 3
- Day 2: Unit 4 (retrieval of 1)
- Day 3: Unit 5

### Connective Tissue
- **Entry cue:** Slack DM after rep's 2nd call of the day
- **Recap:** first 10 sec = retrieval question from previous unit
- **Progress:** "3 of 7" in Slack message
- **Exit commitment:** "Try this on your next call and reply with what happened"

### Anti-Patterns Avoided
- Not "sales training course in slices" — each unit teaches one objection
- Not all video — mix text, audio, scenario
- Not all in one day — spacing preserved
- Not optional retrieval — unit 4 exists specifically for it

---

## Known Limitations

1. **Microlearning without spacing is just short content.** If learners consume all units back-to-back, most evidence-based benefits disappear. Platform/delivery must enforce or encourage spacing.

2. **Not suitable for complex conceptual learning.** Forcing it produces superficial understanding. Use other formats for concepts.

3. **Measurement is harder than in long courses.** Per-unit completion is easy; behaviour change in the real context is the real metric and requires external data.
