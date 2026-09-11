# Domain: introductory economics (supply, demand, elasticity, equilibrium)

## 1. What the sandbox is

Object: one concrete market scenario that runs through the whole course, e.g. "the bike-rental market in a college town". Every page adds variables to this one market and never switches scenario.
Implementation: plain SVG or `<canvas>` drawing the supply/demand diagram, no charting library.

- Curves: supply and demand as parameterized lines (slope, intercept); drag endpoints or move sliders to change parameters.
- Equilibrium: JavaScript computes the intersection live and marks P* / Q* on the diagram.
- Event buttons: scenario events like "a $2 tax per rental" or "student population doubles" that shift a curve.
- Numbers panel: shows current P, Q, consumer surplus, producer surplus (exposed as needed).

## 2. Best-fitting task types

- **Predict then verify** (the workhorse): ask "fuel prices rise; which way does the demand curve for bike rentals shift?"; the learner picks a direction on the diagram (or a left / right button); after committing, an animation plays the real shift and marks the new equilibrium.
- **Sandbox + challenge**: target state "get P* under $5 while keeping Q* at or above 200", and the learner may only drag the supply curve. The check reads the computed equilibrium.
- **Discrimination**: three news-style sentences ("rising rents push more shop owners into the market", "fewer students ride in the rainy season", …); which one shifts supply, which shifts demand, which is a movement along a curve rather than a shift.

## 3. Pass-condition mechanisms

- **Direction right/wrong**: curve shifts left / right / stays; price up / down; quantity up / down.
- **Numeric closeness**: the dragged equilibrium P* / Q* falls inside a target range.
- **Option match**: discrimination answers.
- **Drag-and-drop matching**: drag scenario cards into three buckets, "shifts supply" / "shifts demand" / "movement along", and check each card's bucket.

## 4. Illusions of having learned it, and how to avoid them

- **Illusion 1: memorized "the curve moves right".** The learner recalls conclusions without deriving them. → Prediction tasks require two steps: first "does this affect buyers or sellers?", then the direction. Both must be right to pass.
- **Illusion 2: shift vs. movement along the curve.** The classic confusion in this field. → Every page includes at least one task that plants "the price itself changed" inside a scenario as a distractor.
- **Illusion 3: single-variable only.** One event at a time is fine; two at once falls apart. → The capstone gives two simultaneous events and asks the learner to judge that P's direction is determinate while Q's is not (or the reverse).
- **Illusion 4: can compute, cannot translate.** Can calculate elasticity from a formula but cannot read a news item and say whether elasticity is high or low. → Discrimination tasks use real scenario descriptions rather than numbers.
