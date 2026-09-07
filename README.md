# SQ-24 dual analog sequencer — signal flow (WORK IN PROGRESS)

The SQ-24 is a browser-based, two-sequencer synth. Each sequencer has 12
steps (extendable to 24), and the whole thing can run as **one voice** with
Sequencer 2 modulating Sequencer 1 (SUM mode), or as **two independent
voices** playing together (DUO mode).

No installation needed — open the HTML file in a browser and click PLAY.
Audio starts on your first click anywhere on the panel (this is a browser
requirement, not a bug).

---

## 1. Quick start

1. Click **PLAY**. The step LEDs above each sequencer start scanning.
2. Turn a **knob** on the grid (click and drag up/down) to set that step's
   pitch, filter, or mod value. Changes apply live, even while playing.
3. Click **DEMO: DUOPHONIC** at any point to load a ready-made pattern and
   hear the synth in action.
4. Click a **patch point** (the gold dot under each step) to set where a
   sequencer loops back to, or to add a trigger.

---

## 2. Master control row

| Control | What it does |
|---|---|
| **PLAY** | Starts/stops both sequencers together. The dot on the button pulses while playing. |
| **◀ / ▶ step buttons** | Manually step the sequencers backward/forward one step at a time, without pressing Play. Useful for programming while silent. |
| **CLOCK SPEED knob** | Sets tempo (roughly 40–240 BPM). |
| **GLIDE TIME knob** | Base portamento (pitch slide) time between notes, in seconds. |
| **PLAYBACK MODE** | **STEP** — sequencer only advances when you press the step buttons. **CONT** — loops continuously (default). **ONCE** — plays through once, then stops automatically at the loop point. |
| **CV RANGE** (±5V / ±1V toggle) | Scales how far the pitch/filter knobs swing. ±1V gives smaller, subtler pitch and filter movements; ±5V is the full range. |
| **VOICE MODE** (SUM / DUO toggle) | The core mode switch — see Section 5. |

---

## 3. Sequencer 1 — always the primary voice

Sequencer 1 always plays through **Voice 1** (its own VCO/VCF/VCA/envelope,
in Expander Module 1). It has three rows of step knobs:

- **Row A — Pitch.** Each knob sets that step's pitch offset in volts
  (roughly ±5 semitones per volt at full CV range).
- **Row B — Filter.** Each knob sets that step's filter cutoff offset.
- **Row C — Mod.** A small dropdown next to Row C picks whether this row's
  values modulate the **filter** (VCF MOD) or the **volume** (VCA MOD) —
  handy for adding accents or timbral movement without touching Row B.

**TRIG OUT row (below the knobs):** click a patch point to mark that step
as the sequencer's loop-back point. Click the same point again to clear it.
If Sequencer 1's length is set to 24 (see below), clicking a patch point
cycles through: off → loop at this step (1–12) → loop at this step + 12
(13–24, shown in red) → off again.

---

## 4. Expander Module 1 (voice controls for Sequencer 1)

Docked to the right of Sequencer 1's grid:

- **SEQ toggle (12/24):** flips Sequencer 1 between a 12-step and a 24-step
  pattern. In 24-step mode, Row A becomes two chained 12-step halves
  (steps 1–12 then 13–24) and Row B is not used for that pass.
- **VCO 1:**
  - **Wave buttons** — sawtooth or square wave for Voice 1's oscillator.
  - **Octave buttons (32′/16′/8′/4′)** — coarse octave transposition, in
    the pipe-organ notation borrowed from classic synths (32′ = lowest,
    4′ = highest).
  - **FREQ knob** — fine pitch tuning, roughly ±1 octave.
- **ENV 1 (ATTACK / DECAY / SUSTAIN / RELEASE):** the standard ADSR
  envelope shaping Voice 1's filter and amplitude on every step.

---

## 5. Voice Mode: SUM vs. DUO

This toggle changes what Sequencer 2 *is*.

### SUM mode (default)
Sequencer 2 doesn't produce its own sound. Instead, each of its three rows
is **added into** one of Voice 1's parameters, chosen from a dropdown next
to each row:

- `+ PITCH`, `+ FILTER`, `+ MOD` — adds to the matching Sequencer 1 value
- `DECAY TIME` — stretches or shortens Voice 1's decay stage per step
- `GLIDE TIME` — adds extra portamento on that step

This is how you layer a second rhythmic/melodic pattern into one voice —
e.g. Sequencer 1 plays a bassline while Sequencer 2's Row A adds pitch
accents on top.

### DUO mode
Sequencer 2 becomes a fully independent second voice, using **Expander
Module 2** (same layout as Module 1: SEQ length, VCO 2, ENV 2). Its rows
are now fixed and no longer routable:

- Row A → Voice 2 pitch
- Row B → Voice 2 filter
- Row C → Voice 2's own decay time

Both sequencers still share the same CLOCK SPEED and GLIDE TIME masters,
but each voice has its own waveform, octave, tuning, and envelope — so you
can play, say, a sawtooth bass on Voice 1 against a square-wave lead on
Voice 2.

Switching modes doesn't erase your step values — it just changes how
they're used, so it's safe to flip back and forth while programming.

---

## 6. Sequencer 2 & its trigger row

Sequencer 2's knob grid works the same as Sequencer 1's (click-drag to set
values, SEQ 12/24 toggle, same patch-point double-length behavior for the
resulting pattern).

**TRIG ASSIGN dropdown** (above the grid) sets what Sequencer 2's patch
points *do* — only one role is active at a time, and the lit patch-point
color tells you which:

| Assignment | Patch color | Effect |
|---|---|---|
| LOOP LENGTH | Red | Restarts Sequencer 2's own loop at that step |
| ACCENT | Yellow | Boosts filter cutoff and volume on that step (works in both SUM and DUO mode) |
| GLIDE | Blue | Adds ~150ms extra portamento on that step |
| CLOCK SEQ1 | Green | Advances Sequencer 1's step — only takes effect if Sequencer 1's `CLOCK` dropdown is set to `EXT (FROM S2)` |

---

## 7. Clock & rate menus

Each sequencer has its own small menu pair, top-right of its grid:

- **CLOCK:** `INTERNAL` (runs off the master clock) or `EXT` (waits for
  triggers from the *other* sequencer instead — only meaningful when the
  other sequencer's TRIG ASSIGN is set to `CLOCK SEQ1`, or symmetrically
  for Sequencer 1 driving Sequencer 2).
- **RATE:** step duration — 1/16, 3/16 (dotted-feel), or 1/8 notes,
  relative to CLOCK SPEED.

Setting Sequencer 1 to `EXT` and Sequencer 2's trigger to `CLOCK SEQ1`
lets Sequencer 2 "drive" Sequencer 1 irregularly — useful for polyrhythmic
or generative patterns.

---

## 8. Keyboard row

The small on-screen keyboard at the bottom doesn't play notes on its own —
it **transposes** everything. Press and hold a key to preview that
transposition; the active key stays highlighted after release, and the
transposition applies to all subsequent playback until you pick a new key.

---

## 9. DEMO button

Click **DEMO: DUOPHONIC** to instantly load a preset two-voice pattern
(switches to DUO mode, sets both voices' waveforms/envelopes, fills in
step values, and starts playback). Good as a starting point to explore or
to reset the panel to a known state.

---

## 10. Quick troubleshooting

- **No sound at all:** click anywhere on the panel first — browsers block
  audio until a user interaction occurs.
- **Sequencer 2 changes don't seem to do anything:** check VOICE MODE. In
  DUO mode, the per-row routing dropdowns are hidden (they're fixed to
  pitch/filter/decay); in SUM mode, Expander Module 2's knobs are dimmed
  and inactive.
- **Pattern won't loop where expected:** check the TRIG OUT patch points —
  an unset loop point means the sequencer just runs to the end of its
  current length (12 or 24) before wrapping.

---

*SQ-24 Dual Analog Sequencer by Jose Velazquez MA — [Voltage & Wave](https://voltageandwave.co.uk/)*
