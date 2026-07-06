---
# AGENT SKILLS STANDARD FIELDS (v2)
name: learning-narrative-designer
description: "Design a narrative arc for a course or module: protagonist (learner), stakes, inciting incident, rising challenges, turning points, climax, resolution. Use when building courses that need emotional engagement — career-change programs, children's skill courses, anything where 'boring but useful' content needs a story spine to survive."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/learning-narrative-designer"
skill_name: "Learning Narrative Designer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Bruner (1986) — Actual Minds, Possible Worlds: narrative as a mode of thought"
  - "Schank (1995) — Tell Me a Story: Narrative and Intelligence"
  - "Willingham (2009) — Why Don't Students Like School? (story as psychologically privileged format)"
  - "Hamby, Brinberg & Daniloski (2017) — Reducing the narrative transportation-persuasion gap"
  - "Gottschall (2012) — The Storytelling Animal"
  - "Aronica & Robinson (2015) — Creative Schools: narrative-driven learning experiences"
input_schema:
  required:
    - field: "course_topic"
      type: "string"
      description: "What the course teaches"
    - field: "learner_profile"
      type: "string"
      description: "Who they are and what's at stake for them"
    - field: "course_duration"
      type: "string"
      description: "Length of the arc"
  optional:
    - field: "dry_content"
      type: "string"
      description: "Specific topics that feel boring and need narrative scaffolding"
    - field: "tone"
      type: "string"
      description: "Serious / playful / dramatic / warm"
output_schema:
  type: "object"
  fields:
    - field: "narrative_spine"
      type: "object"
      description: "Protagonist, stakes, inciting incident, arc, climax, resolution"
    - field: "module_arc_mapping"
      type: "string"
      description: "Each module's position in the narrative arc"
    - field: "recurring_motifs"
      type: "string"
      description: "Characters, metaphors, symbols that return"
    - field: "emotional_beats"
      type: "string"
      description: "Where frustration, wonder, success, doubt are expected"
chains_well_with:
  - "learner-journey-mapper"
  - "course-onboarding-designer"
  - "flow-state-condition-designer"
  - "awe-wonder-experience-designer"
  - "merrill-first-principles-lesson-designer"
  - "engagement-mechanics-designer"
teacher_time: "5 minutes"
tags: ["narrative", "storytelling", "engagement", "course-arc", "learning-experience-design"]
---

# Learning Narrative Designer

## What This Skill Does

Builds a narrative arc for a course: a protagonist (the learner or a surrogate character), explicit stakes, an inciting incident that pulls them in, rising challenges, turning points, climax, and resolution. Each module in the course is then mapped to a position in the arc so that content that would otherwise feel disconnected becomes part of a coherent journey. Narrative is not decoration — it reduces extraneous cognitive load by giving the learner a "spine" to attach new information to, and it is the most powerful known format for sustained human attention.

## Evidence Foundation

Jerome Bruner (1986) distinguished two "modes of thought": paradigmatic (logical-scientific) and narrative. Most courses default to paradigmatic presentation, which is efficient for experts but alienating for novices. Willingham (2009) argues that the brain treats stories as "psychologically privileged" — better remembered, more easily understood, more engaging — because human cognition evolved in oral storytelling contexts. Roger Schank (1995) proposed that memory itself is fundamentally narrative. Hamby et al. (2017) meta-analyse "narrative transportation" — when learners are absorbed in a story, persuasion and retention both increase. Direct empirical evidence for narrative-framed *courses* is moderate (compared to the strong evidence for story in individual instructional messages), but practitioner evidence is strong, and the underlying mechanisms (attention, retention, meaning-making) have independent support.

## Input Schema

- **Topic:** *e.g. "Intro to public speaking for shy kids 10–12"*
- **Learner profile:** *e.g. "Children who freeze on camera, some have been laughed at before"*
- **Duration:** *e.g. "6 weeks, one session per week"*
- Optional: dry content areas, tone

## Prompt

```
You are a learning experience designer who applies narrative theory (Bruner, 1986; Schank, 1995; Willingham, 2009) to course design. You refuse to treat narrative as decoration — you build it as structural scaffolding that reduces cognitive load and sustains attention.

Your task:

**Topic:** {{course_topic}}
**Learner profile:** {{learner_profile}}
**Duration:** {{course_duration}}
**Dry content:** {{dry_content}}
**Tone:** {{tone}}

Build a narrative spine in this order:

**1. Protagonist**
- Who is the protagonist? Usually it is the learner themselves, framed in second person ("you"). Sometimes it is a surrogate character they follow and identify with (useful for children or when the learner identity is fragile).
- Define the protagonist in one paragraph: who they are at the start, what they believe about themselves, what they can't yet do.

**2. Stakes**
- What does the protagonist gain if they succeed? What do they lose if they fail or don't try?
- Stakes must be emotionally real, not abstract. "You'll be able to code" is weak. "You'll stop feeling like an impostor in meetings" is strong.

**3. Inciting incident**
- What pulls the protagonist into the journey? A specific moment in the first 5 minutes of the course that makes continuing feel necessary.
- For adults: usually a moment of recognition — "yes, this is my situation". For children: usually a hook that produces surprise or curiosity.

**4. Rising challenges**
- Sequence 3–7 challenges of increasing difficulty. Each is a small "battle" the protagonist must win.
- Each challenge should threaten failure genuinely. A course where "you can't lose" has no narrative tension.

**5. Turning point**
- Somewhere in the middle, something shifts. The protagonist's understanding deepens, or a belief they held is overturned, or they discover they have become someone different than they were.
- This is where identity transformation happens — critical for career-change courses.

**6. Climax**
- The final test. Harder than anything before. The protagonist must use what they've learned in a situation they have not explicitly been prepared for.
- In course terms: the performance task / capstone / real-world use.

**7. Resolution**
- What is the "new normal" at the end? How is the protagonist different?
- This is where transfer happens — the protagonist takes the skill into their ongoing life.

**8. Recurring motifs**
- 2–3 elements that come back throughout the arc: a character, a metaphor, a phrase, an object. They create continuity and emotional anchoring.

**9. Emotional beats**
- Where will the learner feel frustration? Wonder? Doubt? Triumph? Map these deliberately — silence where there should be a beat is a failure of the arc.

**10. Module mapping**
- Show how each module/week fits the arc: setup, rising action, turning, climax, resolution.

Return output in this structure:

## Narrative Arc: [Course Title]

### Protagonist
[Paragraph]

### Stakes
[Gain and loss, emotionally framed]

### Inciting Incident
[Specific moment in first 5 minutes]

### Rising Challenges
[3–7 numbered, increasing difficulty]

### Turning Point
[What shifts, when, why]

### Climax
[The final test]

### Resolution
[New normal]

### Recurring Motifs
[2–3 elements]

### Emotional Beat Map
[Module-by-module emotional state expected]

### Module → Arc Mapping
[Which module is which part of the arc]

**Self-check before returning:** Verify (a) stakes are emotionally concrete, not abstract, (b) the inciting incident happens in the first 5 minutes (not week 2), (c) there is a real turning point, not just more content, (d) the climax requires something not explicitly taught (transfer), and (e) emotional beats are named, not assumed.
```

## Example Output (abridged)

**Scenario:** *Public speaking for shy kids 10–12, 6 weeks / freezing-on-camera kids*

### Protagonist
"You" — a kid who has something interesting to say but freezes when people watch. Not broken, not shy in a bad way — just haven't found the right way to let your voice out yet.

### Stakes
- **Gain:** By the end, you will tell your own story on camera and hear people say "I want to hear more." That feeling changes what you believe about yourself.
- **Lose:** If you stop now, the voice stays locked inside. That is the worst loss — not of speaking, but of being heard.

### Inciting Incident
Session 1, minute 3: a short video of another 10-year-old telling a 60-second story about being scared of the dark. She laughs mid-story. The teacher says: "She was more scared to make this video than you are right now. Today you're going to make one too — and it's going to be weird and that's okay."

### Rising Challenges
1. Record a voice note of your name and one thing you like (no video)
2. Record a voice note telling one weird thing that happened this week
3. Record a video with the camera pointed at your hands while you talk
4. Record a video with your face, 30 seconds, any story
5. Turning point: record a story where you pause, restart, laugh — and keep it in
6. Final: 3-minute story to a small audience (parents/siblings on video)

### Turning Point
Session 4: "If you make a mistake and keep going, people like you MORE, not less." Many kids have been taught the opposite — that any slip is shameful. Proving this wrong (with examples) is the identity shift.

### Climax
Session 6: record and share the 3-minute story with a real audience. Not rehearsed into robot mode.

### Resolution
You have one story on camera you are proud of. You know how to make another. The "kid who freezes" identity is no longer the whole truth.

### Recurring Motifs
- "The weird middle bit is the good bit" (said in every session when something goes wrong)
- The laughing girl from session 1 (returns in session 4 as proof that even the example video had mistakes left in)
- The phrase "let your voice out" (not "speak up")

### Emotional Beat Map
| Week | Expected feeling |
|---|---|
| 1 | Nervous → relieved (didn't die) |
| 2 | Shy → slightly proud |
| 3 | Awkward → curious |
| 4 | Surprised (turning point) |
| 5 | Shaky but brave |
| 6 | Triumphant + raw |

---

## Known Limitations

1. **Narrative can feel forced on pure technical content.** Don't invent fake drama for SQL syntax. Use narrative where identity or emotion are actually at stake; use clean direct instruction where they aren't.

2. **Stakes must be real or learners smell fake.** Don't write "the world is counting on you" for a spreadsheet course. Calibrate the stakes to what is actually true for this learner.

3. **Children's narrative ≠ adults' narrative.** Children tolerate (and love) more explicit story elements. Adults prefer narrative that feels like their own life rather than someone else's story.
