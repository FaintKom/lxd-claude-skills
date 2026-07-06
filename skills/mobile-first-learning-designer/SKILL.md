---
# AGENT SKILLS STANDARD FIELDS (v2)
name: mobile-first-learning-designer
description: "Design learning content for mobile-first consumption: short sessions, thumb-reach interaction, offline resilience, notification discipline, bandwidth constraints, small screens, portrait orientation, interruption tolerance. Use when learners will primarily access on phones — which is most adult and children's audiences today, even when desktop is theoretically available."
disable-model-invocation: false
user-invocable: true
effort: low

# EXISTING FIELDS

skill_id: "learning-experience-design/mobile-first-learning-designer"
skill_name: "Mobile-First Learning Designer"
domain: "learning-experience-design"
version: "1.0"
evidence_strength: "moderate"
evidence_sources:
  - "Wroblewski (2011) — Mobile First: foundational text on mobile-first design"
  - "Clark (2012) — Designing Web Interfaces for Mobile"
  - "Traxler (2009) — Current State of Mobile Learning"
  - "Nielsen Norman Group — Mobile UX reports"
  - "Sweller et al. (2011) — Cognitive Load Theory: small screens amplify cognitive load"
input_schema:
  required:
    - field: "course_or_asset"
      type: "string"
      description: "What is being designed for mobile"
    - field: "audience_context"
      type: "string"
      description: "When and where learners will use it"
  optional:
    - field: "platform"
      type: "string"
      description: "Native app / mobile web / hybrid / PWA"
    - field: "bandwidth_assumptions"
      type: "string"
      description: "Reliable wifi / cellular / variable / offline primary"
output_schema:
  type: "object"
  fields:
    - field: "session_architecture"
      type: "string"
      description: "Session length, interruption recovery, resume behaviour"
    - field: "interaction_patterns"
      type: "string"
      description: "Thumb reach, tap targets, gestures, portrait-first"
    - field: "resilience"
      type: "string"
      description: "Offline, reconnect, state preservation, bandwidth adaptation"
    - field: "notification_strategy"
      type: "string"
      description: "When, how often, what cadence, respectful defaults"
    - field: "anti_patterns"
      type: "string"
      description: "Mobile-learning traps avoided"
chains_well_with:
  - "microlearning-architect"
  - "mayer-multimedia-principles-designer"
  - "engagement-mechanics-designer"
  - "course-onboarding-designer"
  - "accessible-learning-designer"
teacher_time: "3 minutes"
tags: ["mobile-first", "mobile-learning", "mLearning", "responsive", "learning-experience-design"]
---

# Mobile-First Learning Designer

## What This Skill Does

Designs learning content for phone-first consumption — where the default is 5–10 minute sessions on the bus, in a waiting room, or between other tasks, not 60-minute sessions at a desk. Most learners today consume at least some content on mobile; for many audiences (commuters, parents, shift workers, children) mobile is the primary channel. Designing desktop-first and "making it responsive" produces mobile experiences that feel like punishment.

## Evidence Foundation

Wroblewski (2011) established mobile-first as a design discipline — not because mobile is better, but because designing for constraints produces clearer thinking and degrades gracefully. Traxler (2009) documents the unique properties of mobile learning: interruptible, contextual, short-duration, social. Nielsen Norman Group research on mobile UX consistently shows that tap-target size, thumb reach, portrait orientation, and interruption tolerance are first-class concerns, not afterthoughts. Sweller et al. (2011) show that small screens amplify cognitive load because all UI chrome competes with content for working memory.

## Input Schema

- **Course/asset:** *e.g. "Daily Spanish practice app"*
- **Audience context:** *e.g. "Commuters, 5–10 min sessions on the subway, one-handed"*
- Optional: platform, bandwidth

## Prompt

```
You are a mobile-first learning experience designer (Wroblewski, 2011; Traxler, 2009; NN/g research). You design for interruption, portrait orientation, thumb reach, and variable connectivity as defaults — not edge cases.

Your task:

**Course/asset:** {{course_or_asset}}
**Audience context:** {{audience_context}}
**Platform:** {{platform}}
**Bandwidth:** {{bandwidth_assumptions}}

Design in these dimensions:

**1. Session architecture**
- Target session length: how short is realistic? (3–10 min typical)
- Interruption recovery: what happens when the phone rings or the user switches apps mid-session? State must persist.
- Resume: one-tap to exact spot. No "where was I?" screen.

**2. Interaction patterns**
- Portrait-first. Landscape is nice-to-have, not default.
- Thumb reach: primary actions in the bottom third of the screen, not top
- Tap targets: minimum 44×44pt (iOS) / 48×48dp (Android); WCAG 2.2 says 24×24 CSS minimum but go bigger
- One-handed use: avoid requiring two hands for core flows
- Gestures: discoverable, with visible alternatives; no gesture-only critical actions
- Forms: minimum fields, native keyboards (numeric, email), autocomplete, avoid text entry when possible

**3. Content for small screens**
- Text: larger than desktop default, higher line height, shorter line length
- Images: must work at 360px wide minimum
- Video: vertical or square preferred; 16:9 loses half the screen in portrait
- Tables: redesign as cards; horizontal scroll is last resort
- Diagrams: must be readable without zoom

**4. Resilience**
- Offline: can the learner make progress without connectivity? What degrades gracefully?
- Reconnect: state syncs without user action
- Bandwidth adaptation: progressive loading, video quality options, text fallbacks
- State preservation: if the OS kills the app, learner loses nothing

**5. Notification strategy**
- Frequency: few and meaningful, not many and noisy
- Timing: user-chosen, not algorithm-chosen (respects focus and sleep)
- Content: specific and personal, not "reminder to learn!"
- Respect: never after 9pm, never before 7am (unless user opts in)
- Opt-out: easy, no guilt

**6. Mobile-specific anti-patterns**
- Full-screen interstitial modals on small screens
- Desktop-sized hit areas
- Password entry without "show password"
- Video autoplay with sound on cellular
- Forcing landscape orientation
- Tiny "close" buttons in corners
- Content that requires zoom to read
- Login flows that break on mobile browser

Return output in this structure:

## Mobile-First Design: [Course/Asset]

### Session Architecture
[Length, interruption, resume]

### Interaction Patterns
[Thumb reach, targets, gestures, forms]

### Content for Small Screens
[Text / images / video / tables / diagrams]

### Resilience
[Offline / reconnect / bandwidth / state]

### Notification Strategy
[Frequency / timing / content / opt-out]

### Anti-Patterns Explicitly Avoided
[Specific traps]

**Self-check before returning:** Verify (a) portrait is the default not an afterthought, (b) primary actions are in thumb reach, (c) there is an offline or degraded-network answer, (d) notification strategy includes quiet hours, and (e) you avoided at least one common mobile-learning anti-pattern.
```

## Example Output (abridged)

**Scenario:** *Daily Spanish practice, commuters, 5–10 min sessions, one-handed, variable subway connectivity*

### Session Architecture
- Target: 5 min, hard cap at 10
- Interruption: session state saves on every interaction; can close app mid-exercise and return to exact same question
- Resume: one tap from lock screen widget → exact exercise

### Interaction
- Portrait only (ignore landscape)
- Primary actions (Check / Next) in bottom third, 60pt tall
- Tap-to-reveal translations; drag-to-match exercises sized for thumb
- No typing — tap word tiles to build sentences
- No swipe-to-delete critical actions

### Content
- 16pt body text minimum, 1.5 line height
- Images cropped square, compressed to <100KB
- Videos are vertical 9:16 or square 1:1, never 16:9
- No tables, no diagrams that require zoom

### Resilience
- Daily exercises pre-downloaded at last sync; full sessions work offline
- Progress caches locally, syncs when back online
- Video with text fallback if bandwidth <1Mbps
- If app killed mid-exercise, return lands on same screen

### Notifications
- 1 per day default, user-chosen time
- No notifications 10pm–7am
- Body text is specific: "5 min to hold your 12-day streak" not "Keep learning!"
- One tap to disable, no shame

### Anti-Patterns Avoided
- No forced landscape
- No autoplay audio on cellular
- No "rate us" interstitial mid-session
- No tiny close buttons
- No desktop-style dense menus

---

## Known Limitations

1. **Mobile-first doesn't mean mobile-only.** If desktop users exist, plan the graceful upgrade path. The principles translate upward better than the reverse.

2. **Platform conventions matter.** iOS and Android have different expectations for navigation, back gestures, and animations. Honour the platform.

3. **Children's mobile UX is different.** Younger children have different tap accuracy, different vocabulary, and parents as gatekeepers. Adjust target sizes and copy accordingly.
