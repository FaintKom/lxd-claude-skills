---
# AGENT SKILLS STANDARD FIELDS (v2)
name: learning-video-producer
description: "Plan a learning video from script to publish: structure, pacing, shot choice, visuals, b-roll, narration style, length, captions, and thumbnail. Operationalises Mayer's multimedia principles into concrete production decisions. Use when making explainer videos, screencasts, lecture captures, or any video where the goal is learning — not entertainment or marketing."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/learning-video-producer"
skill_name: "Learning Video Producer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Mayer (2021) — Multimedia Learning (3rd ed.)"
  - "Guo, Kim & Rubin (2014) — How video production affects student engagement: MOOC study showing shorter is better, talking-head hybrid beats slides-only"
  - "Brame (2016) — Effective educational videos"
  - "Clark & Mayer (2016) — e-Learning and the Science of Instruction"
  - "Chen & Wu (2015) — Effects of different video lecture types on sustained attention and cognitive load"
input_schema:
  required:
    - field: "video_topic"
      type: "string"
      description: "What the video teaches"
    - field: "target_audience"
      type: "string"
      description: "Who will watch it"
    - field: "learning_objective"
      type: "string"
      description: "What learners will be able to do/know after watching"
  optional:
    - field: "target_length"
      type: "string"
      description: "Desired length if constrained (default: as short as possible)"
    - field: "production_resources"
      type: "string"
      description: "Solo / team / studio / phone camera / screen capture only"
output_schema:
  type: "object"
  fields:
    - field: "script_outline"
      type: "string"
      description: "Beat-by-beat structure with timing"
    - field: "shot_list"
      type: "string"
      description: "Visual plan per segment"
    - field: "production_specs"
      type: "string"
      description: "Length, format, framing, audio, captions"
    - field: "common_mistakes_avoided"
      type: "string"
      description: "Specific learning-video anti-patterns"
chains_well_with:
  - "mayer-multimedia-principles-designer"
  - "accessible-learning-designer"
  - "microlearning-architect"
  - "course-onboarding-designer"
  - "gagne-nine-events-lesson-designer"
teacher_time: "5 minutes"
tags: ["video-production", "multimedia", "Mayer", "screencast", "learning-experience-design"]
---

# Learning Video Producer

## What This Skill Does

Plans a learning video from script to publish, operationalising the multimedia learning evidence base (Mayer, 2021; Guo et al., 2014; Brame, 2016) into concrete production decisions. The output includes a beat-by-beat script outline with timing, a shot list, production specs (length, framing, audio, captions), and an anti-pattern list specific to learning video (not entertainment video). Most tutorial videos are too long, too slow, too passive, and produced without knowledge of the empirical evidence on what works.

## Evidence Foundation

Guo, Kim & Rubin (2014) analysed 6.9 million video sessions from edX MOOCs and found: videos <6 minutes are the most engaging (drops sharply after 6 min); talking-head + slide hybrid beats slides-only; informal over studio-polished; personal over corporate; slower tutorial pace beats fast delivery of equivalent content. Brame (2016) synthesises the evidence into practical recommendations: keep videos short, use conversational style, highlight important information, use complementary text/visual (not duplicate), and pair videos with active practice. Mayer (2021) provides the broader cognitive theory. Chen & Wu (2015) show that different video types (lecture capture, picture-in-picture, voice-over) have measurably different effects on attention and cognitive load. Evidence is strong that these principles produce measurable learning differences.

## Input Schema

- **Topic:** *e.g. "How SQL JOINs work"*
- **Audience:** *e.g. "Adults with Excel background, no SQL"*
- **Learning objective:** *e.g. "Learner can write a two-table INNER JOIN and a LEFT JOIN for a given schema"*
- Optional: target length, production resources

## Prompt

```
You are a learning video producer who operationalises the evidence base (Mayer, 2021; Guo et al., 2014; Brame, 2016; Chen & Wu, 2015). You refuse to make 30-minute videos — the data says <6 min is optimal, <10 min is the hard cap for most content. You produce videos designed for learning, not for entertainment or personal branding.

Your task:

**Topic:** {{video_topic}}
**Audience:** {{target_audience}}
**Objective:** {{learning_objective}}
**Target length:** {{target_length}}
**Production resources:** {{production_resources}}

Plan in six parts:

**1. Decide length and format.**
- Target: under 6 min. If content requires more, split into multiple videos — do not make one long one. Guo et al. (2014) is unambiguous on this.
- Format choice: talking head / screen capture / slide + voice / hybrid / animation. Use evidence: hybrid (talking head + content) consistently outperforms slides-only.
- Orientation: mobile audience → consider vertical or square; desktop → 16:9.

**2. Script structure (beat-by-beat with timing).**
- **Hook (0:00–0:15):** one sentence showing the problem or the payoff. Not "hi, today we're going to learn..." — start with the thing.
- **Objective (0:15–0:30):** one sentence: what you'll be able to do by the end.
- **Pre-training if needed (0:30–1:00):** any vocabulary or prerequisite concepts (Mayer's pre-training principle).
- **Core content (1:00–4:30):** worked example(s) with narration. Show the thing happening, narrate reasoning. Avoid bullet point slides.
- **Active recap (4:30–5:00):** ask the viewer to predict, answer, or try something. Don't just summarise.
- **Exit (5:00–5:30):** "next, try this" — specific follow-up action, not "like and subscribe".

**3. Shot list / visuals.**
- Primary frame: what the viewer sees most of the time
- Cutaways / b-roll: what you cut to for emphasis
- On-screen text rules (Mayer redundancy principle): NO caption of narration. Only code/labels/key terms on screen.
- Highlight/zoom at moments of importance (signalling)
- Transitions: simple cuts, no fancy effects

**4. Audio and narration style.**
- Conversational ("you" and "I"), not formal
- Personal > corporate
- Real voice > synthetic (Mayer's voice principle)
- Quiet room, consistent levels, no background music
- Pace: slightly slower than natural speech, with pauses at section breaks

**5. Captions and accessibility.**
- Hand-verified captions, not auto-generated
- Chapter markers in YouTube/LMS (not a single 5 min blob)
- Transcript published alongside
- Colour contrast and font size tested at small screen

**6. Common mistakes to avoid (learning video specific).**
- 30-minute "complete guide" videos
- Bullet-point slides the instructor reads verbatim (redundancy violation)
- Talking for 2 minutes before showing anything
- Background music that competes with narration (coherence violation)
- "Like and subscribe" embedded in learning content
- Studio-polish that hides the instructor's personality (less engaging per Guo et al.)
- Instructor face in a corner throughout — should appear when it helps, not as constant decoration
- Speeding through to "save time" — leads to rewatching anyway

Return output in this structure:

## Learning Video Plan: [Topic]

### Length and Format
[Target length + format justification]

### Script Outline (beat-by-beat)
| Time | Beat | What happens | What viewer sees |
|---|---|---|---|
[Rows]

### Shot List
[Primary frame, cutaways, on-screen text rules]

### Audio and Narration Style
[Conversational style decisions, pace, music policy]

### Accessibility
[Captions, chapters, transcript]

### Common Mistakes Explicitly Avoided
[Specific pitfalls for this video type]

### Production Checklist
[Pre-record, record, post-production]

**Self-check before returning:** Verify (a) target length is <6 min unless content genuinely justifies otherwise, (b) on-screen text does not duplicate narration, (c) format is hybrid or screen-capture with voice, not slides-only, (d) hook happens in first 15 seconds, and (e) captions are flagged as hand-verified required.
```

## Example Output (abridged)

**Scenario:** *SQL JOINs for Excel users, 5-min target, screen capture possible*

### Length and Format
- Target: 5:30. If longer, split by JOIN type into two videos.
- Format: screen capture + voice-over, instructor face briefly at 0:00–0:15 for human presence, then disappears for main content (per Chen & Wu 2015 — picture-in-picture during key explanations, otherwise distracts)

### Script Outline
| Time | Beat | What | Viewer sees |
|---|---|---|---|
| 0:00–0:15 | Hook | "You have two Excel sheets. You need customer name next to their order. JOINs do that in one line." | Two messy sheets, then one clean merged result |
| 0:15–0:30 | Objective | "By the end, you'll write an INNER and a LEFT join." | Text card, 3 sec |
| 0:30–1:00 | Pre-training | "Two words: key, and table relationship." | Schema diagram, pointing |
| 1:00–2:30 | INNER JOIN worked example | Write query live, narrate decisions | SQL sandbox, query building |
| 2:30–4:00 | LEFT JOIN worked example | Contrast with same query as LEFT, show what's different | Side-by-side result set |
| 4:00–4:45 | Active recap | "Pause. Write a query joining customers to orders. Come back when done." | Blank sandbox, schema visible |
| 4:45–5:30 | Exit | "Try this in your own dataset today. Link below to practice questions." | Text card with link |

### Shot List
- Primary: SQL sandbox full screen
- Cutaway: instructor face at 0:00–0:15, 4:00–4:15 (for eye contact at active recap moment)
- On-screen text: ONLY the SQL code and table/column labels. No captions of narration.
- Zoom: into the JOIN keyword at 1:30, into the NULL rows at 3:15 (signalling)

### Audio and Narration
- Conversational, "you" and "I"
- ~140 wpm (slower than normal speech)
- 1–2 sec pause after each major concept
- No background music
- Quiet room, close mic

### Accessibility
- Captions hand-verified
- Chapters: 0:00 Problem, 0:30 Setup, 1:00 INNER, 2:30 LEFT, 4:00 Your Turn
- Transcript as separate download
- Font in schema diagram tested at 320px width (mobile)

### Common Mistakes Avoided
- Not 20 min
- No bullet-point slides
- No "welcome to my channel"
- No background lo-fi music
- No face-in-corner throughout
- Active practice built in (not "watch, then try later")

### Production Checklist
- Pre-record: script, schema diagram, practice questions ready
- Record: quiet room, screen capture @ 1080p, mic levels tested, 2 takes
- Post: trim dead air, add zoom at 1:30 and 3:15, caption, add chapters, transcript
- Publish: thumbnail with the end result visible, title benefit-forward

---

## Known Limitations

1. **5–6 min is not a religious rule.** Some content needs 10 min. But default to shorter, and split before stretching.

2. **Talking head vs no talking head is context-dependent.** The evidence says hybrid is best; full-face throughout hurts for pure tutorial content.

3. **Production polish vs. authenticity trade-off.** Too rough looks incompetent; too polished loses warmth. Aim for "clear and personal", not "broadcast quality".
