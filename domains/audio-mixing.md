# Domain: mixing / dynamics (compression, EQ, gain staging)

## 1. What the sandbox is

Object: one 5–10 second running clip (a drum loop or a short vocal phrase), the same clip for the whole course.
Implementation: Web Audio API, no external libraries.

- Source: prefer a beat / pulse sequence synthesized from `OscillatorNode` + `GainNode`, or an `AudioBufferSourceNode` playing a short WAV inlined as base64 (keep it under 200KB to respect the single-file rule). A synthesized source always runs and depends on no file.
- Compressor: `DynamicsCompressorNode`, exposing threshold / ratio / attack / release / knee as needed.
- EQ: `BiquadFilterNode` (peaking / lowshelf / highshelf).
- Visualization: `AnalyserNode` drawing a waveform or level meter onto a `<canvas>`, so the learner can *see* what the compressor is doing.
- A/B switch: one button toggling dry / wet, the basis of every listening task.

Browsers only start an `AudioContext` after a user gesture, so the first screen needs a "Start audio" button.

## 2. Best-fitting task types

- **Sandbox + challenge**: give a target state, e.g. "make the gain-reduction meter read −3 to −6 dB on the loudest hits and barely move elsewhere". The learner adjusts threshold / ratio; the page reads `compressor.reduction` to check.
- **Predict then verify**: ask "if attack goes from 1 ms to 50 ms, do the drum transients get more prominent or flatter?"; the A/B switch unlocks only after the learner commits.
- **Discrimination (listening)**: three versions A / B / C of the same clip with different settings; "which one has a release so long it pumps?". The check is an option match.

## 3. Pass-condition mechanisms

- **Numeric closeness**: read `DynamicsCompressorNode.reduction`, or compute RMS / peak from `AnalyserNode`, and compare against a target range.
- **Parameter range**: check that the learner's threshold / ratio falls inside an interval. This is the weakest check; use it only for pure operation tasks.
- **Listening choice**: A/B/C option match.
- **Direction right/wrong**: prediction tasks only judge direction: "louder / quieter / same", "more prominent / flatter".

## 4. Illusions of having learned it, and how to avoid them

- **Illusion 1: memorized numbers.** The learner remembers "threshold −18, ratio 4:1 is right" and is lost on a quieter clip. → The capstone uses the same clip with input gain pulled down 6 dB and checks whether the learner moves the threshold accordingly.
- **Illusion 2: watching the meter, not listening.** The learner solves tasks by staring at the gain-reduction meter and the ears never engage. → At least one listening task hides the meter and gives only the ears.
- **Illusion 3: can adjust, cannot diagnose.** Can follow hints to a target, but cannot say what is wrong with a clip that is already broken. → The capstone hands over a preset that is over-compressed with too long a release and requires the learner to name the problem before fixing it.
- **Illusion 4: knows the term, not the phenomenon.** Can say "pumping" but cannot hear it. → Terms are demonstrated once on the running material in the principle screens; every later task uses only descriptions of the phenomenon or the audio itself, never the term as a hint.
