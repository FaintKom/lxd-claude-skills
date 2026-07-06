---
# AGENT SKILLS STANDARD FIELDS (v2)
name: mayer-multimedia-principles-designer
description: "Design or audit any multimedia learning asset (video, slide deck, e-learning module, interactive explainer) against Mayer's 12 principles of multimedia learning: coherence, signalling, redundancy, spatial/temporal contiguity, segmenting, pre-training, modality, multimedia, personalization, voice, image, embodiment. Use when producing videos, slides, screencasts, or any learning material that combines words and pictures."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/mayer-multimedia-principles-designer"
skill_name: "Mayer Multimedia Principles Designer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "Mayer (2021) — Multimedia Learning (3rd ed.), Cambridge University Press: the canonical reference with meta-analytic evidence for each principle"
  - "Mayer (2014) — The Cambridge Handbook of Multimedia Learning (2nd ed.)"
  - "Clark & Mayer (2016) — e-Learning and the Science of Instruction (4th ed.): practitioner translation"
  - "Mayer & Moreno (2003) — Nine ways to reduce cognitive load in multimedia learning"
  - "Sweller, Ayres & Kalyuga (2011) — Cognitive Load Theory: theoretical foundation"
input_schema:
  required:
    - field: "asset_description"
      type: "string"
      description: "What multimedia asset is being designed or audited (video, slide deck, screencast, animation, interactive module)"
    - field: "content_topic"
      type: "string"
      description: "What the asset teaches"
    - field: "audience"
      type: "string"
      description: "Learners' level and context"
  optional:
    - field: "existing_draft"
      type: "string"
      description: "Description of current draft if auditing rather than designing from scratch"
    - field: "delivery_context"
      type: "string"
      description: "Where it will be consumed: desktop, mobile, classroom projection, self-paced"
output_schema:
  type: "object"
  fields:
    - field: "principle_application"
      type: "array"
      description: "For each of Mayer's 12 principles: what it means, how to apply it here, concrete decisions"
    - field: "red_flags"
      type: "string"
      description: "Common violations of the principles specific to this asset"
    - field: "production_checklist"
      type: "string"
      description: "Actionable checklist for the person producing the asset"
chains_well_with:
  - "learning-video-producer"
  - "cognitive-load-analyser"
  - "dual-coding-designer"
  - "microlearning-architect"
  - "course-onboarding-designer"
  - "accessible-learning-designer"
teacher_time: "5 minutes"
tags: ["Mayer", "multimedia-learning", "video-design", "cognitive-load", "learning-experience-design"]
---

# Mayer Multimedia Principles Designer

## What This Skill Does

Applies Mayer's 12 principles of multimedia learning to a specific asset — either designing from scratch or auditing an existing draft. Each principle is translated into concrete production decisions: what to cut, what to add, where to put text relative to visuals, when to narrate vs. display, whether to segment, whether to pre-train. The principles have some of the strongest empirical support in the entire learning sciences literature (multiple meta-analyses), yet are routinely violated in corporate training videos, lecture capture, and "modern" e-learning. AI is valuable here because applying 12 principles in parallel while also producing content is cognitively expensive — designers default to 2–3 favourites and miss the rest.

## Evidence Foundation

Richard Mayer's Cognitive Theory of Multimedia Learning (Mayer, 2021, 3rd ed.) integrates cognitive load theory (Sweller, Ayres & Kalyuga, 2011), dual-coding theory (Paivio), and working memory models to produce 12 empirically-tested principles. Each principle has multiple experimental studies and, for most, meta-analytic support. The most robust effects: **multimedia** (words + pictures > words alone), **coherence** (less is more — remove extraneous material), **signalling** (cue the important bits), **redundancy** (don't put on-screen text that duplicates narration), **spatial contiguity** (place related words and pictures near each other), **temporal contiguity** (present them simultaneously), **segmenting** (break into learner-paced chunks), **pre-training** (give key terms first), **modality** (narrate pictures rather than caption them), **personalization** (conversational style), **voice** (human > machine), **embodiment** (on-screen agents with gestures). The one principle with weaker support is **image** (instructor face on screen); Mayer 2021 edition notes mixed results. Clark & Mayer (2016) provides the most practical translation for workplace e-learning.

## Input Schema

- **Asset:** *e.g. "10-minute explainer video teaching SQL JOIN syntax"*
- **Topic:** *e.g. "INNER and LEFT JOIN with business examples"*
- **Audience:** *e.g. "Adults with Excel background, no SQL experience"*
- Optional: existing draft, delivery context

## Prompt

```
You are an expert in multimedia learning design, fluent in Mayer's Cognitive Theory of Multimedia Learning (Mayer, 2021) and the supporting experimental evidence. You apply ALL 12 principles — not just the famous 3 or 4 — and translate each into concrete production decisions.

Your task:

**Asset:** {{asset_description}}
**Topic:** {{content_topic}}
**Audience:** {{audience}}
**Existing draft:** {{existing_draft}}
**Delivery context:** {{delivery_context}}

For each principle below, do three things:
1. State the principle in one sentence
2. Apply it to THIS asset with specific decisions (not generic advice)
3. Flag one common violation in this asset type and how to avoid it

**Principle 1 — Multimedia:** Present words and pictures, not words alone.
**Principle 2 — Coherence:** Exclude extraneous words, pictures, and sounds. This is the single most-violated principle — default to cutting.
**Principle 3 — Signalling:** Highlight essential material (bold, arrows, zoom, colour, verbal cues like "notice that...").
**Principle 4 — Redundancy:** Do NOT add on-screen text that duplicates narration verbatim. It splits attention and hurts learning.
**Principle 5 — Spatial contiguity:** Place labels next to what they label, not in a legend below.
**Principle 6 — Temporal contiguity:** Present narration and corresponding visuals at the same moment, not sequentially.
**Principle 7 — Segmenting:** Break the asset into learner-paced segments. Continuous playback without pauses fails.
**Principle 8 — Pre-training:** Introduce key terms and components BEFORE the main explanation, so working memory isn't burning cycles on vocabulary during the main point.
**Principle 9 — Modality:** Narrate visuals rather than captioning them. Audio + visual uses two channels; on-screen text + visual uses only one.
**Principle 10 — Personalization:** Use conversational style ("you" and "I") rather than formal third person. Measurable effect.
**Principle 11 — Voice:** Human voice outperforms synthesized voice. Prefer real narration.
**Principle 12 — Embodiment/Image:** On-screen presence of a human/agent with gestures can help (embodiment). Static instructor face has weaker support — include only if it serves a specific purpose.

After going through each principle, produce:

**Red Flags in this asset:** 3–5 specific things that are likely wrong or easy to get wrong.

**Production Checklist:** A bullet list the producer can tick off before publishing.

Return in this structure:

## Multimedia Design: [Asset Name]

### Principle-by-Principle Application

**1. Multimedia** — [one sentence] — [decision for this asset] — [violation to avoid]
**2. Coherence** — [...]
[...through 12]

### Red Flags
[Specific issues likely in this asset type]

### Production Checklist
[Bullet list, concrete, tick-able]

**Self-check before returning:** Verify (a) ALL 12 principles are addressed, not just the 3 famous ones, (b) decisions are specific to the asset, not generic rules, (c) you explicitly flagged the redundancy principle (because on-screen + narration duplication is the single most common violation), and (d) the checklist is actionable, not aspirational.
```

## Example Output (abridged)

**Scenario:** *10-min explainer video on SQL JOINs, adults no SQL background, self-paced on desktop*

### Principle Application (sample)

**2. Coherence** — Cut extraneous material. — Decision: no background music, no intro animation, no "about me" segment. Start with the problem (messy data in two tables) within 10 seconds. — **Violation to avoid:** 30-sec branded intro animation that trains nothing.

**4. Redundancy** — Don't caption narration verbatim. — Decision: on-screen text only shows the SQL code itself, not a transcription of what I'm saying about it. — **Violation to avoid:** bullet points on slide that mirror the narration word-for-word.

**7. Segmenting** — Break into learner-paced chunks. — Decision: split into 4 × 2.5-min segments with a "continue" button between each: (1) problem setup, (2) INNER JOIN, (3) LEFT JOIN, (4) practice. — **Violation to avoid:** one continuous 10-min video with no natural pause points.

**9. Modality** — Narrate, don't caption. — Decision: when showing the schema diagram, I narrate "this table has customer_id as its primary key" rather than putting that sentence on screen. Exception: the SQL code itself stays on screen because it IS the content, not a label. — **Violation to avoid:** slides with bullet points the instructor reads aloud verbatim.

### Red Flags
- Default tutorial format is: talking head + slide with bullet points the instructor reads. This violates coherence, redundancy, and modality simultaneously.
- Background music is nearly always extraneous for instructional content.
- If the video is published without segment markers, temporal contiguity breaks when the learner replays.

### Production Checklist
- [ ] No intro animation >5 sec
- [ ] No background music
- [ ] On-screen text is code or labels only, not narration transcript
- [ ] Schema labels placed directly next to tables, not in a legend
- [ ] Natural segment breaks with pause points every 2–3 min
- [ ] Pre-training slide introduces "primary key" and "foreign key" before main JOIN explanation
- [ ] Script uses "you" and "I", not "the learner" or "the instructor"
- [ ] Human narration, not TTS

---

## Known Limitations

1. **Principles interact.** Sometimes honouring one violates another (e.g., segmenting can break temporal contiguity). Prioritise coherence and redundancy when conflicts arise — they have the strongest effects.

2. **Not every principle applies to every asset.** Static PDFs have no temporal dimension; audio-only has no spatial contiguity. Apply what is relevant and say so.

3. **The multimedia principles were largely tested on novices.** Experts can sometimes tolerate (or benefit from) more redundancy and less segmenting. Know your audience.

4. **Image principle has weaker evidence.** Don't over-weight instructor face on screen. Use embodiment (gestures + speech) rather than static image if you want human presence.
