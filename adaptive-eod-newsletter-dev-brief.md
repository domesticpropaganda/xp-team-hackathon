# Adaptive Intelligence — EOD Newsletter
**Development brief for Claude Code / VS Code**

---

## Overview

A single self-contained HTML file rendering the end-of-day email recap for the Adaptive Intelligence system. Designed to be sent via EmailJS and viewed in any email client, but also openable directly in a browser for demo purposes.

**Output:** `eod-newsletter.html`  
**Stack:** Single HTML file. Inline CSS only — no external stylesheets. No JavaScript required.  
**Constraints:** Email-safe HTML. All styles inline. No web fonts via @import (use system serif/sans stack). Max width 600px centred.

---

## Design philosophy

Editorial and refined. Think FT Weekend meets Apple product page — not a SaaS notification email. Warm off-white background, serif headlines, generous whitespace, one warm accent colour. Every element should feel considered and calm. This arrives at the end of her day; it should feel like a well-edited letter, not a dashboard export.

**Three words:** Quiet. Considered. Closed.

---

## Colour palette

All values inline as hex — no CSS variables (email client compatibility).

| Role | Value |
|---|---|
| Page background | `#FAF9F5` |
| Card / section background | `#FFFFFF` |
| Border / divider | `#E8E6DC` |
| Primary text | `#141413` |
| Secondary text | `#B0AEA5` |
| Accent warm | `#D97757` |
| Accent cool | `#6A9BCC` |
| Dark header bg | `#141413` |
| Light on dark | `#FAF9F5` |

---

## Typography

Email-safe font stacks only:

```
Serif (headlines):     Georgia, 'Times New Roman', serif
Sans (body/labels):    -apple-system, 'Helvetica Neue', Arial, sans-serif
```

| Role | Font | Size | Weight | Colour |
|---|---|---|---|---|
| Header wordmark | Sans | 11px | 400 | `#B0AEA5` |
| Hero headline | Serif | 28px | 400 | `#FAF9F5` |
| Hero sub | Sans | 13px | 300 | `#B0AEA5` |
| Section label | Sans | 10px | 400 | `#B0AEA5` (tracked, uppercase) |
| Item headline | Serif | 16px | 400 | `#141413` |
| Body copy | Sans | 14px | 400 | `#141413` |
| Meta / timestamp | Sans | 11px | 400 | `#B0AEA5` |
| Footer | Sans | 11px | 400 | `#B0AEA5` |

---

## Layout

```
┌─────────────────────────────┐
│         HEADER              │  Dark bg, wordmark + date
├─────────────────────────────┤
│         HERO                │  Dark bg, headline + sub
├─────────────────────────────┤
│      TODAY'S WINS           │  2 item cards, left accent bar
├─────────────────────────────┤
│      ACTION QUEUE           │  3-row table, status badges
├─────────────────────────────┤
│      STILL OPEN             │  2 items, lighter treatment
├─────────────────────────────┤
│      TOMORROW               │  Calendar + one callout
├─────────────────────────────┤
│         FOOTER              │  Muted, minimal
└─────────────────────────────┘
```

**Outer wrapper:** `background: #FAF9F5`, padding 32px 0  
**Inner container:** `max-width: 600px`, `margin: 0 auto`, `background: #FFFFFF`, `border: 1px solid #E8E6DC`  
**No border-radius on outer container** — email clients handle it inconsistently. Sharp edges only on the main card.

---

## Section specifications

---

### Header

**Background:** `#141413`  
**Padding:** 20px 32px  
**Layout:** Two columns — wordmark left, date right  

```
Left:   ADAPTIVE INTELLIGENCE   (sans, 11px, #B0AEA5, letter-spacing: 0.1em)
Right:  Thursday, 16 April      (sans, 11px, #B0AEA5)
```

---

### Hero

**Background:** `#141413`  
**Padding:** 32px 32px 40px  
**Border-bottom:** 3px solid `#D97757`

```
Eyebrow:   "End of day · 18:15"   (sans, 10px, #D97757, tracked)
Headline:  "The loop, closed."    (serif, 28px, #FAF9F5, italic)
Sub:       "Here's how Thursday shaped up."  (sans, 13px, #B0AEA5, margin-top 8px)
```

---

### Today's wins

**Background:** `#FFFFFF`  
**Padding:** 32px  
**Section label:** `TODAY'S WINS`

Two item cards. Each card:
- No background — flows inline
- Left accent bar: `3px solid #D97757`, no border-radius, padding-left 16px
- Headline: serif, 16px, `#141413`
- Body: sans, 14px, `#B0AEA5`, line-height 1.6, margin-top 4px
- Divider between cards: `1px solid #E8E6DC`, margin 20px 0

**Card 1**
```
Headline:  Platform sprint unblocked
Body:      Tom confirmed Q3. The team re-planned by noon and 
           energy visibly shifted. One call made early — 
           that's what today cost you, and what it gave them.
```

**Card 2**
```
Headline:  Meridian is closing
Body:      The client asked for a contract call next week. 
           Sarah owned the room. Your prep this morning 
           gave her the conditions to do that.
```

---

### Action queue

**Background:** `#FAF9F5`  
**Padding:** 32px  
**Section label:** `ACTION QUEUE`

Table layout. 3 columns: Action / Source / Status. Full width, no outer border.

**Table header row:**
- Background: `#141413`
- Text: sans, 10px, `#B0AEA5`, tracked, uppercase
- Padding: 10px 16px
- Columns: Action (50%) / Source (25%) / Status (25%)

**Table rows** (alternating `#FFFFFF` / `#FAF9F5`):
- Padding: 12px 16px
- Action text: sans, 13px, `#141413`
- Source text: sans, 12px, `#B0AEA5`
- Divider: `1px solid #E8E6DC` between rows

**Status badges** — inline pill, no border, padding 3px 10px, border-radius 20px, font 11px:

| Status | Background | Text colour |
|---|---|---|
| Done | `#EAF3DE` | `#3B6D11` |
| Deferred | `#FAF9F5` | `#B0AEA5` — with `1px solid #E8E6DC` border |
| Queued | `#E6F1FB` | `#185FA5` |

**Row data:**

| Action | Source | Status |
|---|---|---|
| Message Tom — Platform Q3 direction | Slack | Done |
| Review Sarah's Meridian proposal | Outlook | Done |
| Note to Marcus | WhatsApp | Done |
| Coffee check-in with Emma | Calendar | Deferred |
| Reply to James — LinkedIn intro | LinkedIn | Queued |
| Frame BD pricing thread | Slack | Queued |

---

### Still open

**Background:** `#FFFFFF`  
**Padding:** 32px  
**Section label:** `STILL OPEN`

Two items. Lighter treatment than wins — no accent bar, muted headline.

Each item:
- Headline: sans, 14px, `#141413`, font-weight 400
- Body: sans, 13px, `#B0AEA5`, line-height 1.6, margin-top 3px
- Divider: `1px solid #E8E6DC` between items

**Item 1**
```
Headline:  Emma — coffee check-in
Body:      She's been carrying the Meridian load solo. 
           She won't ask. First thing tomorrow.
```

**Item 2**
```
Headline:  BD pricing thread
Body:      Drifted without an owner. Needs your framing 
           before the pipeline review. 15 minutes tomorrow 
           closes it cleanly.
```

---

### Tomorrow

**Background:** `#FAF9F5`  
**Padding:** 32px  
**Section label:** `TOMORROW`

**Calendar block:**  
Single row of 3 event pills. Each pill:
- Background: `#FFFFFF`
- Border: `1px solid #E8E6DC`
- Border-radius: 6px
- Padding: 10px 14px
- Time: sans, 10px, `#D97757`
- Label: sans, 13px, `#141413`, margin-top 3px

```
Pill 1:   09:00 · Free — Emma check-in + BD thread
Pill 2:   11:00 · Fixed — Team standup
Pill 3:   14:00 · Meridian prep — Sarah's brief lands tonight
```

**Callout block** (below calendar, margin-top 24px):
- Left border: `3px solid #D97757`, no border-radius, padding-left 16px
- Text: serif, italic, 15px, `#141413`, line-height 1.6

```
"What created momentum today wasn't volume.
It was one clear decision made early.
That's the pattern."
```

---

### Footer

**Background:** `#141413`  
**Padding:** 24px 32px  
**Layout:** Two lines, centred

```
Line 1:   Adaptive Intelligence  (sans, 11px, #B0AEA5)
Line 2:   Generated Thursday 16 April · For your eyes only  (sans, 11px, #5a5a58)
```

---

## Email-safe rules

- All styles inline — no `<style>` blocks (Gmail strips them)
- No `border-radius` above 6px — Outlook clips it
- No `flexbox` or `grid` — use `<table>` for multi-column layouts
- No web font imports — serif/sans system stacks only
- All images as `<img>` with explicit `width` and `height` (none used here)
- `cellpadding="0"` and `cellspacing="0"` on all tables
- `border-collapse: collapse` on all tables
- Explicit `width="600"` on the outer table
- Line-height set explicitly on all text elements — email clients override it
- `mso-line-height-rule: exactly` on `<td>` elements for Outlook
- Background colours set on both `<table>` and `<td>` for redundancy

---

## Demo mode (browser preview)

When opened directly in a browser, the newsletter should render identically to the email version. No JavaScript needed. The file is self-contained.

Add a thin browser-only strip above the outer wrapper:

```
"Adaptive Intelligence · EOD recap · Preview mode"
(sans, 11px, #B0AEA5, text-align centre, padding 12px 0)
```

This strip is visible in browser only — wrap in a `<div>` that email clients will ignore (email clients strip `<div>` padding differently, so the outer table starts immediately).

---

## Success criteria

- Renders correctly in browser without a server
- All sections present and in order
- Action queue table displays all 6 rows with correct status badges
- Accent bars render on win cards and callout quote
- Dark header and hero sections contrast cleanly against light body
- Footer sits flush at the bottom with no extra whitespace
- No external dependencies — fully self-contained

---

*End of brief*
