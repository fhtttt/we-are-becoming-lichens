# We Are Becoming Lichens — repo conventions

A speculative 10-minute presentation. Single deliverable: `index.html` at the repo root — one self-contained slide-video. Image and source resources live in `resources/partN/`. The site is published via GitHub Pages, so root layout matters: `index.html` is what gets served.

This file exists so multiple contributors (and their Claude Code sessions) can extend Parts 3 and 4 without breaking the visual unity already established in Parts 1 and 2.

---

## Thesis (do not soften, do not restate as a generic ecology talk)

> The patterns we see in a lichen colony and in satellite views of human settlement are converging — not by coincidence, but because we are entering a phase where humans, like lichens, exist as **relationships moving across surfaces** rather than populations fixed to places.

Everything in the deck must serve this one idea. "Lichens are cool" is not the argument. The argument is the structural mirror between lichen reproduction and contemporary human mobility.

---

## The four-part structure — DO NOT renumber or merge

| # | Title (working)       | Purpose                                                                                          | Status      |
|---|-----------------------|--------------------------------------------------------------------------------------------------|-------------|
| I | The Visual Rhyme      | Pure looking. Two diptychs (lichen ↔ satellite). Audience must be given time to *recognize*.     | Built       |
| II| Three Strategies      | Lichen reproduces 3 ways: **Soredia, Isidia, Ascospores**. Educational, animated.                | Built       |
| III| Three Mobilities     | The same 3 strategies in human movement: **Digital nomads ↔ Soredia, Climate migration ↔ Isidia, Global refugees ↔ Ascospores**. | **Placeholder — to build** |
| IV| After the Forest      | What a 400-Myr partnership teaches a species mobile for ~200 yrs. Speculative / design call.     | **Placeholder — to build** |

The pairing in Part III is load-bearing. **Do not reorder it, do not swap which human mode pairs with which lichen mode** — the whole talk rests on this mapping:

- Soredia (both partners travel as a bundle) ↔ digital nomad (self-contained, lands anywhere)
- Isidia (a whole fragment of the parent breaks off) ↔ climate migration (community moves together, shorter distance)
- Ascospores (fungus alone, must find an alga on arrival) ↔ refugee (forced dispersal, must locate work/kin/language on arrival)

Part III should structurally mirror Part II's three-column layout so the parallel reads visually, not just verbally.

---

## Visual & design principles

These are intentional choices, not defaults. Match them.

**Tone.** Calm, reflective, slow. The audience needs *time to look*. Default transitions are 1.4s opacity+translate; diptychs in Part I stagger by 0.9s on purpose. Do not speed this up. Autoplay timings (`SLIDE_MS` in the script) are tuned for reflection — 9s opening, 14–16s for the looking/learning slides.

**Color palette** (CSS variables in `:root` at the top of `index.html` — use these, do not introduce new hexes):
- `--bg #efeae0` warm sand · `--bg-deep #e5dfd1` · `--cream #f6f2e8`
- `--ink #2b2a24` · `--ink-soft #4a473d` · `--muted #6b6557`
- `--moss #5a7050` · `--moss-deep #3e5238` · `--sage #a8b89e`
- `--terra #b8825a` · `--stone #8a8478` · `--line #cdc6b5`

The diagram animation colors (`#5a9e28` algae, `#c47a20` fungus, `#8a6fc4` soredia, `#2a8a6a` isidia, `#3a78c0` spore, `#d84444` encounter) are also intentional and shared between Parts II and III — if Part III uses any motif from the lichen modes, **reuse the same color** so the eye links them.

**Typography.** Serif (Iowan Old Style / Palatino) for headings and body. `ui-sans-serif` only for eyebrows, labels, controls, and canvas text. Italics carry quiet emphasis — used for the thesis, the quote, and the closing thought. Don't add a third typeface.

**Texture.** A soft film-grain SVG noise + radial vignette sits on `.stage::before/::after`. Keep it. New slides inherit it automatically — don't paint solid backgrounds that defeat it.

**Layout grammar** (every slide follows this):
1. `eyebrow` line (roman numeral + part title)
2. `h2` headline (one sentence, max ~22ch per line)
3. A right-aligned italic `lede` paragraph (~38–46ch) that sets up the visual
4. The visual itself (diptych / canvas grid / parallel grid)
5. A quiet footer/caption row

Slides 3 and 4 should not invent a new grammar. Use the existing classes (`.slide`, `.eyebrow`, `.head`, `.modes`/`.parallels`, etc.) — extend, don't replace.

---

## What's already in `index.html` — extend, don't rewrite

- The slide controller (`go / next / prev / scheduleAuto`) handles 5 slides today. To add content to slides 3 and 4, edit the `<section class="slide s3">` and `<section class="slide s4">` blocks directly — the controller picks them up by class.
- `startAnims()` runs only on slide 2 (`if(cur===2)`). If Part III adds canvas animations, gate them the same way (`cur===3`) and add a matching `stopAnims` path — otherwise they burn CPU off-screen.
- `SLIDE_MS` is an array indexed by slide number. If you change a slide's length, update its entry.
- The dot-nav titles array (`titles = [...]`) must stay in sync with the slide count and order.

---

## Conventions when adding to Parts III and IV

- **Preserve the placeholder markers** (`[ visuals / data — to come ]`) only as long as that slot is unfilled. When you fill it, remove the marker — don't leave it as decoration.
- **Don't introduce charts/maps that need real data we don't have.** If you need migration numbers, refugee counts, etc., either source them with a citation in the file or use a stylized abstraction (dotted maps, like the `.par .map` pattern already in slide 3).
- **No emojis in slide content.** The deck is quiet on purpose.
- **No external fonts, no CDN scripts, no build step.** This must remain a single double-clickable HTML file. Vanilla JS + inline `<style>` only.
- **Images go under `resources/partN/`** with the same lowercase pattern. Reference them with relative paths from `index.html` (e.g. `resources/part1/image.jpg`).
- **Test by opening the file directly** (`open index.html`). It must work from `file://` *and* from GitHub Pages — no `fetch()`, no module imports, no server required, no absolute URLs.

---

## Git commits

- **Do not** add `Co-Authored-By: Claude <...>` (or any Claude / Anthropic co-author trailer) to commit messages.
- **Do not** add `🤖 Generated with Claude Code` footers to commit messages or PR descriptions.
- Write commit messages as if a human teammate authored them directly. No AI attribution, no tool branding.

---

## Things that have come up already (lessons)

- The four part1 images are **two diptychs**, not four standalone images. Order matters: `image.jpg` (lichen) pairs with `image (1).jpg` (satellite); `image (2).jpg` (lichen) pairs with `image (3).jpg` (settlement). Don't shuffle.
- The Part II animation logic was lifted from `resources/part2/lichen_reproduction_partners.html` and adapted to a DPR-aware `sizeCanvas()` helper. If you tweak the animations, edit them in `index.html` — `resources/part2/*.html` is a source reference, not a runtime dependency.
- Canvas animations must be paused when their slide is not active. We learned this matters for laptop fans during rehearsal-length sessions.
