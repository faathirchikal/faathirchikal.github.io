---
name: Faathir Chikal Asyuraa
description: A job-hunt portfolio set like a printed rail timetable.
colors:
  yellow: "#f6c800"
  on-yellow: "#141414"
  ink: "#141414"
  muted: "#555049"
  ground: "#fbfaf7"
  rule: "#d8d4cb"
  signal: "#c8102e"
  ink-dark: "#ece9e2"
  muted-dark: "#a8a399"
  ground-dark: "#121211"
  rule-dark: "#34322e"
  signal-dark: "#ff5a6e"
typography:
  display:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "clamp(2.5rem, 8vw, 4.25rem)"
    fontWeight: 700
    lineHeight: 0.95
    letterSpacing: "-0.02em"
  headline:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "clamp(2rem, 6vw, 3rem)"
    fontWeight: 700
    lineHeight: 1.05
    letterSpacing: "-0.015em"
  title:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "1.5rem"
    fontWeight: 700
    lineHeight: 1.15
  row-title:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "1.3125rem"
    fontWeight: 700
    lineHeight: 1.2
  body:
    fontFamily: "Barlow, system-ui, sans-serif"
    fontSize: "1.125rem"
    fontWeight: 400
    lineHeight: 1.6
    fontFeature: "tnum"
  body-small:
    fontFamily: "Barlow, system-ui, sans-serif"
    fontSize: "1rem"
    fontWeight: 400
    lineHeight: 1.5
  status:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "1.0625rem"
    fontWeight: 700
    lineHeight: 1.25
    letterSpacing: "0.04em"
  label:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "0.875rem"
    fontWeight: 700
    letterSpacing: "0.08em"
rounded:
  none: "0"
  code: "3px"
spacing:
  gutter: "16px"
  row: "1rem"
  column-gap: "1.25rem"
  section: "2.5em"
  footer: "4rem"
components:
  masthead-band:
    backgroundColor: "{colors.yellow}"
    textColor: "{colors.on-yellow}"
    typography: "{typography.display}"
    padding: "clamp(2.5rem, 8vw, 4.5rem) 0 clamp(1.5rem, 4vw, 2.25rem)"
  running-head:
    backgroundColor: "{colors.yellow}"
    textColor: "{colors.on-yellow}"
    padding: "0.75rem 0"
  column-head:
    backgroundColor: "{colors.ink}"
    textColor: "{colors.ground}"
    typography: "{typography.label}"
    padding: "0.3rem 0.5rem"
  departure-row:
    typography: "{typography.row-title}"
    padding: "1rem 0.5rem"
    rounded: "{rounded.none}"
  status-notice:
    textColor: "{colors.ink}"
    typography: "{typography.status}"
    padding: "0.5rem 0 0.25rem"
---

# Design System: Faathir Chikal Asyuraa

## Overview

**Creative North Star: "The Printed Timetable"**

The site is set like an operations timetable: a yellow departure-poster band on top, then a quiet warm-white sheet of ruled rows. Square, flat, and text-only. Hierarchy comes from the condensed face, weight, and horizontal rules, never from boxes or depth.

It works in light and dark (via `prefers-color-scheme`). The yellow band stays yellow in both; only the sheet below changes.

**Key Characteristics:**
- One full-width yellow band per page; everything else is ink on ground.
- Two faces: condensed for the frame, regular Barlow for reading.
- Horizontal rules do all the structuring; no cards, no shadows, square corners.
- Status set in bold condensed caps where a timetable puts the time.
- Tabular figures everywhere.

## Colors

A near-monochrome warm sheet with one loud yellow and one small red.

### Primary
- **Departure Yellow** (yellow): the masthead band and text selection. Text on it is always On-Yellow ink.

### Secondary
- **Signal Red** (signal; signal-dark in dark mode): link underline on hover and the focus outline. Nothing else.

### Neutral
- **Timetable Ink** (ink / ink-dark): body text, headings, the 2px rules over sections and the footer, the column-head bar.
- **Faded Ink** (muted / muted-dark): row summaries, the case-study summary line, field labels.
- **Warm Paper** (ground / ground-dark): page background.
- **Hairline** (rule / rule-dark): 1px row rules, table cell rules, link underlines at rest. In dark mode it also fills the column-head bar.

### Named Rules
**The One Band Rule.** Yellow appears as one full-width band at the top of a page (and as the selection color). It never becomes a button, badge, or highlight inside the sheet.

**The Signal Only Rule.** Red marks interaction (hover, focus) and nothing else; it never colors static text.

## Typography

**Frame Font:** Barlow Semi Condensed (with Arial Narrow), weights 500 and 700
**Reading Font:** Barlow (with system-ui), weights 400 and 600, plus italic 400

**Character:** Condensed signage over a plain, even text face, like a printed timetable's heads over its notes.

### Hierarchy
- **Display** (700, clamp 2.5–4.25rem, 0.95): the name on the home band only.
- **Headline** (700, clamp 2–3rem, 1.05, max 22ch): case-study and essay titles.
- **Title** (700, 1.5rem, 1.15): section heads, each with a 2px ink rule above. Subsections use 1.25rem.
- **Row title** (700, 1.3125rem, 1.2): project links in the departure table.
- **Body** (400, 1.125rem, 1.6): all reading text, in a 40rem column.
- **Body small** (400, 1rem, 1.5, muted): row summaries and table text.
- **Status** (700, 1.0625rem, +0.04em, uppercase): the status column.
- **Label** (700, 0.875rem, +0.08em, uppercase): column heads. The "Status" field label in the notice is the same idea at weight 500, muted.
- **Frame 500** (1rem–1.375rem): the home headline, the case-study summary line, the footer links.

### Named Rules
**The Two Faces Rule.** Condensed for anything that frames or labels (band, titles, heads, status, footer); Barlow for anything read as sentences.

**The Caps Belong In Columns Rule.** Uppercase tracked type is for column heads, status values, and field labels in a row. It is not a kicker above a title.

## Layout

One centered column: `max-width` of the measure (40rem) plus a 16px gutter each side. No grid beyond the departure table, which is two columns (9.5rem status, then the rest, 1.25rem gap); below 34rem the status column narrows to 7.5rem and type steps down. Sections open with 2.5em of space and a rule. The footer sits 4rem below the content behind a 2px ink rule.

## Elevation & Depth

Flat. No shadows anywhere. The only filled surfaces are the yellow band and the column-head bar; separation comes from rules (1px hairline between rows, 2px ink over sections).

## Shapes

Square corners throughout. The single exception is inline code (3px radius on a faint hairline tint), which keeps code spans legible inside prose.

## Components

### Masthead band
Home: full-width yellow with the name in Display, the headline in frame 500 beneath, and a bold CV link. Inner pages: the same yellow reduced to a running head (name left, CV link right, bold condensed, 0.75rem padding). Links on yellow use a 35% ink underline that goes solid on hover; focus outline is ink, not red.

### Departure table
A borderless list ruled as a timetable: an ink column-head bar ("Status / Project", Label type, light text), then rows separated by 1px hairlines with a closing hairline. Left cell: status in Status caps. Right cell: Row-title link and a muted one-line summary.

### Status notice
Directly under a page title: a 2px ink rule, then a muted caps field label "Status" followed by the status in bold condensed. The first paragraph after it becomes a muted frame-500 summary line closed by a hairline.

### Links
Inherit text color, 2px underline in Hairline at 0.2em offset; on hover the underline turns Signal Red; on focus a 3px Signal Red outline at 2px offset.

### Tables (in prose)
Full width, collapsed, left-aligned, 1rem text. Header cells in bold condensed over a 2px ink rule; body cells over 1px hairlines.

### Footer
A wrapping row of frame-500 links (CV, email, LinkedIn, GitHub) under a 2px ink rule.

### Figures
Project diagrams are standalone SVGs in `diagrams/`, drawn top to bottom at no more than 600 wide. Each carries its own `<style>`: 1.5px ink lines and boxes, frame-face labels, muted arrow labels, and a `prefers-color-scheme` block that swaps ink and muted for dark mode. Yellow fills only the element the case study turns on, and its text stays ink in both modes. Thesis figures are the paper and poster PNGs in `figures/`, shown as published. Every figure links to its own file so a phone reader can open it full size.

## Do's and Don'ts

### Do:
- **Do** keep the yellow band as the only full-width color field, with ink text on it in both themes.
- **Do** structure with rules: 1px Hairline between rows, 2px Timetable Ink above sections and the footer.
- **Do** put a project's status in the left column in bold condensed caps, in plain words.
- **Do** keep reading text in Barlow 400 at 1.125rem/1.6 inside the 40rem column.
- **Do** load any new weight before using it; only Barlow 400/600/700 and Semi Condensed 500/700 ship.

### Don't:
- **Don't** put content in bordered or shadowed cards.
- **Don't** use Signal Red for anything but hover and focus.
- **Don't** add shadows or rounded corners (inline code excepted).
- **Don't** use tracked caps as a kicker or eyebrow above a heading.
