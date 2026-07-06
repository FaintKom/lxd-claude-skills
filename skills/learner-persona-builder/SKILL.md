---
# AGENT SKILLS STANDARD FIELDS (v2)
name: learner-persona-builder
description: "Build detailed learner personas for course design, tailored for either adults (career changers, reskilling, professional learning) or children (new skills, developmental context). Outputs prior knowledge, motivations, anxieties, time constraints, identity concerns, and design implications. Use at the start of course design (ADDIE Analyze phase) or when a course is underperforming and you suspect audience mismatch."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "instructional-design/learner-persona-builder"
skill_name: "Learner Persona Builder"
domain: "instructional-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Knowles, Holton & Swanson (2020) — The Adult Learner (9th ed.): andragogy and adult learning principles"
  - "Dirksen (2015) — Design for How People Learn (2nd ed.): persona-driven design"
  - "Mezirow (1991) — Transformative Learning: identity in adult career change"
  - "Vygotsky (1978) — Mind in Society: developmental context for children"
  - "Dweck (2006) — Mindset: fixed vs growth mindset across ages"
  - "Cooper (1999) — The Inmates Are Running the Asylum: origin of persona method"
input_schema:
  required:
    - field: "audience_description"
      type: "string"
      description: "Who the course is for — age, context, goal"
    - field: "course_topic"
      type: "string"
      description: "What the course teaches"
  optional:
    - field: "known_audience_data"
      type: "string"
      description: "Real data you have from past learners, interviews, surveys"
    - field: "persona_count"
      type: "string"
      description: "How many personas to produce (default: 2–3 distinct ones)"
output_schema:
  type: "object"
  fields:
    - field: "personas"
      type: "array"
      description: "Each persona with name, context, prior knowledge, motivation, barriers, identity, design implications"
    - field: "design_implications"
      type: "string"
      description: "Concrete design decisions driven by the personas"
    - field: "assumptions_to_validate"
      type: "string"
      description: "What parts of the personas are hypotheses to confirm with real learners"
chains_well_with:
  - "addie-course-designer"
  - "training-needs-analyzer"
  - "sam-iterative-course-prototyper"
  - "action-mapping-designer"
  - "differentiation-adapter"
teacher_time: "5 minutes"
tags: ["personas", "audience-analysis", "adult-learning", "andragogy", "instructional-design"]
---

# Learner Persona Builder

## What This Skill Does

Generates 2–3 distinct learner personas tailored to the course audience and topic. Each persona includes concrete details on prior knowledge, motivation, anxieties, time constraints, and identity concerns — plus explicit design implications ("because of this persona, the course should..."). For adults, the skill draws on andragogy (Knowles) and identity frameworks (Mezirow) because adult learners bring life experience, self-concept, and often emotional stakes that children do not. For children, the skill draws on developmental context and motivation research. AI is valuable here because most course creators write one vague average learner and miss the variance that determines whether the course works for the edges of the audience.

## Evidence Foundation

Knowles' andragogy (Knowles, Holton & Swanson, 2020) argues adults learn differently than children in at least six ways: they need to know WHY they're learning; they bring life experience as a resource; they are self-directed; they are motivated by immediate problems; they are motivated internally more than externally; and their identity matters in what they learn. Mezirow (1991) extends this: for adults in career change or major transitions, learning often requires "perspective transformation" — revisiting assumptions about self — which triggers significant anxiety. Vygotsky (1978) established that children's learning is fundamentally mediated by social context and operates within a Zone of Proximal Development — what they can do with help but not alone. Dweck (2006) shows that fixed vs growth mindset is significant across ages and affects persistence. Cooper (1999) introduced personas as a design method; its use in instructional design is widespread though empirical evidence specifically for persona-driven course design is limited — the value is in forcing designers to consider variance rather than designing for an average. Dirksen (2015) provides the most practical adaptation to learning design.

## Input Schema

- **Audience description:** *e.g. "Adults 25–40 switching from marketing to data analytics"* or *"Children 9–11 learning to cook"*
- **Course topic:** *e.g. "SQL fundamentals"*
- Optional: known audience data, persona count

## Prompt

```
You are an expert instructional designer trained in andragogy (Knowles, Holton & Swanson, 2020), transformative learning (Mezirow, 1991), developmental learning theory (Vygotsky, 1978), mindset research (Dweck, 2006), and persona design (Cooper, 1999; Dirksen, 2015). You build learner personas that capture the variance that actually matters for course design, not demographic stereotypes.

Your task is to build learner personas for:

**Audience:** {{audience_description}}
**Course topic:** {{course_topic}}

Optional:
**Known data:** {{known_audience_data}}
**Persona count:** {{persona_count}} (default 2–3)

Build 2–3 DISTINCT personas that capture the variance within the audience. Not "average learner + optimistic learner + pessimistic learner" — look for structurally different learner situations that would require different design decisions.

For each persona, include:

**Name and one-line identity**
- A concrete name and a one-line description the designer can keep in mind.

**Context**
- Age, role or life stage, relevant background. For adults: work context, family, financial pressure. For children: family setup, prior exposure, developmental stage.

**Prior knowledge**
- What do they already know about the topic? What related things do they know? What might they wrongly think they know?
- Be specific. "Some Excel" is vague; "uses filters and pivot tables but has never written a formula with INDEX/MATCH" is useful.

**Motivation**
- For adults: what's driving this? Career change? Performance review pressure? Curiosity? Identity shift? External (boss said) or internal (I want)?
- For children: who signed them up? What do THEY want out of it (not what parents want)? What would make it fun?
- Address Knowles: adults need to know WHY. What is this persona's "why"?

**Anxieties and barriers**
- For adults: impostor syndrome, "I'm too old", fear of looking stupid, time pressure, fear of failing in front of colleagues, financial risk of career change.
- For children: fear of being laughed at, comparison to siblings, short attention, hunger/tiredness, no quiet space at home.
- These are NOT character flaws — they are real constraints that the course must acknowledge or work around.

**Identity concerns (adults especially)**
- Mezirow: for career changers, learning often threatens self-concept. "I'm not a tech person" is a belief that must be addressed.
- "Who will I be if I succeed? Who will I be if I fail?" The course interacts with identity.

**Time and life constraints**
- How many hours per week can this persona actually spend? When (mornings, evenings, weekends)?
- What competes for that time?

**Mindset signal**
- Are there early signals that distinguish fixed-mindset vs growth-mindset behaviour? How will the course support growth-mindset language?

**Design implications**
- The most important section. "Because of this persona, the course should ___." Be concrete.
- Minimum 3 implications per persona.

After the personas, produce:

**Design implications (combined)**
- Synthesise: what must the course do to serve all personas? Where do they diverge (and require differentiation)?

**Assumptions to validate**
- Which parts of these personas are hypotheses? Flag them. Before cohort 1, talk to 3 real people matching each persona to check.

Return output in this structure:

## Personas for [Course Name]

### Persona 1: [Name]
[All sections above]

### Persona 2: [Name]
[All sections above]

### Persona 3: [Name] (if applicable)
[All sections above]

### Combined Design Implications
[Synthesised decisions]

### Assumptions to Validate
[What to check with real learners]

**Self-check before returning:** Verify (a) personas are structurally distinct, not variations on a theme, (b) design implications are concrete enough to change a course decision, (c) for adult audiences identity is addressed, (d) for children developmental context is addressed, and (e) you flagged which parts are hypotheses to validate.
```

## Example Output (abridged)

**Scenario:** *Audience: "Adults 25–40 switching from marketing/HR to data analytics" / Topic: "SQL fundamentals"*

### Persona 1: Maria, 32 — "The Quiet Quitter"
- **Context:** Marketing manager 6 years, tired of endless content calendars, two young kids, partner earns more (safety net), looking for remote-friendly work.
- **Prior knowledge:** Power user of Excel pivot tables and VLOOKUP; knows what a database is conceptually; has never written code. Thinks "SQL is a programming language" (partly wrong).
- **Motivation:** Wants interesting work, flexibility, identity shift from "marketing person" to "data person". Why = lifestyle + meaning, not salary.
- **Anxieties:** "I'm not a math person." Fear of being slow compared to younger bootcamp students. Time anxiety — can only study after kids are asleep (9–11pm).
- **Identity:** Significant. Feels like "betraying" her marketing self. Needs to see other marketers in data to believe it's possible.
- **Time:** 6–8 hours/week, mostly 9–11pm, inconsistent (kids get sick).
- **Mindset signal:** Uses "I can't do math" early — fixed mindset language. Respond with "SQL is not math, it's asking questions."
- **Design implications:**
  1. Offer recorded sessions — can't commit to live events
  2. Use marketing datasets in early examples (familiar domain)
  3. Show "career changer graduates" testimonials prominently
  4. Reframe "SQL is math" misconception in Week 1
  5. Build weekly check-in to catch Maria before she drops out quietly

### Persona 2: Dmitry, 28 — "The Desperate Pivot"
- **Context:** Junior HR specialist, unhappy with career, watches YouTube tutorials obsessively, active on career-change forums, no kids, lives with parents.
- **Prior knowledge:** Watched 3 free SQL courses — knows SELECT/WHERE in theory but cannot write one under pressure. Overestimates his readiness.
- **Motivation:** Escape current job; urgency high. Why = financial + freedom. External pressure (family questions his HR job).
- **Anxieties:** Impostor syndrome if he fails after "trying AI, tried crypto, now trying data". Running out of attempts in his own head.
- **Identity:** Already detached from HR self, looking for a new story.
- **Time:** 15+ hours/week, evenings, high consistency initially, risks burnout by week 4.
- **Mindset signal:** Comparative — checks leaderboards, asks "is this enough to get a job?" constantly. Reassurance-seeking.
- **Design implications:**
  1. Early performance task so Dmitry's self-assessment confronts reality
  2. Anti-burnout pacing — cap recommended weekly hours
  3. Clear milestones ("you are now at week 3 level") to satisfy reassurance need
  4. Community access to reduce isolation

### Persona 3: Anna, 38 — "The Reluctant Reskiller"
- **Context:** Senior HR business partner, her company announced layoffs affecting her department, 90-day notice, high pressure, older teenage kids.
- **Prior knowledge:** Strong with people, zero technical background, hasn't written anything code-like in 15+ years.
- **Motivation:** Survival. External pressure (layoff) → immediate need. Why = income replacement.
- **Anxieties:** Age-related imposter syndrome ("too old for this"), financial fear, shame.
- **Identity:** Major threat. "I was a leader — now I'm a student again." Humiliation risk.
- **Time:** Variable — currently 20h/week while on notice, drops to 0 if she gets hired in a non-data role.
- **Mindset signal:** Grit present but fragile — any early failure could confirm "I can't do this."
- **Design implications:**
  1. Explicitly address "too old" fear in Week 1 (stats, examples)
  2. Design for fast early wins — first query in first 20 minutes
  3. Respect professional identity — don't infantilise, use peer language
  4. Offer "quick path" for those who need to finish in 4 weeks, not 8

### Combined Design Implications
- All three need async delivery (live-only would lose Maria and Anna)
- All three need early confidence wins (different reasons, same need)
- Two need identity-affirming messaging (Maria, Anna)
- One needs pace containment (Dmitry)
- Marketing/HR datasets in examples serve two of three directly
- Weekly check-in mechanism must exist — silence = dropout risk

### Assumptions to Validate
- Do marketing professionals actually identify as "not math people" at this rate? (Check with 3 real marketers before launch.)
- Is the 9–11pm time constraint common for parent-learners? (Survey current waitlist.)
- Does age-related imposter syndrome manifest the way Persona 3 suggests? (Interview 2–3 women over 35 who've done career changes.)

---

## Known Limitations

1. **Personas are hypotheses until validated.** Treat them as design inputs to test, not ground truth. Get real learner interviews as soon as possible.

2. **Three personas will not capture everyone.** They are a sampling of structural variance, not a taxonomy. Design should serve the middle and the hardest persona.

3. **For children, parents are a secondary audience.** Parents choose the course; children experience it. Consider both, but don't confuse them.

4. **Personas can stereotype if you are not careful.** Avoid demographic shortcuts ("women are more anxious"). Ground every claim in the actual learning situation, not identity category.
