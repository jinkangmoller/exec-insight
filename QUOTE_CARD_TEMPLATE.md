# Exec Insights — Quote Card SVG Master Template

*The single source of truth for every student-quote visual card. All quote cards are SVG image files following this exact spec, so they are portable (postable to LinkedIn/Substack without screenshots) and visually identical. Reference cards: `visual_card_adnan.svg`, `visual_card_yvonne.svg`.*

---

## Why SVG, not HTML render

Quote cards exist to be **posted as images**. SVG = a real, portable, vector image file Jin can upload anywhere, crisp at any size, no screenshot needed. HTML-rendered cards have no file and require screenshotting. So every quote card is a saved `.svg` in `images/`, referenced from the dashboard as `<img ... class="card-image">`.

---

## Canvas & background

- Size: `1080 × 1080` (square, viewBox `0 0 1080 1080`)
- Background: `<rect width="1080" height="1080" fill="#F5F0E8"/>` (warm cream)

## Fixed elements (identical on every card)

**Eyebrow label** — top-left:
```
<text x="80" y="106" font-family="'Helvetica Neue', Helvetica, Arial, sans-serif"
      font-size="15" font-weight="700" fill="#E8453C" letter-spacing="2">MY EXEC STUDENTS' INSIGHT</text>
<rect x="80" y="114" width="218" height="2.5" fill="#E8453C"/>
```

**Opening quotation mark** — serif "66" style, two single-left-quote glyphs, visible grey:
```
<text x="80"  y="246" font-family="'Times New Roman', Georgia, serif" font-size="95" font-weight="700" fill="#C0BAB0">&#x2018;</text>
<text x="118" y="246" font-family="'Times New Roman', Georgia, serif" font-size="95" font-weight="700" fill="#C0BAB0">&#x2018;</text>
```

**Handle** — bottom-right:
```
<text x="1000" y="1018" text-anchor="end" font-family="'Helvetica Neue', Helvetica, Arial, sans-serif"
      font-size="18" fill="#1A1A1A" opacity="0.5" letter-spacing="1">@JINKANGMOLLER</text>
```

**Coral dot** — bottom-left:
```
<circle cx="80" cy="1012" r="5.5" fill="#E8453C" opacity="0.85"/>
```

## Variable elements (per card)

**Quote text** — Georgia bold serif, `fill="#1A1A1A"`, left-aligned at `x="80"`.
- Use a **punchy trimmed core** of the verbatim, NOT the full quote. 2–4 short lines. The full quote lives in the dashboard card-meta below; the SVG carries the punch.

**Trimming rule — preserve the student's voice (Jin's standard):**
- Default to the actual verbatim. Light tightening is allowed ONLY to make the card readable, meaningful, and useful.
- **Preserve the student's voice, register, and key verbs.** Tighten filler; never paraphrase into a different tone.
- Keep the first-person reflective register if the original has it. Keep the verb that carries the thought.
- Remove only: attribution prefixes that live in the card-meta anyway ("DT helped me…"), redundant filler, trailing clauses that dilute the punch.
- Example (correct): "DT helped me reimagine hustling for cold, hard KPIs into a creative process…" → "Now I reimagine / cold, hard KPIs / into a creative process / that evokes our whole being." — keeps her voice and the verb "reimagine".
- Example (WRONG): trimming "DT helped me reimagine" to "Hustling for" — this strips her reflective first-person voice and the central verb. Do not do this.
- When unsure whether a trim shifts the tone, keep more of the verbatim.
- **Adaptive font size by line count:**
  - 2–3 short lines → `font-size="66"` to `72"`, line spacing ~`82–90`
  - 4 lines → `font-size="54"` to `58"`, line spacing ~`68`
  - Never smaller than 50px. If the quote needs more than 4 lines at 50px, trim it harder — the card should be punchy, not a paragraph.
- First text baseline starts around `y="330"` (one quote-mark height below the mark).

**Yellow highlight** — a `<rect>` placed BEHIND the highlighted word(s), drawn before the text line so the text sits on top:
```
<rect x="76" y="[lineBaseline - fontSize*0.82]" width="[phraseWidth]" height="[fontSize*1.15]" fill="#FFE600" rx="3"/>
```
- Estimate `phraseWidth ≈ (chars in phrase) × (fontSize × 0.55)` for Georgia bold, then add ~16px padding.
- Position the rect's `x` to start where the highlighted phrase begins on its line (if the phrase is mid-line, offset x by the width of the preceding text on that line).
- It is fine to break the line so the highlighted phrase sits at the start of its own line — that makes positioning clean and looks strong.

**Thin rule** — short divider under the last quote line:
```
<rect x="80" y="[lastLineBaseline + ~44]" width="72" height="1.5" fill="#1A1A1A" opacity="0.25"/>
```

**Attribution** — italic, grey, under the rule:
```
<text x="80" y="[ruleY + 36]" font-family="Georgia, 'Times New Roman', serif"
      font-size="22" fill="#7A7168" font-style="italic">[First Name] — [Month Year] cohort</text>
```
- Format: `[Name] — [Month Year] cohort` (e.g. "Adnan — Feb 2026 cohort"). Use the student's display name as it appears in the card data.

## File naming

`images/visual_card_[firstname_lastname_or_slug].svg` — lowercase, underscores. One file per quote card.

## Dashboard reference

Each quote card in the dashboard HTML references its SVG exactly like Adnan:
```html
<img src="./images/visual_card_[slug].svg" alt="[Name]" class="card-image">
```
(Replaces the old `<div class="card-quote-visual">…</div>` HTML render.)

## The yellow-highlight caveat (the one fiddly bit)

SVG cannot auto-flow a highlight the way HTML `<mark>` can. The highlight rectangle width/position is estimated from character count. If a rendered highlight looks slightly too wide or narrow, nudge the rect `width` / `x` by ~10–20px. This is the only part that occasionally needs a visual tweak — everything else is fixed template.

---

*CCO owns generating these (content). CPO owns the dashboard structure (container). Reference cards: visual_card_adnan.svg, visual_card_yvonne.svg.*
