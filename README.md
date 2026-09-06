# SQ-24 dual analog sequencer — signal flow

## Overall architecture

```mermaid
flowchart TD
    CLK[Master clock]

    subgraph SEQ1["Sequencer 1 — primary CV"]
        S1A["Row A<br/>Pitch → VCO"]
        S1B["Row B<br/>Filter → VCF"]
        S1C["Row C<br/>Mod → filter or VCA"]
    end

    subgraph SEQ2["Sequencer 2 — modular assign"]
        S2A["Row A<br/>Assignable dest"]
        S2B["Row B<br/>Assignable dest"]
        S2C["Row C<br/>Assignable dest"]
    end

    subgraph VOICE["Voice engine (shared)"]
        VCO[VCO]
        VCF[VCF]
        VCA[VCA]
        VCO --> VCF --> VCA
    end

    ADSR[ADSR envelope]
    OUT[Audio output]

    CLK --> SEQ1
    CLK --> SEQ2
    SEQ2 -. sums into .-> SEQ1
    SEQ1 --> VOICE
    SEQ2 --> VOICE
    ADSR -.shapes.-> VCA
    VOICE --> OUT
```

Seq 1's three rows feed pitch, filter, and the row-C mod destination
directly. Seq 2's rows can each be routed (via the per-row dropdown) into
any of those same targets — plus decay time or glide time — so its values
**sum with** Seq 1's before hitting the shared VCO → VCF → VCA chain. The
ADSR envelope shapes the amplitude (and filter) stage on every step.

## Sequencer 2 trigger routing

Seq 2's patch-point row isn't audio — it's a routing switch. Only one role
is active at a time, chosen by the `TRIG ASSIGN` dropdown, and the lit
patch-point color matches the active role (red / yellow / blue / green).

```mermaid
flowchart TD
    TRIG["Seq 2 trig out<br/>4 patch-point roles"]

    RESET["Reset<br/>Restarts seq 2 loop<br/>(red)"]
    ACCENT["Accent<br/>Boosts VCF + VCA<br/>(yellow)"]
    GLIDE["Glide<br/>+150ms portamento<br/>(blue)"]
    CLOCK1["Clock seq 1<br/>Advances seq 1's step<br/>(green)"]

    TRIG --> RESET
    TRIG --> ACCENT
    TRIG --> GLIDE
    TRIG --> CLOCK1
```

---
*Schematics for the SQ-24 Dual Analog Sequencer by Jose Velazquez MA — [Voltage & Wave](https://voltageandwave.co.uk/)*
