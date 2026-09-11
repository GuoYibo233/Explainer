# Visual theme: Learning Music

Every generated page uses the look of learningmusic.ableton.com, Ableton's own learn-by-doing site. It was chosen because it is built for exactly this kind of page: one idea per screen, an interactive widget in the middle, calm dark ground that keeps attention on the bright interactive cells. The skeleton already implements it; this file exists so a page whose CSS is edited or extended stays on-theme. Values below were read from the site's `css/main.css`.

## Type

- The site uses **Futura PT** (400 and 700), self-hosted. It is a licensed Adobe font and cannot be embedded in generated pages.
- The skeleton therefore stacks `"Futura PT", Futura, Jost, "Helvetica Neue", Helvetica, Arial, sans-serif` and embeds **Jost** (OFL-1.1, a geometric sans cut on the Futura model) as a base64 woff2 variable font, about 35KB. A machine with Futura PT or Futura installed shows the real thing; every other machine shows Jost, never a humanist fallback.
- Root size 20px, line-height 1.5, antialiased. Body text `.8rem` (16px). `h1` and `h2` `1.6rem` (32px) at line-height 1.2, bold. Small labels `.7rem` (14px).
- Line length is capped: paragraphs and headings `max-width: 24rem` (about 60 characters), left-aligned, the page column `max-width: 40rem`.
- Bold is used for headings, buttons, widget labels and the header crumb. Nothing else.

## Color

Greys carry the page; accents carry state. All accent blocks take black text.

| Token | Hex | Site usage | Skeleton usage |
|---|---|---|---|
| base | `#666666` | page ground (`.bg--base`) | ground |
| gray-30 | `#4D4D4D` | buttons, widget controls, sidebar | panels, sandbox, secondary buttons, callouts |
| gray-45 | `#757575` | | |
| gray-50 | `#858585` | column labels, lane highlight | labels, untested state bar |
| gray-70 | `#B3B3B3` | beat labels | muted text |
| gray-90 | `#D9D9D9` | | |
| white | `#FFFFFF` | text, transport button, slider thumb | text, primary button |
| goldfish | `#FED134` | active sequencer cells | progress bar, chosen option, hints, "revealed" state |
| tiffany | `#00D2BE` | accent | pass state |
| salmon | `#FF8389` | accent, top banner | wrong / failed state |
| antarctica | `#6DCBFF` | accent | info |
| lavender | `#D5B3FF` | accent | reference answer block |
| razzmatazz | `#FF2956` | accent | reserved |

## Components (as the site builds them)

- **Buttons** (`.btn`): inline-flex, bold, no border, no radius, padding `.35em 1.5em`; default is gray-30 with white text; large variant `.8rem` with padding `1.5em 2em`; `--wide` is full width; `--link` is transparent with white text. The skeleton's primary button is white on the dark ground, mirroring the site's white transport buttons.
- **Prev / next** (`.prev-next`): a column, `padding-top: 3rem`, with **Next first** (`order: -1`), full width and grey, and Previous below it as a link-style button.
- **Widget panel** (`.widget__controls`): gray-30 block, padding `.75rem`, bold labels at `.7rem`. Sliders (`.range`): 4px track in the ground grey, 20px round white thumb.
- **Header**: `.8rem` bold crumb in the form `1/10: Beats`, `padding-top: 1.5rem`.
- **Grid cells** (for sequencer-style sandboxes): gray-50 `#858585` idle, goldfish active, 1px ground-colored gaps, beat lines gray-30.

## Light variant

`<html data-theme="light">` switches to the ableton.com look: black text on `#F3F3F3`, white panels, black secondary buttons, `#0000FF` primary button, acid-yellow `#E1FF8A` callouts. Use only if the sandbox needs a light ground (for example a printed-style diagram); the default is dark.

## Do not

- Round corners (the thumb is the one circle), drop shadows, gradients, icon fonts, emoji as icons.
- Use the Ableton logo or name inside pages. The look is borrowed; the brand is not.
- Add accent hues beyond the table. Grey does the layout work; accents mean something.
