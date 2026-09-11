# Visual theme: Learning Music

Every generated page uses the look of learningmusic.ableton.com, Ableton's own learn-by-doing site. It is built for exactly this kind of page: one idea per screen, an interactive widget in the middle, a calm dark ground that keeps attention on the bright interactive cells. The skeleton implements it; this file records where each value comes from so a page that extends the CSS stays on-theme.

Source of truth is the site's code, not its screenshots. Values below were read from `https://learningmusic.ableton.com/css/main.css` and confirmed as computed styles on a live lesson page (`make-beats.html`) at 400, 700 and 1200px. If a value here and the site disagree, the site wins: measure it with a headless browser (`getComputedStyle`) rather than guessing from a picture.

## Scale

- Root `html` font-size **20px**, line-height 1.5. Everything else is in rem of that.
- Spacing scale: **15 / 30 / 40 / 60 / 80px** = `.75 / 1.5 / 2 / 3 / 4rem`. Nothing in between.
- Breakpoints are written in em of the browser default (16px), so: `28.8em` = 461px, `38.4em` = 614px, `51.2em` = 819px, `64em` = 1024px.

## Type

| Element | < 614px | ≥ 614px | Notes |
|---|---|---|---|
| body text (`.body-text`) | 16 / 24 | 20 / 30 | weight 400 |
| h1 | 32 / 38.4 (1.2) | 48 / 57.6 (1.2) | bold, margin 30px top and bottom |
| h2 | 24 / 33.6 (1.4) | 32 / 38.4 (1.2) | bold, margin-top 60px |
| header crumb (`1/10: Beats`) | 16 / 24 | 20 / 30 | bold |
| small labels, footer | 14 / 21 | 14 / 21 | column labels are `#b3b3b3`, bold |
| widget labels | 16 / 24 | 16 / 24 | bold, inside the widget panel |

Line length: headings, paragraphs and lists are capped at `max-width: 24rem` (480px). Text is left-aligned.

Font: the site self-hosts **Futura PT** (FTN45 book 400, FTN55 medium 700). It is a licensed Adobe font and cannot be embedded. The skeleton stacks `"Futura PT", Futura, Jost, "Helvetica Neue", Helvetica, Arial, sans-serif` and embeds **Jost** (OFL-1.1, a geometric sans on the Futura model) as a base64 variable woff2, about 35KB, so a machine without Futura still renders a geometric sans rather than a humanist fallback.

## Layout

| Region | < 461 | ≥ 461 | ≥ 819 | ≥ 1024 |
|---|---|---|---|---|
| container side padding | 30px | 40px | 80px | 80px |
| header padding-top / min-height | 30px / — | 30px / — (≥ 614: 40px / 140px) | 40px / 140px | 60px / 180px |
| side rail (`.nav-title__button`) | 26px menu button, top-right | same | fixed left rail, **80px** wide, `#4d4d4d`, rotated bold label 16px | label 20px |
| body padding-left | 0 | 0 | 80px (the rail) | 80px |
| prev/next block padding | 60px top / 30px bottom | same | 60px all sides | 80px top and bottom, 120px sides |
| prev/next direction | column, **Next first** | column | column | row, Previous left, Next right (`margin-left: auto`) |

The skeleton has no chapter menu, so it omits the mobile menu button and shows the rail only from 819px, with the course title as its rotated label.

## Components

- **Button** (`.btn`): inline-flex, centered, bold, no border, `border-radius: 0`, `user-select: none`. Default (`.btn--default`) is `#4d4d4d` with white text. Link style (`.btn--link`) is transparent with white text. There is no louder button than grey; emphasis comes from position, not color.
- **Large button** (`.btn--lg`, used for Next / Previous): 16 / 24 with padding **24px 32px** (72px tall) below 614px; 20 / 30 with padding **40px 60px** (110px tall) from 614px. `.btn--wide` makes it full width. The skeleton uses these exact sizes for Next / Previous and for the in-screen Check / Back pair.
- **Widget panel** (`.widget__controls`): `#4d4d4d`, padding **15px**, 16 / 24 bold. The widget sits 30px below the text above it.
- **Transport button**: 60 × 60px, white, black glyph.
- **Slider** (`.range`): 25px tall, 10px side padding, 4px track in the ground grey `#666`, 20px round white thumb.
- **Sequencer grid**: lanes 60px tall with 2px `#666` gaps, idle cell `#858585`, active cell goldfish `#fed134`, beat lines `#4d4d4d`, bar highlight white at 10% opacity, column labels 14px bold `#b3b3b3` (beat columns) / `#858585` (others).

## Color

Greys carry the layout; accents carry state. Every accent block takes black text.

| Token (site class) | Hex | Site usage | Skeleton usage |
|---|---|---|---|
| base (`.bg--base`) | `#666666` | page ground | ground |
| gray-30 (`.bg--button`) | `#4D4D4D` | buttons, widget panel, side rail | panels, sandbox, buttons, callouts |
| gray-45 | `#757575` | | |
| gray-50 | `#858585` | idle cells, column labels | labels, untested state bar |
| gray-70 (`.bg--secondary`) | `#B3B3B3` | beat labels | muted text |
| gray-90 | `#D9D9D9` | | |
| white | `#FFFFFF` | text, transport button, slider thumb | text |
| goldfish | `#FED134` | active cells | progress bar, chosen option, hints, "revealed" |
| tiffany | `#00D2BE` | accent | pass state |
| salmon | `#FF8389` | accent, top banner | wrong / failed state |
| antarctica | `#6DCBFF` | accent | info |
| lavender | `#D5B3FF` | accent | reference answer block |
| razzmatazz | `#FF2956` | accent | reserved |

## Light variant

`<html data-theme="light">` switches to the ableton.com look: black text on `#F3F3F3`, white panels, black buttons, `#0000FF` primary. Use only if a sandbox needs a light ground. The default is dark.

## Verifying a page against the site

`getComputedStyle` on the site and on the page, same element pairs, same widths. The pairs that must match: body text, h1, header crumb, large button (font, line-height, all four paddings, colors), widget panel (background, padding, font), slider (track, thumb), prev/next block (padding, direction). Screenshots are for a final sanity look, not for reading values.

## Do not

- Round corners (the slider thumb is the one circle), drop shadows, gradients, icon fonts, emoji as icons.
- Use the Ableton logo or name inside pages. The look is borrowed; the brand is not.
- Invent spacing outside the 15 / 30 / 40 / 60 / 80 scale, or accent hues outside the table.
