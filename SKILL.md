---
name: learn-by-doing
description: Generate a single-file HTML practice page that lets the user learn a skill by doing it, with built-in automatic checking and feedback, instead of learning through an ongoing chat with the AI. Each page covers exactly one small concept; multiple pages are chained into a course by an outline. Use this skill whenever the user says things like "I want to learn X", "make me a practice page", "do the next page", "make something I can practice on", or any request to master a concrete skill or concept in a short time, even if they never say "page" or "artifact".
---

# Learn by Doing

Goal: the user says "I want to learn X" and gets back an HTML page they can reopen any time, practice on alone, and be graded by. The AI takes part once, at generation time. After that the page does the teaching.

One page = one small concept = 20–30 minutes. Multiple pages are chained by a course outline.

Why this shape: learning through chat needs the AI online, cannot be repeated, and never tells the learner whether they actually "got it". Baking the checks into the page means the learner can open it whenever, drill it as often as they like, and see for themselves which abilities passed and which did not. Extra effort at generation time buys every later practice session for free.

## Two ways this skill is triggered

**A. New topic**: "I want to learn X", where X is something fairly large
→ Build the course outline first (Step 0), split it into pages, then build only the first page.

**B. Continue**: "do the next page" / "do page 3 of X"
→ Read only `courses/<topic>/outline.md` and the handoff for the previous page. Do not read other pages' HTML.
Reading the HTML fills the context with nothing useful: everything the next page needs to know is in the handoff.

Before starting, find out one thing: the user's background (job, prior knowledge, tools they use). Examples should draw on the user's own world, so if you do not know, ask one question.

## Step 0: Course outline (new topics only)

Write `courses/<topic>/outline.md` in the format of `templates/outline.md`:

```markdown
# <Topic>
Running material: <one material used for the whole course, e.g. one audio clip, one economic scenario>
Ability tree: <all abilities in learning order, each starting with a verb, each verifiable>

## Pages
1. <Page name> — abilities: … — status: todo/done
2. …

## Handoff
<After each page, append 3–5 lines: what it taught, which terms it used, where the learner may still be shaky. The next page reads only this>
```

Splitting rule: each page holds 1–3 adjacent abilities and can be finished in 20–30 minutes.

Why one running material: switching material makes the learner re-orient to a new object on every page, spending attention on "what is this" instead of "how does what I am learning show up here". The same clip or scenario from start to finish lets later pages point straight back to earlier ones.

Show the outline to the user and get confirmation before building the first page.

## Step 1: Break this page into abilities

Take this page's abilities from the outline. Do not phrase them as knowledge; phrase them as "what the learner can do afterwards": start with a verb, "can judge…", "can dial in…", "can tell apart…".

Counter-example: "knows what threshold is" ❌ → "on hearing over-compression, can point out that the threshold is too low" ✅

"Knows" cannot be checked. "Can point out" can be written as a check the page's JavaScript runs. If this step is done badly, the pass conditions in Step 2 cannot be written.

## Step 2: Design the exercise list and get the user to confirm it (every page)

Each ability gets 1–3 exercises. Fill in the table below. First show the user the exercise list in a few sentences ("This page will have N exercises: 1… 2… Does that work?"). Only write the page after they confirm. Do this on every page, never skip it. Once generated the page is hundreds of lines of HTML, far more expensive to change than a table.

| Ability | Task type | What the user does | Pass condition | Hint 1 | Hint 2 | Hint 3 | Reveal |
|---|---|---|---|---|---|---|---|

### Task types

- **Sandbox + challenge**: a freely manipulable object plus one target state; reaching it passes automatically. Suits parameter-tuning and hands-on operations.
- **Predict then verify**: the user predicts the outcome first (choose / fill in / sketch), and only after committing sees the real result side by side. Suits concepts. Prefer this type and use it often: predicting first forces the learner to actually apply the principle; seeing the result and nodding is confirmation, not learning.
- **Discrimination**: several samples / arguments / scenarios; pick the correct one or point out the wrong one.

### Pass conditions

Must be something the page's JavaScript can execute on its own: numeric range, state check, option match, regex. A task whose pass condition cannot be written is either redesigned or dropped.

### Difficulty

Step up steadily. Between two adjacent tasks add exactly one new variable. No sudden jumps mid-page, no random interleaving. Interleaving and mixing belong only in the capstone task.

### Hint ladder (three increasing levels)

- Hint 1: point the direction — "look / listen here", without saying what to change
- Hint 2: narrow the range — "the problem is somewhere between A and B"
- Hint 3: almost the answer — one step short
- Reveal: give the answer plus one sentence pointing back to the principle taught earlier (do not re-teach; the principle was already given)

### Two mandatory closers on every page

- **Capstone task**: a realistic scenario that uses every ability on the page, with no hints.
- **Explain-it-back**: after the capstone there must be one prompt, "explain this to someone who has never learned it". The user writes in a text box. The page does not grade it; it stores the text, then shows a reference answer for the user to compare against on their own.

## Step 3: Page structure (fixed)

```
[Baseline]          Straight into the sandbox for 1–2 tries, or 1–2 quick questions.
                    Abilities already mastered are marked skipped and not re-taught
[Principle]         Explain this page's principle fully, but always tied to the running
                    material, never in the abstract. Length is fine, but split into
                    screens, one point per screen
[Predict → verify]  Immediately after the principle: use it to predict what happens on
                    the running material, then verify
[Task 1..n]         Each task: demo → user acts → check
                    wrong: hint 1 → hint 2 → hint 3 → reveal
                    right: next task
[Capstone]          No hints; passing it is what "learned" means
[Explain-it-back]   Explain this page to someone else. Stored, reference answer shown
                    for self-comparison, not graded
[Review panel]      Shows the ability tree with this page's abilities marked:
                    untested / passed unaided / passed via reveal / failed
                    Also shows where this page sits in the course, with links to the
                    previous and next page
```

The principle comes before the tasks, not after a mistake. After a mistake the page never re-explains the principle; it only gives hints and points back.

`templates/page-skeleton.html` already implements screen navigation, the task state machine, the three-level hint ladder, progress storage, the review panel and the visual theme. Start from it instead of writing from scratch: these mechanics are identical on every page, and rewriting them only introduces inconsistency.

## Step 4: Hard technical rules

- Page language: English. Use the standard English terms of the field.
- Single HTML file, styles and scripts inline. External libraries only from cdnjs, and avoid them when possible.
- Persist with `window.storage`: the state of every ability, the wrong answers, and the last position. On reopening, serve 2 questions on the weakest abilities before any new content. Fall back to `localStorage` when `window.storage` does not exist.
- Key prefix `course:<topic>:page:<n>`, so the review panel can read progress across pages.
- One point per screen. No explanatory text that needs scrolling to finish.
- Every check, hint and reveal is hard-coded in the page. No API calls of any kind.
- Works on mobile: touch targets at least 44px, no horizontal overflow.
- Examples drawn from the user's own background whenever possible.
- Visual theme: the Learning Music look defined in `references/theme.md` (dark grey ground, grey panels, white text, Futura-style type with Jost embedded as the fallback, no borders or radii, goldfish yellow for the active state, flat color blocks for feedback). The skeleton ships with it; keep its tokens when adding sandbox UI, and read the reference before writing any CSS of your own.

Page location: `courses/<topic>/page-<n>.html`.

## Step 5: Self-check after generation

- [ ] Every ability has at least one task testing it
- [ ] Every task's check runs in the browser alone, with no AI involved
- [ ] The three hints really escalate, and hint 1 does not give the answer
- [ ] Three wrong attempts always lead to the reveal; nothing can dead-end
- [ ] Adjacent tasks differ by exactly one new variable
- [ ] The capstone actually uses every ability on the page
- [ ] The explain-it-back prompt exists and has a reference answer
- [ ] The exercise list was confirmed by the user
- [ ] The whole page uses the same running material
- [ ] Progress survives a refresh; no horizontal overflow at phone width
- [ ] This page's handoff was appended to outline.md and the page status set to "done"

If a check fails, fix it. Never hand over a page that fails the self-check.

## Domain files (domains/*.md)

Before building a page in a given field, look in `domains/` for a matching file and read it if present. If none exists, write one in the format below before Step 2; every later page in the same field reuses it.

Each domain file answers only four questions and does not repeat the general rules in this file:

1. What the "sandbox" is in this field (the object being manipulated, and the technology that implements it)
2. Which task types fit this field best, with one example each
3. Which pass-condition mechanisms are available (numeric closeness, listening choice, direction right/wrong, drag-and-drop matching…)
4. The common "illusions of having learned it", and how task design avoids them

Existing domain files:

- `domains/audio-mixing.md` — mixing / dynamics (compression, EQ), Web Audio sandbox
- `domains/economics.md` — introductory economics (supply, demand, elasticity), interactive chart sandbox
