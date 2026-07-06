---
# AGENT SKILLS STANDARD FIELDS (v2)
name: accessible-learning-designer
description: "Audit or design a course for accessibility: WCAG 2.2 AA compliance, Universal Design for Learning (UDL), screen reader compatibility, keyboard navigation, captions, alt text, colour contrast, cognitive accessibility, and accommodation for common disabilities. Use when producing any learning content that will be published publicly or used in organisations with accessibility obligations, and as baseline good practice for all learners."
disable-model-invocation: false
user-invocable: true
effort: medium

# EXISTING FIELDS

skill_id: "learning-experience-design/accessible-learning-designer"
skill_name: "Accessible Learning Designer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "strong"
evidence_sources:
  - "W3C (2023) — Web Content Accessibility Guidelines (WCAG) 2.2"
  - "CAST (2018) — Universal Design for Learning Guidelines version 2.2"
  - "Rose & Meyer (2002) — Teaching Every Student in the Digital Age: UDL foundation"
  - "Burgstahler (2020) — Universal Design in Higher Education: From Principles to Practice"
  - "U.S. Section 508 standards and EU EN 301 549"
input_schema:
  required:
    - field: "asset_or_course"
      type: "string"
      description: "What is being audited or designed"
    - field: "format"
      type: "string"
      description: "Video, web module, PDF, live session, e-learning SCORM, app, etc."
  optional:
    - field: "known_audience_needs"
      type: "string"
      description: "If specific accessibility needs are known (visual, hearing, motor, cognitive)"
    - field: "compliance_target"
      type: "string"
      description: "WCAG AA / WCAG AAA / Section 508 / EN 301 549 / voluntary best practice"
output_schema:
  type: "object"
  fields:
    - field: "wcag_checklist"
      type: "array"
      description: "Each relevant WCAG criterion: status, evidence, fix if needed"
    - field: "udl_application"
      type: "string"
      description: "UDL principles applied: multiple means of representation, action/expression, engagement"
    - field: "accommodations"
      type: "string"
      description: "Specific accommodations for common disability categories"
    - field: "common_violations"
      type: "string"
      description: "Issues specific to this format that routinely fail"
chains_well_with:
  - "mayer-multimedia-principles-designer"
  - "learning-video-producer"
  - "mobile-first-learning-designer"
  - "microlearning-architect"
  - "course-onboarding-designer"
teacher_time: "5 minutes"
tags: ["accessibility", "WCAG", "UDL", "inclusive-design", "learning-experience-design"]
---

# Accessible Learning Designer

## What This Skill Does

Audits or designs learning content for accessibility using WCAG 2.2 (legal and technical standard) and UDL (pedagogical framework). Produces a concrete checklist mapped to the specific format (video, PDF, web module, live session), flags common violations for that format, and recommends accommodations. Accessibility is not optional — it is both legally required in many jurisdictions and essential for reaching the ~15% of learners who have some disability, plus the much larger population of situationally disabled learners (noisy environments, limited bandwidth, older devices).

## Evidence Foundation

WCAG 2.2 (W3C, 2023) is the international technical standard, with legal force in the EU (EN 301 549), US (Section 508 for federal contexts, ADA for general), and many other jurisdictions. UDL (CAST, 2018; Rose & Meyer, 2002) is the pedagogical complement: three principles (multiple means of representation, multiple means of action/expression, multiple means of engagement) that make learning accessible to the widest range of learners by design, not by retrofit. Burgstahler (2020) provides the most rigorous practitioner treatment for higher education. Evidence is strong that accessible design benefits all learners, not just those with disabilities (the "curb cut effect"): captions help non-native speakers and noisy environments; clear structure helps everyone.

## Input Schema

- **Asset/course:** *e.g. "10-video e-learning module on customer service"*
- **Format:** *e.g. "SCORM module with video + quiz + PDF workbook"*
- Optional: known audience needs, compliance target

## Prompt

```
You are an accessibility-focused learning designer fluent in WCAG 2.2 (W3C, 2023) and UDL (CAST, 2018). You refuse to treat accessibility as a retrofit — you design for it from the start. You know that accessibility benefits all learners through the curb-cut effect.

Your task:

**Asset/course:** {{asset_or_course}}
**Format:** {{format}}
**Known audience needs:** {{known_audience_needs}}
**Compliance target:** {{compliance_target}} (default: WCAG 2.2 AA)

Produce an accessibility audit/design in this structure:

**Part 1 — Format-specific WCAG checklist**
For the given format, list the relevant WCAG criteria and mark each: compliant / violation / not applicable. Include at minimum:

**Perceivable:**
- Text alternatives (alt text for images, transcripts for audio)
- Captions for video (not auto-generated — verified)
- Audio description for video where visual content conveys meaning
- Colour contrast 4.5:1 for normal text, 3:1 for large text
- Text resize to 200% without loss of content/function
- Content not conveyed by colour alone
- Audio control (no autoplay, or easy stop)

**Operable:**
- Keyboard navigation (all interactive elements reachable and operable)
- No keyboard trap
- Skip links for repetitive content
- Timing adjustable (no short time limits on learning activities)
- No flashing content (seizure risk)
- Focus visible
- Focus order logical
- Pointer target size (24×24 CSS pixels minimum for 2.2)

**Understandable:**
- Language of page/content identified
- Consistent navigation
- Consistent identification
- Error identification and suggestion
- Instructions clear
- Reading level appropriate for audience

**Robust:**
- Compatible with assistive technologies (valid HTML, proper ARIA)
- Name/role/value programmatically determinable
- Status messages announced to screen readers

**Part 2 — UDL application**
Apply the three UDL principles to the course:

1. **Multiple means of representation** (the "what" of learning):
   - Is content available in multiple formats? (text + audio + visual)
   - Are key concepts explained in more than one way?
   - Is vocabulary pre-taught / linked to definitions?
   - Are visuals labelled, not just pretty?

2. **Multiple means of action and expression** (the "how" of learning):
   - Can learners demonstrate understanding in more than one way?
   - Text response, voice response, drawing, choice — are alternatives available where possible?
   - Are assessments fair to learners who process or produce differently?

3. **Multiple means of engagement** (the "why" of learning):
   - Are there choices that let learners take ownership?
   - Is content relevant to different contexts and identities?
   - Is feedback timely and growth-oriented?

**Part 3 — Accommodations for common disability categories**
For each, specific provisions:
- **Blind / low vision:** screen reader compatibility, alt text quality, keyboard access, colour independence
- **Deaf / hard of hearing:** captions quality, transcripts, signed content where feasible
- **Motor / mobility:** keyboard, voice control, large targets, no timing pressure
- **Cognitive / learning disabilities:** clear language, chunking, predictable structure, alternative paths, reduced distraction
- **Mental health / anxiety:** no forced public performance, opt-out for social features, calm defaults

**Part 4 — Common violations in this format**
Every format has its own common failures. For video: auto-generated captions accepted as "done" (they are not accurate). For PDF: untagged, not reflowable. For SCORM: keyboard traps in custom widgets. For live sessions: no real-time captions, chat as only interaction mode (excludes non-typers).

Return output in this structure:

## Accessibility Audit/Design: [Asset]

### WCAG 2.2 Checklist (format-relevant)
| Criterion | Status | Evidence/Fix |
|---|---|---|
[Row per relevant criterion]

### UDL Application
[Representation / Action & Expression / Engagement with specific decisions]

### Accommodations by Disability Category
[Bullet list per category]

### Common Violations in This Format
[Specific failures to watch for in this format type]

### Priority Fixes
[Ranked list if any violations found]

**Self-check before returning:** Verify (a) you included UDL not only WCAG (WCAG is legal minimum, UDL is pedagogical), (b) accommodations cover cognitive and mental-health categories not only physical, (c) auto-generated captions are flagged as insufficient, and (d) you identified at least one format-specific common violation.
```

## Example Output (abridged)

**Scenario:** *10-video SCORM module on customer service, corporate LMS*

### WCAG Checklist (abridged)
| Criterion | Status | Evidence/Fix |
|---|---|---|
| 1.2.2 Captions | Violation | Auto-captions only — must manually verify |
| 1.2.5 Audio description | Violation | Slide content shown silently — narrate or add AD track |
| 1.4.3 Colour contrast | Compliant | Verified 4.8:1 |
| 2.1.1 Keyboard | Violation | Custom video controls not keyboard operable |
| 2.4.7 Focus visible | Violation | Focus indicator missing on quiz buttons |
| 3.1.1 Language | Compliant | Declared en-US |

### UDL Application
- **Representation:** videos + transcripts + key-concept text summaries + glossary
- **Action/Expression:** quiz supports text and drag-drop responses; written reflection can be replaced with voice note
- **Engagement:** learners choose 1 of 3 customer-scenario tracks based on their role

### Accommodations
- Blind: screen-reader-tested transcripts; all video content described in audio
- Deaf: verified captions, transcripts, no audio-only information
- Cognitive: chunked into <7 min segments, predictable structure, glossary accessible from any screen
- Anxiety: role-play scenarios are private, not publicly graded

### Common Violations in SCORM
- Custom video players lack keyboard control
- Quiz widgets fail screen reader announcements of results
- "Next" button disabled until timer, no accessible alternative
- PDF workbooks attached untagged

### Priority Fixes
1. Manual caption verification (legal + pedagogical)
2. Keyboard access for video controls (blocks screen reader users entirely)
3. Audio description for slide-only content
4. Focus indicators across all interactive elements

---

## Known Limitations

1. **WCAG compliance ≠ real usability.** A course can pass WCAG and still be hard for screen-reader users. Test with real assistive technology users when possible.

2. **Live accessibility is harder than async.** Real-time captions, sign language interpretation, and chat-as-alternative all require deliberate planning.

3. **Cognitive accessibility is under-covered by WCAG.** WCAG 2.2 improved this with new criteria, but clear language and chunking still rely on UDL judgement, not mechanical testing.
