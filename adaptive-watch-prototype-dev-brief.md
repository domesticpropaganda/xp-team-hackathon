# Adaptive Intelligence — Watch Prototype
**Development brief for Claude Code / VS Code**

---

## Overview

A single HTML file prototype simulating a day in the life of the persona across three time-of-day phases. The prototype renders an Apple Watch UI in the browser, navigable via a time-of-day selector. Each phase has its own watch screen state and triggers phase-appropriate audio on interaction.

**Output:** `index.html` — open directly in browser, no server required  
**Stack:** Vanilla HTML + CSS + JavaScript. No frameworks, no build step.  
**Audio:** Placeholder `.mp3` files referenced by path. Swap for real files when ready.

---

## File structure

```
/adaptive-watch-prototype/
├── index.html              ← entire prototype
└── audio/
    ├── morning-brief.mp3   ← placeholder (Phase 1)
    ├── postworkout.mp3     ← placeholder (Phase 3)
    └── chime.mp3           ← short notification sound (Phase 2 nudge arrival)
```

All audio files should be created as silent placeholder `.mp3` files initially. Claude Code can generate these with a simple script or note their expected paths clearly in the HTML.

---

## Design system

Inherit exactly from the mockups already produced. All values below are specified — do not deviate.

### Colours
```
--bg-shell:        #1c1c1a   (watch outer casing)
--bg-screen:       #0d0d0b   (screen background)
--bg-surface:      #1a1a18   (bands, secondary surfaces)
--bg-card:         #222220   (elevated card background)
--accent-primary:  #D97757   (warm orange — high urgency, CTAs, playing state)
--accent-secondary:#6A9BCC   (cool blue — medium urgency)
--text-primary:    #FAF9F5   (primary text on dark)
--text-secondary:  #B0AEA5   (secondary / muted text)
--text-ghost:      #5a5a58   (timestamps, metadata)
--border-subtle:   #2e2e2b   (dividers, card borders)
--border-card:     #2a2a28   (button borders)
```

### Typography
```
Font: 'Poppins' (Google Fonts import)
Weights: 300 (body/labels), 400 (headlines/CTAs)

Scale:
  10px / 300   → source labels, metadata, eyebrows
  11px / 300   → secondary body, timestamps
  13–14px / 400 → primary headlines on watch
  11px / 300   → CTA buttons
```

### Watch shell dimensions
```
Outer shell:    176px wide, border-radius 44px
Screen:         156 × 190px, border-radius 36px
Band top/bot:   28px height, border-radius 8px top or bottom only
Crown button:   7 × 30px, right -8px, top 44px
Side button:    7 × 18px, right -8px, top 86px
```

### Buttons
```
Pill buttons:   height 28px, border-radius 14px, font 11px/300
Primary:        background #D97757, color #FAF9F5
Secondary:      background #2a2a28, color #B0AEA5
Full-width:     height 44px, border-radius 22px, font 13px/400 (phone player only)
```

---

## Layout

The prototype renders three elements horizontally centred on the page:

```
[ Time-of-day selector ]        ← top navigation
[ Watch shell + screen ]        ← centre, always visible
[ Context panel ]               ← right of watch, phase-specific info
```

### Time-of-day selector
Three pill tabs rendered above the watch:

```
[ 6:47am  Morning brief ]  [ 10:23am  Nudges ]  [ 12:40pm  Post-workout ]
```

- Active tab: `#D97757` background, `#FAF9F5` text
- Inactive tab: transparent, `#5a5a58` text, `#2e2e2b` border
- Clicking a tab transitions the watch screen and context panel to that phase
- Transition: fade + slight upward translate, 200ms ease

### Context panel
Sits to the right of the watch shell (or below on narrow screens). Shows supporting information for each phase — described per phase below. Width ~260px. Background transparent. No card wrapper — flows as plain text.

---

## Phase specifications

---

### Phase 1 — Morning brief
**Tab label:** `6:47am · Morning brief`

#### Watch screen
```
Status bar:
  Left:   "6:47" — 11px/400, #FAF9F5
  Right:  dot — 4px circle, #D97757

Content (padding 6px 14px 12px):
  App icon:   32×32px rounded rect (#1e1e1c bg), play triangle (#D97757) inside
  Source:     "Adaptive · morning brief" — 10px/300, #B0AEA5
  Headline:   "Your brief is ready" — 14px/400, #FAF9F5
  Sub:        "18 min · 3 decisions today" — 11px/300, #5a5a58

Actions (bottom):
  [ Listen ]   — primary pill, full width left
  [ Later ]    — secondary pill, full width right
```

#### Behaviour
- Tapping **Listen** → triggers `audio/morning-brief.mp3` to play
- Watch headline changes to `"Playing…"` during playback
- Dot pulses gently (CSS keyframe opacity 1 → 0.3, 1s loop) while audio plays
- Tapping **Later** → shows sub-text `"Remind me in 30 min"` briefly, then resets (no actual timer needed)
- Tapping **Listen** again while playing → pauses audio, resets to ready state

#### Context panel (Phase 1)
```
Eyebrow:    "Inside this brief" — 10px/300, #5a5a58, tracked
Items (3):
  ● Platform launch — sprint blocked, needs you   [dot: #D97757]
  ● Meridian proposal ready for review             [dot: #2e2e2b]
  ● Marcus & Emma — relational check-ins           [dot: #2e2e2b]

Below items:
  "3 actions queued" — 11px/300, #5a5a58
```

---

### Phase 2 — Watch nudges
**Tab label:** `10:23am · Nudges`

Three cards, cycled by clicking left/right arrows or swiping (click-drag acceptable as swipe fallback).

#### Card data

**Card 1 — High urgency**
```
Dot colour:   #D97757
Source:       "Slack · now"
Headline:     "Tom needs your call on Platform timeline"
Sub:          "Sprint blocked. 4 people waiting."
Left bar:     2px solid #D97757
Actions:      [ Reply ]  [ Later ]
```

**Card 2 — Medium urgency**
```
Dot colour:   #6A9BCC
Source:       "Outlook · 8 min ago"
Headline:     "Sarah's Meridian proposal is ready"
Sub:          "Review before EOD to move to contract."
Left bar:     2px solid #6A9BCC
Actions:      [ Flag ]  [ Dismiss ]
              Flag button uses #6A9BCC background
```

**Card 3 — Awareness only**
```
Dot colour:   #2e2e2b
Source:       "HubSpot · 1 hr ago"
Headline:     "Platform bottleneck clearing"
Sub:          "Tom confirmed Q3. Sprint resumed."
Left bar:     2px solid #2e2e2b
Actions:      [ Got it ]  (single button, secondary style, auto width)
```

#### Watch screen structure (all cards)
```
Status bar:
  Left:   current time ("10:23", "10:31", "11:47" per card)
  Right:  dot — colour varies per card

Content:
  Left urgency bar (2px, full height of content area, no border-radius)
  Right content:
    Source label   — 10px/300, #B0AEA5
    Headline       — 13px/400, #FAF9F5, max 2 lines
    Sub            — 11px/300, #B0AEA5

Actions: 2 pill buttons (or 1 for card 3)

Pagination dots: 3 dots below actions
  Active: #D97757, 6px
  Inactive: #2e2e2b, 4px
  Gap: 5px
```

#### Navigation controls
Rendered outside/below the watch shell (not on the screen):
```
  ←   [  ●  ○  ○  ]   →
```
Left/right arrows: `#5a5a58`, 20px. Clicking advances card with 150ms slide transition.

#### Behaviour
- **Reply** (Card 1) → plays `audio/chime.mp3`, button label changes to `"Sent ✓"` for 2s then resets
- **Flag** (Card 2) → button label changes to `"Flagged"`, accent becomes `#5a5a58` (muted confirmation)
- **Dismiss / Got it** → card fades slightly, sub-text updates to `"Dismissed"` for 1.5s
- No audio triggered on cards 2 or 3 — nudge sound is reserved for high-urgency only

#### Context panel (Phase 2)
```
Eyebrow:    "3 nudges · filtered from 12 signals"

For active card, show:
  Source context:  1 sentence explaining why this surfaced
  Card 1: "Tom messaged twice overnight. Engineering sprint starts in 40 min."
  Card 2: "Sarah sent the draft 8 min ago. Meridian close window is today."
  Card 3: "Bottleneck you unblocked this morning. No action needed."

Below:
  "Non-urgent signals held for EOD recap" — 11px/300, #5a5a58
```
Context panel updates as cards cycle.

---

### Phase 3 — Post-workout catch-up
**Tab label:** `12:40pm · Post-workout`

#### Watch screen
```
Status bar:
  Left:   "12:40" — 11px/400, #FAF9F5
  Right:  dot — 4px circle, #6A9BCC (cool — lighter moment)

Content:
  Source:     "Adaptive · post-workout" — 10px/300, #B0AEA5
  Headline:   "Here's what moved while you were out" — 13px/400, #FAF9F5, 2 lines
  Sub:        "5 min · nothing urgent" — 11px/300, #5a5a58

Mini waveform:
  12 bars, 2px wide each, 2px gap
  Heights (px): 6, 10, 14, 10, 8, 12, 16, 12, 8, 10, 6, 8
  All bars: #2e2e2b (unplayed state)
  On play: first 4 bars → #6A9BCC (cool accent, not orange — lighter tone)

Actions:
  [ Play ]   — primary pill, full width left, #6A9BCC background
  [ Later ]  — secondary pill, full width right
```

#### Behaviour
- Tapping **Play** → triggers `audio/postworkout.mp3`
- Waveform animates left to right as "playback" — bars fill to `#6A9BCC` progressively (CSS animation, ~5s loop, for demo purposes)
- Headline changes to `"Listening…"` during playback
- Dot pulses in `#6A9BCC` during playback
- Play button label → `"Pause"`, background stays `#6A9BCC`
- Tapping **Later** → sub-text briefly shows `"Saved for later"`, resets

#### Context panel (Phase 3)
```
Eyebrow:    "While you were out · 47 min"

Items (3 — lighter tone, no urgency dots):
  — Meridian: Sarah sent a follow-up, no reply needed yet
  — LinkedIn: James confirmed the intro, ball in your court this week
  — Platform: Q3 timeline now confirmed across the team

Below:
  "Full recap arrives at 6pm" — 11px/300, #5a5a58
```

---

## Audio placeholder setup

Claude Code should create the `/audio/` folder and three silent placeholder files. If binary generation is not possible, include a clear comment block in the HTML:

```html
<!--
  AUDIO PLACEHOLDERS
  Replace these files with real audio before demo:
  - audio/morning-brief.mp3   (18 min generated brief)
  - audio/postworkout.mp3     (5 min post-workout catch-up)
  - audio/chime.mp3           (short notification sound, ~1s)

  All Audio() objects are initialised on page load.
  Playback errors are caught silently — UI states still trigger correctly
  even if audio files are missing.
-->
```

Audio objects should be initialised at page load with error handling:
```javascript
const audio = {
  brief: new Audio('audio/morning-brief.mp3'),
  postworkout: new Audio('audio/postworkout.mp3'),
  chime: new Audio('audio/chime.mp3')
};
Object.values(audio).forEach(a => {
  a.addEventListener('error', () => {});
});
```

---

## Interaction states summary

| Element | Default | Active / Playing | Confirmed | Dismissed |
|---|---|---|---|---|
| Listen / Play btn | #D97757 / #6A9BCC | Darker shade, "Pause" label | — | — |
| Dot indicator | Solid colour | Pulsing (CSS keyframe) | — | — |
| Waveform bars | #2e2e2b | Fill left→right | — | — |
| Reply btn (card 1) | #D97757 | — | "Sent ✓", 2s then reset | — |
| Flag btn (card 2) | #6A9BCC | — | "Flagged", muted | — |
| Dismiss / Got it | #2a2a28 | — | — | Sub → "Dismissed" |
| Later btn | #2a2a28 | — | Sub updates briefly | — |

---

## Page chrome

Minimal. The focus is the watch.

```
Page background:   #0a0a09 (near-black)
Page padding:      40px top, centred content
Font import:       Google Fonts — Poppins 300, 400

Header (subtle, top of page):
  "Adaptive Intelligence" — Poppins 300, 12px, #3a3a38, tracked
  "Prototype · [phase name]" — updates as phase changes

Footer: none
```

No scrolling on desktop. The entire prototype fits in viewport height.

---

## Responsive behaviour

The prototype is primarily designed for desktop browser preview (1200px+). On narrower screens:
- Context panel moves below the watch shell
- Watch shell centres horizontally
- Tab selector wraps if needed (each tab on its own line acceptable)

No mobile-specific optimisation required for the hackathon build.

---

## Development phases / suggested build order

1. **Shell + navigation** — watch frame, band, crown, phase tabs, page chrome
2. **Phase 1** — morning brief screen, audio trigger, playing state
3. **Phase 2** — nudge cards, pagination, card cycling, per-card context panel
4. **Phase 3** — post-workout screen, waveform animation, audio trigger
5. **Polish** — transitions between phases, interaction micro-states, audio error handling

---

## Success criteria

- Opens in browser with no server, no install
- All three phases accessible via tab navigation
- Play/Listen triggers audio (or fails silently if placeholder missing)
- Watch UI matches mockup design system exactly — colours, type, sizing
- Card cycling works cleanly with pagination feedback
- Context panel updates correctly per phase and per active card
- No console errors on load

---

*End of development brief*
