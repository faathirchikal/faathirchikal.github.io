---
name: Faathir Chikal Asyuraa
description: A job-hunt portfolio set as an operations sheet, with every project drawn as its mechanism.
colors:
  ground: "#f5f3ee"
  surface: "#fbfaf7"
  ink: "#2a2926"
  muted: "#5e5a53"
  rule: "#dad5cb"
  accent: "#2f6f6a"
  accent-tint: "#e2ece9"
  live: "#6e8f78"
  trial: "#a88b52"
  ground-dark: "#191917"
  surface-dark: "#22221f"
  ink-dark: "#ebe7df"
  muted-dark: "#aba59b"
  rule-dark: "#3a3934"
  accent-dark: "#7fb8b0"
  accent-tint-dark: "#23302e"
  live-dark: "#8fae98"
  trial-dark: "#cbb07a"
typography:
  display:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "clamp(2.75rem, 7vw, 4.5rem)"
    fontWeight: 700
    lineHeight: 0.95
    letterSpacing: "-0.02em"
  headline:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "clamp(2rem, 6vw, 3rem)"
    fontWeight: 700
    lineHeight: 1.12
    letterSpacing: "-0.015em"
  section:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "clamp(1.625rem, 3.5vw, 2.125rem)"
    fontWeight: 700
    lineHeight: 1.12
  title:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "1.3125rem"
    fontWeight: 700
    lineHeight: 1.15
  lead:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "1.25rem"
    fontWeight: 500
    lineHeight: 1.3
  body:
    fontFamily: "Barlow, system-ui, sans-serif"
    fontSize: "1.125rem"
    fontWeight: 400
    lineHeight: 1.6
    fontFeature: "tnum"
  label:
    fontFamily: "Barlow Semi Condensed, Arial Narrow, sans-serif"
    fontSize: "0.9375rem"
    fontWeight: 700
    lineHeight: 1
rounded:
  hairline: "2px"
  inset: "3px"
  tag: "4px"
  card: "6px"
  pill: "999px"
spacing:
  gutter: "16px"
  measure: "42rem"
  wide: "68rem"
  card-pad: "1rem"
  grid-gap: "1rem"
components:
  button:
    backgroundColor: "transparent"
    textColor: "{colors.ink}"
    typography: "{typography.label}"
    rounded: "{rounded.card}"
    padding: "0.4rem 0.85rem"
  button-hover:
    textColor: "{colors.accent}"
  button-solid:
    backgroundColor: "{colors.accent}"
    textColor: "{colors.surface}"
    rounded: "{rounded.card}"
    padding: "0.4rem 0.85rem"
  chip-live:
    textColor: "{colors.live}"
    rounded: "{rounded.pill}"
    padding: "0.35rem 0.6rem"
  chip-trial:
    textColor: "{colors.trial}"
    rounded: "{rounded.pill}"
    padding: "0.35rem 0.6rem"
  chip-building:
    textColor: "{colors.accent}"
    rounded: "{rounded.pill}"
    padding: "0.35rem 0.6rem"
  chip-research:
    textColor: "{colors.muted}"
    rounded: "{rounded.pill}"
    padding: "0.35rem 0.6rem"
  tag:
    backgroundColor: "{colors.accent-tint}"
    textColor: "{colors.muted}"
    rounded: "{rounded.tag}"
    padding: "0.2rem 0.5rem"
  pillar:
    backgroundColor: "{colors.accent-tint}"
    textColor: "{colors.ink}"
    rounded: "{rounded.pill}"
    padding: "0.4rem 0.8rem"
  card:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.ink}"
    rounded: "{rounded.card}"
    padding: "1rem"
  panel:
    backgroundColor: "{colors.surface}"
    rounded: "{rounded.card}"
    padding: "clamp(1rem, 3vw, 1.75rem)"
  nav-bar:
    backgroundColor: "{colors.ground}"
    textColor: "{colors.ink}"
    height: "3.5rem"
---

# Design System: Faathir Chikal Asyuraa

## Overview

**Creative North Star: "The Operations Sheet"**

The site reads as one person's operations sheet: a dated swimlane of what shipped, what was trialled and what is being built, and every project drawn as its mechanism in a small schematic line drawing. Warm neutral paper by day, graphite by night, soft charcoal ink, and one muted deep teal that marks links, the current place, and the accent stroke in every drawing. Nothing shouts; the structure does the work.

Density is moderate and tabular. Content sits on flat paper panels with hairline rules and a small 6px corner; there are no shadows and no gradients beyond a single tint wash behind card art. A condensed grotesque (Barlow Semi Condensed) sets every frame element: names, headings, chips, buttons, nav, labels inside drawings. Barlow sets the reading text. Status is always visible and always in the same four-mark vocabulary.

It refuses the hero-plus-skill-bars portfolio and the loud single-colour poster band. The site runs with no JavaScript: the mobile menu is a native disclosure, the timeline is server-rendered SVG.

**Key Characteristics:**
- Warm paper / graphite grounds, charcoal ink, one teal accent.
- Condensed frame type, humanist reading type, tabular numerals everywhere.
- Flat panels: 1px hairline borders, 6px corners, no shadows.
- A four-mark status system (live, trial, building, research) shared by chips and the timeline.
- Schematic line drawings in three strokes, coloured only by tokens, so they follow the theme.

## Colors

A warm, low-chroma paper palette with exactly one hue doing the pointing.

### Primary
- **Workshop Teal** (`accent`; dark `accent-dark`): links on hover and their underline (at 45% mix at rest), the current nav item's 2px underline, solid buttons, the "building" chip, the timeline's "now" line, card hover borders and titles, focus outlines (3px), text selection, and the 2.5px accent stroke in every drawing.
- **Teal Wash** (`accent-tint`; dark `accent-tint-dark`): tag and pillar backgrounds, inline code, timeline role bands, illustration fills, and (mixed 45% into surface) the art strip of a case card.

### Secondary (status marks only)
- **Sage** (`live`; dark `live-dark`): live, ran, or replaced work. Filled dot in chips, filled bar in the timeline.
- **Ochre Grey** (`trial`; dark `trial-dark`): work in client or acceptance trial. Half-filled dot in chips, 45%-mix bar with solid stroke in the timeline.

### Neutral
- **Warm Paper** (`ground`; dark `ground-dark`): page background, and the sticky bar at 92% over a blur.
- **Card Paper** (`surface`; dark `surface-dark`): every panel, card, figure and the open mobile menu; also the text colour on teal.
- **Soft Charcoal** (`ink`; dark `ink-dark`): body text, headings, button outlines, the 1.5px heavy rules under group and table heads, the main illustration stroke.
- **Graphite Note** (`muted`; dark `muted-dark`): ledes, questions, captions, crumbs, tags, years and role labels, the 1.5px secondary illustration stroke, the research chip.
- **Hairline** (`rule`; dark `rule-dark`): every 1px border and divider, the timeline year grid, scrollbar thumb.

### Theming
Light is the default on `:root`. Dark applies under `prefers-color-scheme: dark` unless the root carries `data-theme="light"`, and is forced by `data-theme="dark"`; both dark blocks repeat the same nine values. Only these nine variables change between themes; components never branch on theme.

### Named Rules
**The One Teal Rule.** Teal is the only hue that points. Sage and ochre exist only as status marks; never use them for links, emphasis or decoration, and never add a second accent.

**The Status Legibility Rule.** Status chip text must hold 4.5:1 on surface. The shipped light `live` and `trial` values do not yet (about 3.4:1 and 3.1:1); darken them before adding more status text, do not copy them to new text roles.

## Typography

**Display / Frame Font:** Barlow Semi Condensed (with Arial Narrow, sans-serif), weights 500, 600, 700
**Body Font:** Barlow (with system-ui, sans-serif), 400, 600, 700 and 400 italic

**Character:** A condensed engineering grotesque frames the sheet, and its wider sibling carries the reading; same skeleton, two widths. All numerals are tabular.

### Hierarchy
- **Display** (700, clamp(2.75rem, 7vw, 4.5rem), 0.95, -0.02em): the name on the home page only.
- **Headline** (700, clamp(2rem, 6vw, 3rem), 1.12, -0.015em, max 24ch): case study, essay and About page titles.
- **Section** (700, clamp(1.625rem, 3.5vw, 2.125rem)): home section heads. Prose h2 is 1.5rem with a hairline above; h3 and story/group heads are 1.25rem.
- **Title** (700, 1.3125rem, 1.15): case-card titles; 1.125rem for pager, More-work items and the bar name.
- **Lead** (500, 1.25rem, 1.3, muted): the case-page question (max 36ch) and, at clamp(1.25rem, 2.4vw, 1.5rem), the home positioning line (max 30ch).
- **Body** (400, 1.125rem, 1.6, tabular): reading text at a 42rem measure; ledes in muted at 46ch; captions and item copy at 0.9375rem.
- **Label** (700 or 500, 0.875–1.0625rem): nav (500, 1.0625rem), buttons (700, 1rem), chips (700, 0.9375rem), tags (500, 0.9375rem), crumbs and jump links (500, 1rem). Drawing labels are 13px 600, or 700 in teal.

### Named Rules
**The Two Widths Rule.** Condensed for anything you scan (names, heads, chips, nav, labels, table heads and first columns); regular Barlow for anything you read. Never set a paragraph in the condensed face.

**The No-Uppercase Rule.** Labels are sentence case at normal tracking. Hierarchy comes from width and weight, not caps or letter-spacing.

## Layout

Two containers, both padded by a 16px gutter: a 68rem **wide** column for the bar, home and footer, and a 42rem **measure** column for case studies, the essay and About. Home sections are spaced by clamp(2.5rem, 6vw, 4rem); the footer sits 5rem below content behind an inset hairline.

- **Home intro:** two equal columns (text, then a 2×2 drawing mosaic), collapsing to one column at 52rem.
- **Timeline:** full wide column inside a panel; its SVG holds a 900px minimum and scrolls horizontally. Below 60rem it opens scrolled to the recent end.
- **Case cards:** auto-fill grid, minimum min(100%, 20rem), 1rem gap.
- **Other projects:** two CSS columns (min 22rem, 2.5rem gap), groups never split.
- **Flagship story:** stacked rows split 11rem heading / content, collapsing to one column at 40rem.
- **Case page order:** crumb, title, question, chip and tags, hero figure, jump links, prose, two-up pager (one-up below 34rem).
- **Mobile:** below 52rem the nav and external buttons fold into the Menu disclosure. Below 40rem hero and story drawings keep a 34rem minimum, scroll sideways, and print a muted "Scroll sideways" hint.

Anchors land under the sticky bar via a 4.5rem scroll padding.

## Elevation & Depth

Flat. Depth is tonal: the page is `ground`, every container is a step lighter (`surface`) with a 1px `rule` border. Hover answers with a teal border, never a lift. The only translucency is the sticky bar (ground at 92%, saturate 1.2, 8px blur). The flagship story uses 1px gaps over a `rule` background to draw its internal dividers.

**The Paper-Flat Rule.** No box-shadows, drop shadows or text shadows anywhere, including hover and focus states.

## Shapes

Three radii, each with one job: a small 6px corner on cards, panels, figures, buttons and the menu summary; 999px pills for status chips and pillar chips; 4px for stack tags (timeline role bands use rx 4 to match). Inline code is 3px, a raster inside a hero is 3px, focus outlines 2px. Borders are 1px `rule` for containers, 1.5px for interactive outlines and chip rims, and 1.5px `ink` for the heavy rule under group heads, table heads and the glance list.

## Components

### Buttons
Compact and outlined, like a form field on the sheet.
- **Shape:** gently cornered (6px), 1.5px border.
- **Outline (default):** transparent, ink border and text, frame face 700 1rem, padding 0.4rem 0.85rem.
- **Solid:** teal fill and border, surface text. One solid button per group, for the main action (CV, Read the case study).
- **Hover / Focus:** outline turns teal (border and text); solid brightens 8%. Focus is a 3px teal outline, 2px offset.

### Chips
- **Status chip:** pill, frame face 700 0.9375rem, 1.5px rim at 45% of its colour, with a 0.6rem leading mark: **live** sage, filled dot; **trial** ochre, half-filled dot; **building** teal, empty ring; **research** muted, filled rounded square with a plain rule rim. Status text is the page's own words; the include maps it to a kind (live/replaced, trial, research, else building).
- **Tag:** 4px corner, teal wash, muted 500 text. For stack names only.
- **Pillar:** pill, teal wash, ink 600 text, small teal dot. Home intro only.

### Cards / Containers
- **Case card:** 6px, surface, 1px rule border, no padding on the shell. Art strip on top (teal wash 45% over surface, rule underline, 1rem padding) holds the 320×180 drawing; body below at 1rem with question (muted lead), title, status chip, then up to four tags pinned to the bottom. Hover: teal border and teal title.
- **Panel:** the same shell with clamp(1rem, 3vw, 1.75rem) padding, used by the timeline, case hero figure and mosaic tiles. Figcaptions are 0.9375rem muted.

### Navigation
- **Sticky bar:** 3.5rem, translucent ground, rule underline. Name left (700), nav links (500, 1.0625rem) with a 2px teal underline on `aria-current`, LinkedIn outline and CV solid buttons right.
- **Menu:** below 52rem, a `details` disclosure with a 6px outlined "Menu" summary; it opens a full-width surface sheet of links separated by hairlines, adding CV and LinkedIn.
- **Jump links:** frame 500 1rem between two hairlines, a muted "Jump to" lead, teal links, underline on hover.
- **Pager:** two surface panels, Previous / Next in muted 500, the neighbour's thumbnail (max 7rem tall), its title in frame 700. The last case links back to all work.

### Swimlane Timeline (signature)
A server-rendered SVG in a panel: lanes per employer and study, year gridlines in rule, role bands in teal wash with muted labels, and projects as bars in the status grammar (live filled sage, trial ochre at 45% with ochre stroke, building surface with dashed 4 3 teal stroke). A teal "now" line caps it. Project labels are frame 600 17px; linked ones turn teal and underline on hover. A legend of status chips sits below.

### Flagship Story
Stacked rows in one bordered panel divided by 1px gaps. Each row: a 1.25rem heading with a teal 500 subtitle beneath it, then the copy, optional hero drawing (max 34rem) and button pair.

### More-work Groups and Lists
Group head 1.25rem over a 1.5px ink rule; items are a 3.5rem icon drawing beside title (frame 700 1.125rem), a 0.9375rem muted line and a 0.875rem status line, separated by hairlines. Link lists (essay, demos) use the same hairline rhythm with 1.25rem frame links. The **glance** list on About is an 8rem / 1fr definition grid under a 1.5px ink rule with frame-bold terms.

### Illustrations
Inline SVG includes, coloured only through the drawing classes so they follow the theme:
- **Strokes:** main line ink 2px; secondary line muted 1.5px; accent line teal 2.5px; all with round caps and joins. Dashes 5 5 for projected or pending paths.
- **Fills:** teal wash for areas, surface with a 2px ink stroke for objects, ink or teal for dots. Labels 13px frame, muted 600 or teal 700.
- **Sizes:** case thumbnails 320×180; case heroes 720 wide (240–300 tall); More-work icons 64×48. The mosaic hides drawing labels.
- **Content:** generic, synthetic schematics of the mechanism only. One teal accent stroke carries the story in each drawing.

### Footer
Wide column, inset hairline above, frame face 500: name in 700 pushed left, then CV, email, LinkedIn and GitHub, wrapping as needed.

## Do's and Don'ts

### Do:
- **Do** take every colour from the nine theme variables so light, dark and `data-theme` all hold without extra rules.
- **Do** give every project a status chip from the four kinds, near the top, in the page's own words.
- **Do** draw new illustrations with the drawing classes only: 2px ink, 1.5px muted, 2.5px teal, round joins, at 320×180, 720 wide, or 64×48.
- **Do** use the 6px corner for containers and buttons, 999px for chips and pillars, 4px for tags.
- **Do** answer hover with a teal border or teal text, and focus with a 3px teal outline.
- **Do** set scannable labels in Barlow Semi Condensed and reading text in Barlow at a 42rem measure.

### Don't:
- **Don't** add shadows of any kind; depth is ground, surface and hairline.
- **Don't** put kicker or eyebrow labels above headings; a subtitle goes beneath, as in the story rows.
- **Don't** introduce a second accent hue or use sage and ochre outside status marks.
- **Don't** add JavaScript; use `details`, anchors and server-rendered SVG.
- **Don't** hard-code colours in SVGs or draw real screenshots, internal systems or business figures; drawings are synthetic schematics.
- **Don't** set labels in uppercase or with letter-spacing.
