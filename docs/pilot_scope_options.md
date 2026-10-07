# Pilot scope, design options and task families (draft v0.2)

*Drafted by Claude, 2026-10-06, after Guglielmo's answers to Q1–Q13 (decisions D6–D18 in `CLAUDE.md`). This is a menu of options and a recommendation for Guglielmo to choose from; nothing here is agreed until it appears in the decision log. Everything Claude proposes here is at the level of **task families** (kinds of engineering work). Guglielmo authors the concrete tasks: scenario, inputs, reference solution and rubric.*

> **Suggestion before reading §5.** If you want some tasks to be provably yours rather than prompted by this menu, spend 15–20 minutes listing task ideas from your own experience first. They get the provenance tag "G" (see §4.6), which makes the circularity analysis in the paper stronger.

## 1. Constraints that shape the scope

| Constraint (decision) | Consequence for the pilot |
|---|---|
| Sole author, 10–15 h/week, no dates (D16) | ~20–30 tasks is the realistic ceiling; each task must be buildable in ~6–16 hours |
| Experts give short, directed, unpaid input (D9, D13) | No 4–8 h blind re-solves. Answer keys must be verifiable without a paid second engineer (§4.2, §4.4) |
| Minimal cost; frontier models via subscriptions preferred (D15; Q15 open) | Few models and runs; manual grading must stay affordable; keep a mix of tasks that need heavy local software and tasks that need only Python (§4.5) |
| Scientific quality and a solid repo are the core (D6) | Every task is fully documented (provenance, key type, validation record); the pilot's results are about the instrument's quality, not a ranking |
| Depth over breadth; suggested pillars: structures, aero/performance/design, propulsion (D10) | Three pillars in depth; other areas only as small probes |

**What a pilot of ~24 tasks can and cannot show.** It can measure the answer-key error rate, grader agreement, run-to-run variance and how authentic experts judge the tasks. It cannot rank two frontier models unless they differ by roughly 20 points or more on a pass/fail metric. The paper should be framed accordingly.

## 2. Pilot scope alternatives

| | **A. Structures only** | **B. Three pillars, fixed-wing** | **C. Three pillars + sector variety + integration** (recommended) |
|---|---|---|---|
| Content | Airframe and space structures, all modes | Structures; aero, performance and design; propulsion (gas turbine) — all fixed-wing | Same three pillars; ~25% of tasks on other vehicles (spacecraft structure, launcher propulsion, UAV); 3 integration tasks spanning pillars |
| Tasks | 12–15 | ~24 (8 per pillar) | ~24 (7 per pillar + 3 integration) |
| Shows | The method works in one discipline; fastest full loop | The method transfers across three disciplines with different key types (experiment, two-method, cycle model) | The above, plus transfer across vehicle types, plus the least-measured competence: integrating across disciplines |
| Cannot show | Anything beyond structures | Anything beyond fixed-wing | Depth in non-fixed-wing sectors (only probes) |
| Your effort (rough) | ~170 h ≈ 3–4 months | ~340 h ≈ 6–7 months | ~360 h ≈ 6–8 months |
| Fit with your expertise | Best (Cranfield structures) | Good | Good; launcher and spacecraft probes stay within structures and propulsion fundamentals |

**Recommendation: C, built in stages.** It costs about the same as B, but makes the paper's claim "aerospace, not just aircraft" credible and covers integration, which no existing benchmark tests.

1. **Stage 1, structures pathfinder (7 tasks):** run the whole loop end to end: author, expert review, model runs, grading, audit. Then fix the method before scaling. About 2–2.5 months, and it produces a first presentable result early.
2. **Stage 2, aero/performance/design and propulsion (14 tasks).**
3. **Stage 3, integration (3 tasks),** reusing the machinery from stages 1–2.

Option A is the fallback if Stage 1 shows tasks take much longer than estimated.

## 3. Full benchmark reach alternatives

Target map: **10 domains × 5 sectors.**

- **Domains:** (1) aerodynamics and CFD; (2) performance, sizing and design; (3) structures, materials and aeroelasticity; (4) propulsion (air-breathing, rocket, electric); (5) flight dynamics, GNC and control; (6) systems engineering, safety and certification; (7) avionics and flight/embedded software; (8) test and evaluation; (9) manufacturing and quality; (10) operations, maintenance and reliability.
- **Sectors:** fixed-wing, rotorcraft, UAV/eVTOL, launch vehicles, spacecraft.

| | **F1. Aircraft engineering** | **F2. Full aerospace** | **F3. Curated core + contributed modules** (recommended) |
|---|---|---|---|
| Coverage | All 10 domains; fixed-wing, rotorcraft, UAV | All 10 domains × 5 sectors | The F2 map, filled by a core you curate and author, plus modules contributed by other engineers under the published method, with you as editor |
| Size target | 120–150 tasks | 200–250 tasks | Core of ~80–120 items (including parametric variants) plus modules |
| Feasible for a sole author? | Only over several years | No | Yes: the methodology and repo are the product; contributions scale coverage without payment |

Why size matters: per-domain results need ~20 or more items per domain, and ±5-point comparisons between models need roughly 250 or more items. A sole author reaches this only through parametric variants (§4.1) and contributed modules.

**Suggested expansion order after the pilot.** It is ordered by open-tool maturity, value and your reach:

1. Flight dynamics and control (JSBSim, python-control).
2. Systems, safety and certification (free CS-23/CS-25 and FAA material).
3. Spacecraft and launch (GMAT, Orekit, Basilisk; builds on the pilot probes).
4. Test, operations and maintenance diagnosis (C-MAPSS, ESA-ADB).
5. Manufacturing and quality.
6. Rotorcraft (PoliMi's DUST and MBDyn).
7. Avionics and flight software.
8. The licensed track, later.

## 4. Design options

### 4.1 Unique tasks or parametric families

- **Unique tasks:** each task is hand-built. This gives maximum authenticity and the most effort.
- **Parametric families:** you author one anchor task and its reference pipeline. Then you derive 1–3 variants by changing parameters (geometry, loads, material, conditions), and your pipeline recomputes their references at 1–3 h each.
  - Pros: cheaper items, more statistical stability, and memorised textbook answers fail on the variants.
  - Cons: variants within a family are correlated, so statistics must cluster by family; and the variants must stay realistic.
- **Recommendation:** anchors for all families; add variants only to families whose reference pipeline is fully scripted. Analysis treats the family as the unit.

### 4.2 Answer-key types (the main defence against key errors)

| Key type | Meaning | Strength |
|---|---|---|
| **X** External data | Reference from experiment, flight test, workshop data or a validated public report | Strongest; independent of the author |
| **2M** Two methods | You compute the reference two independent ways (e.g. handbook method plus FE or CFD) and they agree within a stated band | Strong; catches most author errors |
| **BC** By construction | Seeded errors (Check tasks) or faults injected into a simulation (Diagnose tasks) | Strong for "what is the answer"; authenticity must come from real cases |
| **R** Rubric-dominant | The answer is mainly judgement (trade-offs, recommendations) | Weakest without paid reviewers; needs ≥2 expert reviews |

**Recommendation:** at least 70% of tasks keyed X, 2M or BC, and at most ~30% R-dominant. Publish each task's key type.

### 4.3 Task modes (this touches Q14, which is deferred; included so the menu is complete)

- **Produce:** do the analysis or design.
- **Diagnose:** find a cause from data.
- **Check:** independently check a colleague's analysis containing realistic errors; checking is a formal role in aerospace.

The recommended shortlist is about 19 Produce, 2 Diagnose and 3 Check. If you prefer to decide on Check tasks later with Q14, the shortlist still works without them.

### 4.4 Validation for a sole author with short expert reviews

This replaces the v0.1 gates (interview traceability and blind re-solves). Each task records which gates it passed.

| Gate | What happens | Your time | Expert time |
|---|---|---|---|
| V1 Traceability | The task cites its basis: your experience, a public incident or report, a standard practice reference, or expert input; plus a provenance tag (§4.6) | in authoring | — |
| V2 Key verification | X or 2M key; tolerances justified from experimental uncertainty, method scatter or measured solver spread | in authoring | — |
| V3 Expert review | Short structured review per task (often 2–3 tasks per conversation): realistic? key right? tolerance sensible? rubric's critical items right? expected time for a competent engineer? R-dominant tasks get ≥2 reviewers | ~1 h per consult | ~20–30 min per task |
| V4 Grader sanity | The reference passes; an empty answer fails; a plausible "common mistake" answer fails | ~1 h per task | — |
| V5 AI-assisted key audit | Model runs flag possible key errors; you decide (with an expert if needed); never changed automatically | in grading | occasional |
| V6 Grading reliability | You grade all pilot outputs; experts re-grade a small sample during consults; an LLM judge is used only if it agrees with you and the experts | ~15 min per output | a few outputs per consult |
| V7 Public errata | Public tasks on GitHub with an issue template for disputing a key; corrections versioned | ongoing | community |

What this gives up compared with the v0.1 plan: an independent full re-solve of every task. What it keeps: keys that don't depend on your judgement alone (X and 2M), an expert check of every task, measured grader agreement, and transparency. The paper should report exactly this.

### 4.5 Staying runnable whichever way Q14–Q15 go

Each family is tagged with the software it needs. Some tasks need only Python; others need XFOIL, AVL, OpenVSP, SU2, CalculiX or Code_Aster, pyCycle or NASA CEA, which an agent can only run in a local environment where these are installed. The shortlist keeps roughly a 60/40 split between the two kinds, so the pilot is meaningful under any later choice of how models are run.

### 4.6 Keeping Claude out of the answer keys

These safeguards apply the "no circularity" rule now that Claude is suggesting task families.

- **Provenance tag on every task:**
  - idea origin: G (you), E (expert), P (public incident or report), C (from this menu);
  - author of the concrete task: always G;
  - key source: X, 2M by G, or BC by G.
- **Claude never computes reference solutions,** never writes rubric content for benchmark tasks, and never decides tolerances. It may help with formatting, tooling and plumbing.
- **Report results by provenance.** If Claude scores relatively better on C-origin tasks than GPT does, that measures circularity; it's a small, novel analysis for the paper.
- **Contamination:** private tasks pass through Claude and ChatGPT whenever we work on them or run them. Check that both accounts have the "use my conversations to improve/train models" setting turned off.

## 5. Task-family menu

**Legend**

- ★ = in the recommended shortlist (§6).
- **Mode:** P = Produce, D = Diagnose, C = Check.
- **Software:** Py = Python only; tools are named.
- **Key:** X, 2M, BC, R (§4.2).

All families avoid military systems. Launcher items stay at textbook fidelity (export control).

### Pillar S — Airframe and space structures

| ID | Family | The work | Judgement tested | Sector | Mode | Software | Key and source |
|---|---|---|---|---|---|---|---|
| S1 ★ | Fitting/lug substantiation | Margins of safety for a lug or fitting under combined limit loads, all failure modes | Complete failure-mode set; ultimate factor and fitting factor (CS 25.303, 25.625); oblique-load resolution | Fixed-wing | P | Py (CalculiX optional) | 2M: classical lug method plus 3D FE; allowables from MIL-HDBK-5J (public) |
| S2 | Bolted splice load distribution | Fastener load share and bearing-bypass margins in a multi-row splice | Fastener-flexibility model choice; critical rows; bypass interaction | Fixed-wing | P | Py (CalculiX optional) | 2M: spring model plus FE |
| S3 ★ | Stiffened-panel stability | Skin buckling, stringer crippling and post-buckled strength of a wing or fuselage panel | Post-buckling philosophy; effective width; governing mode | Fixed-wing (launcher interstage variant) | P | Py + CalculiX or Code_Aster | X/2M: NACA panel test reports, handbook method plus FE |
| S4 ★ | Composite laminate sizing with real allowables | Size or check a laminate under combined loads using open-hole and impact-damage strain allowables and environmental knockdowns | Allowables-based design vs first-ply failure; hot/wet; damage-tolerance knockdowns | Any | P | Py | 2M: laminate theory plus public NCAMP allowables data (check terms) |
| S5 | Spectrum fatigue of a detail | Fatigue life of a metallic detail under a load spectrum | Kt vs Kf; mean-stress correction; scatter factor; limits of Miner's rule | Fixed-wing, rotorcraft | P | Py | 2M; public S-N data (MIL-HDBK-5J) |
| S6 ★ | Damage tolerance and inspection interval | Crack growth from detectable to critical size; inspection threshold and repeat interval | Geometry factor; NDT detectable size; residual strength; life factors (CS 25.571) | Fixed-wing | P | Py | 2M: two integration approaches; published crack-growth constants |
| S7 | Thin-shell buckling of a pressurised barrel or tank | Wall sizing under axial load, bending and internal pressure | Knockdown factor choice (NASA SP-8007 vs newer test-based factors); pressure stabilisation | Launcher, fixed-wing | P | Py (FE optional) | 2M + X: NASA SP-8007 (public), published shell tests |
| S8 ★ | Spacecraft bracket under launch environment | Check a bracket for quasi-static loads, minimum frequency and random vibration | Validity of Miles' equation; design vs qualification factors (NASA-STD-5001); stiffness requirement from the launcher manual | Spacecraft | P | Py + Code_Aster or CalculiX (modal) | 2M: Miles plus FE random response; public launcher user's manual |
| S9 ★ | Stress-note check | Independently check a stress note containing realistic errors | Verification; standards; ranking findings by margin impact | Any | C | Py | BC: seeded errors taken from real checker findings (your experience plus expert input) |
| S10 | Test–analysis correlation | Explain why measured static-test strains differ from FE predictions | Boundary conditions, load introduction, gauge placement; discriminating checks | Any | D | Py (+ FE) | X: needs a suitable public test dataset (to be found) |
| S11 ★ | Concession or repair substantiation | Disposition of a non-conformance (e.g. short edge distance), or a doubler repair | Margin recalculation; repair load path and stiffness; practical options | Fixed-wing (production/MRO) | P | Py | 2M + R |

### Pillar A — Aerodynamics, performance and design

| ID | Family | The work | Judgement tested | Sector | Mode | Software | Key and source |
|---|---|---|---|---|---|---|---|
| A1 ★ | CL<sub>max</sub> with fidelity choice | Estimate wing CL<sub>max</sub> with uncertainty for a design review | Validity limits of inviscid VLM vs section data vs empirical methods; Reynolds effects; uncertainty | Fixed-wing, UAV | P | XFOIL, AVL or OpenVSP/VSPAERO | X: NACA/NASA wind-tunnel reports on lesser-known wings |
| A2 ★ | Drag build-up | Zero-lift drag of a configuration by component build-up | Form factors, transition, interference and excrescence allowances | Fixed-wing, UAV | P | OpenVSP (parasite-drag tool) + Py | X: published flight or test drag data |
| A3 ★ | Take-off and one-engine-out climb | Take-off field length and weight–altitude–temperature limits for a twin | Regulatory speeds and gradients (CS 25.107–25.121); balanced field; hot and high | Fixed-wing | P | Py | 2M plus regulatory checks (CS-25 is free) |
| A4 | Payload–range and fuel policy | Payload–range diagram with a reserves policy | MTOW, MZFW and fuel-capacity corners; reserves; cruise technique | Fixed-wing | P | Py or Aviary | 2M: Breguet segments plus mission simulation |
| A5 ★ | Conceptual sizing to requirements | Constraint diagram, design point, first weight estimate | Choosing and justifying the design point; spotting conflicting requirements | Fixed-wing, UAV | P | Py, AeroSandbox or Aviary | 2M plus a constraint-satisfaction grader (many valid answers) + R |
| A6 | High-lift sizing | Flap selection for approach speed and landing field length | Empirical increments; weight vs complexity | Fixed-wing | P | Py (+ XFOIL) | 2M + R |
| A7 ★ | Static stability and tail sizing | Neutral point, static margin, tail size for forward-CG trim and rotation | Downwash; CG range; trim drag; cross-checking methods | Fixed-wing, UAV | P | AVL + Py | 2M: AVL vs USAF DATCOM methods (public) |
| A8 | Engine-out control (VMC) | Size the fin and rudder for minimum control speed | CS 25.149; bank angle; thrust asymmetry | Fixed-wing | P | AVL + Py | 2M plus regulatory |
| A9 ★ | CFD with numerical-uncertainty estimate | RANS solution of a validation case with a grid-convergence study | Mesh and y+; turbulence-model choice; Richardson extrapolation/GCI; knowing when CFD can't be trusted | Fixed-wing | P | SU2 + Gmsh | X: NASA Turbulence Modeling Resource cases (CC0), with conditions varied to defeat memorisation |
| A10 | Flight-test data reduction | Reduce climb or glide test points to standard conditions; derive the drag polar | Standardisation; instrument errors; scatter | Fixed-wing, UAV | P | Py | BC: synthetic data from a known JSBSim model with realistic noise |
| A11 ★ | Performance-report check | Independently check a performance substantiation containing realistic errors | CAS/TAS, ISA deviation, gradients, units | Fixed-wing | C | Py | BC: seeded errors from real practice |

### Pillar P — Propulsion

| ID | Family | The work | Judgement tested | Sector | Mode | Software | Key and source |
|---|---|---|---|---|---|---|---|
| P1 ★ | Turbofan design-point cycle | Cycle parameters to meet cruise thrust and SFC targets | Realistic component efficiencies and cooling flows; variable gas properties; sanity against existing engines | Fixed-wing | P | pyCycle (+ Cantera) | 2M: pyCycle plus an independent hand cycle; NASA reference models shipped with pyCycle |
| P2 | Off-design and installation | Thrust lapse and installed SFC over a mission | Bleed and power offtake; intake recovery; installation losses | Fixed-wing | P | pyCycle | 2M |
| P3 ★ | Gas-path diagnosis | Identify the degraded module from measured deltas | Fault signatures; measurement noise; smearing; recommending discriminating checks | Fixed-wing (operations) | D | pyCycle (fault injection) | BC: injected faults; C-MAPSS for realistic noise |
| P4 | Test-cell correction | Correct test-cell data to standard day; assess correlation | Corrected parameters; humidity; cell effects | Fixed-wing (test) | P | Py | 2M |
| P5 | Intake or nozzle selection | Intake sizing, or convergent vs convergent–divergent nozzle over an NPR range | 1D gas dynamics; off-design penalties | Fixed-wing, UAV | P | Py | 2M |
| P6 ★ | Electric propulsion matching | Motor, propeller and battery selection for an endurance and payload requirement | Thrust-to-weight for control authority; current and thermal limits; altitude and temperature effects | UAV/eVTOL | P | Py | 2M plus constraint grader (many valid answers); UIUC propeller data fetched at runtime |
| P7 ★ | Rocket engine performance trade | Mixture and expansion ratio for a stage: Isp vs propellant density; equilibrium vs frozen flow | Bulk-density trade; efficiency factors; altitude compensation | Launcher | P | NASA CEA + Py | 2M: CEA plus hand efficiency model. Textbook fidelity only |
| P8 ★ | Sea-level test of an altitude nozzle | Predict flow separation; recommend a test approach | Separation criteria and their uncertainty; side-load hazard; options | Launcher (test) | P | NASA CEA + Py | 2M + R |
| P9 ★ | Hot-fire data reduction and anomaly | Derive c\*, C<sub>F</sub> and Isp efficiencies from test traces; explain an anomaly | Data reduction; uncertainty; anomaly recognition | Small rocket (test) | D | Py | X if public data is found, otherwise BC (synthetic from a known model) |
| P10 | Solid motor ballistics (amateur scale) | Pressure–time prediction from grain geometry | Burn-rate law; erosive burning; safety margins | Amateur rocketry | P | openMotor (licence to verify) + Py | 2M. Amateur scale only |
| P11 ★ | Cycle-analysis check | Check a cycle analysis containing realistic errors | Constant-cp misuse; bleed accounting; station numbering | Fixed-wing | C | Py or pyCycle | BC: seeded errors |

### Integration (cross-pillar)

| ID | Family | The work | Judgement tested | Sector | Mode | Software | Key and source |
|---|---|---|---|---|---|---|---|
| I1 ★ | Re-engining impact | A lower-SFC but heavier engine on an existing aircraft: fuel burn, field length, balance, mount loads | The chain of effects across disciplines; second-order weight growth | Fixed-wing | P | Aviary/OpenMDAO + Py | 2M + R |
| I2 ★ | MTOW growth | +x% MTOW: loads → wing-root margins → structural weight → performance | Snowball effects; which requirement breaks first | Fixed-wing | P | Py | 2M + R |
| I3 ★ | Small UAV from requirements | Size the airframe, propulsion and main spar for a mission | Conflicting requirements; margins across disciplines | UAV | P | Py or AeroSandbox | Constraint-satisfaction grader + R |

## 6. Recommended shortlist for option C (24 families)

| Stage | Families | Count |
|---|---|---|
| 1 — Structures pathfinder | S1, S3, S4, S6, S8, S9, S11 | 7 |
| 2 — Aero, performance and design | A1, A2, A3, A5, A7, A9, A11 | 7 |
| 2 — Propulsion | P1, P3, P6, P7, P8, P9, P11 | 7 |
| 3 — Integration | I1, I2, I3 | 3 |

Profile of the shortlist:

- **Sectors:** 18 fixed-wing, 1 spacecraft, 3 launcher, 2 UAV.
- **Modes:** 19 Produce, 2 Diagnose, 3 Check.
- **Software:** ~13 Python-only (including pip-installable libraries such as AeroSandbox), ~11 needing named open tools.
- **Keys:** ~18 X, 2M or BC; ~6 with a large R component (S11, A5, P8, I1–I3).
- **Lifecycle:** mostly analysis and design. Test appears in P8 and P9, certification substantiation in S1, S6 and A3, production/MRO in S11, and operations in P3.

Swap freely: the non-starred families are the alternates. Stage 1 suggestion: start with the Python-only families (S1, S4, S6, S9) to exercise the method quickly, then add S3 and S8 to bring the FE tools in.

## 7. Effort assumptions (rough; Stage 1 will calibrate them)

| Item | Your time |
|---|---|
| Python-only task (scenario, inputs, two-method key, rubric, documentation) | 6–10 h |
| Tool-heavy task, once the tool environment exists | 10–16 h |
| Check task (writing a realistic flawed document) | 6–10 h |
| Parametric variant of an existing family | 1–3 h |
| Expert consult (preparation and notes) | ~1 h each |
| Runs and manual grading (2 models × 3 runs, ~15 min per output) | ~1.5–2 h per task |
| One-off tool environments and graders (implementation phase; Claude can write most of the plumbing under your review) | ~40–60 h in total |

## 8. Decisions needed from you

1. **Pilot option:** A, B or C, and whether to build it in stages.
2. **Full benchmark reach:** F1, F2 or F3, and the expansion order.
3. **Parametric variants:** yes (scripted families only) or unique tasks only.
4. **Key-type policy:** at least 70% X, 2M or BC.
5. **Validation gates:** V1–V7 as the validation protocol.
6. **Circularity safeguards:** provenance tags; Claude never computes keys; results reported by provenance.
7. **Task families:** accept the shortlist, or swap families. If you want some tasks to be your own (G-tagged), list your own ideas first.
8. **Task modes:** whether to include Check and Diagnose now, or leave them to Q14.

After these decisions: contacts research per selected family, then the simple consultation plan.
