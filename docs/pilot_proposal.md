# AerospaceBench pilot — proposal (draft v0.1)

> **Partly superseded (2026-10-06).** Guglielmo's answers (decisions D6–D18 in `CLAUDE.md`) replace the interview-based elicitation (§6), the blind re-solve gate (§5.3), the 12–20 engineer sizing and the effort figures (§7). Guglielmo authors the tasks, and experts give short, directed reviews. Scope and validation are reworked in [`pilot_scope_options.md`](pilot_scope_options.md). The construct (§2), task package (§3.1), grading stack (§3.5) and risks remain useful references.

*Drafted by Claude, 2026-10-06, after the landscape research in [`reports/Aerospace engineering LLM benchmarks.md`](../reports/Aerospace%20engineering%20LLM%20benchmarks.md). Nothing in this document is agreed until it appears in the decision log in `CLAUDE.md`. Questions blocking the next step are in [`open_questions.md`](open_questions.md).*

## 1. What the research tells us

1. **Nobody measures aerospace engineering work yet.** About two dozen aerospace LLM evaluations exist, almost all from 2025–2026. Most are multiple-choice recall sets on aviation operations, maintenance and regulations, where frontier models score 60–83%. Design, analysis, certification, test, rotorcraft, aerostructures, spacecraft budgets and flight software have essentially no LLM evaluation.
2. **Difficulty lives in execution and verification, not knowledge.** Textbook calculation is near saturation (ThermoQA 94%), while simulation-verified design stays near one-third (EngDesign 34%, CADWorld 17.5% vs 87% for experts). Agents driving CFD score about 50 points lower when the result must be *right* rather than merely *run*.
3. **Failures are silent and engineering-typical:** wrong boundary conditions, units, sign conventions, regulatory factors, cascading errors, and not checking results. A final-answer grader misses most of these; an engineer reviewing the work catches them.
4. **Answer keys and graders limit benchmark quality more than task authors' credentials do.** Audits forced FrontierMath to change 42% of its problems; grader defects in SciCode held frontier scores at 9–27% until fixing them raised scores to 69–92%. Harness choice alone has moved agent scores by more than 30 points.
5. **An open-source-only toolchain is feasible** for most disciplines at conceptual-to-preliminary fidelity. Four holes remain: rotorcraft comprehensive analysis, certification-grade damage tolerance, Nastran-class flutter, and paywalled process standards (DO-178C, ARP4754B/4761A).

Consequence: the pilot's job is to **validate the instrument** (authentic tasks, correct keys, trusted grading, known variance), not to produce a leaderboard.

## 2. Proposed construct

> **AerospaceBench measures whether an LLM or LLM agent produces aerospace engineering work that a competent practising engineer would accept and sign off on, for realistic tasks across the lifecycle and vehicle types, including how it chooses models, handles uncertainty and incomplete information, balances constraints, integrates disciplines and verifies its own results.**

Each task is tagged with the **engineering facets** it exercises, taken from your brief, so we can report results by facet as well as by sector:

| Facet | What a competent engineer does |
|---|---|
| F1 Model and fidelity choice | Picks a method fit for purpose and states its validity limits |
| F2 Assumptions and incomplete information | Fills the natural gaps in a real work package with stated, defensible assumptions, or flags what must be asked |
| F3 Analysis correctness | Gets the numbers right (units, conventions, factors) |
| F4 Verification | Checks convergence, balances, limit cases and hand-calc sanity before trusting a result |
| F5 Uncertainty and margins | Quantifies uncertainty and carries margins and safety factors correctly |
| F6 Constraints and trade-offs | Balances competing requirements and justifies the choice |
| F7 Integration | Accounts for cross-discipline effects (e.g. weight → CG → tail → trim drag) |
| F8 Standards and compliance | Grounds claims in the applicable regulation or standard, without inventing clauses |
| F9 Diagnosis | Infers causes from symptoms and data, and proposes discriminating checks |
| F10 Judgement to stop or escalate | Recognises infeasible, unsafe or out-of-scope requests and says so |

The secondary goal (aerospace as a proxy for general engineering ability) is served by also tagging each task with its underlying discipline (fluids, solids, thermodynamics, controls, electrical, software, systems), so results can later be compared with other engineering benchmarks.

## 3. Shape of the benchmark

### 3.1 The task package

Each task looks like a real work package, not an exam question:

- **Brief:** written as an engineer would receive it (work request, ticket, email from a lead), with the inputs a real package would contain: drawings, data files, requirement extracts, prior reports, catalogues.
- **Reference material:** freely redistributable sources only (CS-25/CS-23 and AMC, 14 CFR, FAA ACs, NASA public standards and reports, MIL-HDBK-5J allowables, NACA reports). No task may require paywalled clause text.
- **Environment:** a pinned container with Python and the open tools the task needs; no internet.
- **Deliverable:** what the engineer would produce, typically a short engineering memo or calculation note, the working files (scripts, input decks), and a results summary table (key quantities with units and uncertainty). Engineers routinely produce summary tables of margins and results, so this is not an artificial format, and it lets graders check numbers deterministically.
- **Hidden material:** reference solution(s), acceptable-answer sets or tolerance bands with their justification, the rubric, grader scripts.
- **Metadata:** pseudonymous source-engineer ID, stratum, facets, discipline, estimated expert time, export-screening status, contamination notes, and a record of any LLM involvement in preparing the task.

### 3.2 Coverage frame

Two axes drawn from your brief, **lifecycle stage** (design, analysis, test, certification and safety, manufacturing and quality, operations and maintenance) and **sector** (fixed-wing, rotorcraft, UAV, launch vehicle, spacecraft, with propulsion, systems and software cutting across), plus the facet tags above. Sampling is anchored to O*NET's Aerospace Engineers task list (17-2011.00) and its technician and maintenance neighbours, so we can show coverage was sampled from real work rather than chosen for convenience.

### 3.3 Task modes

| Mode | Description | Why include it |
|---|---|---|
| **Produce** | Do the analysis or design and deliver the work product | The core of engineering work |
| **Diagnose** | Find the cause of an anomaly from data or symptoms; ground truth comes from real records or faults injected into a simulation following real incidents | Diagnosis is central to test, operations and maintenance, and injected faults give unambiguous keys |
| **Check** | Independently check a colleague's analysis or report that contains realistic errors drawn from interviewees' real experience | Independent checking is a formal role in aerospace (stress checking, compliance verification); keys are unambiguous; directly tests F4 and F8 |

**Check** tasks are only authentic if the errors are the kind interviewees have actually seen. Errors invented to trip models would be the artificial difficulty you want to avoid.

### 3.4 Environment and harness (decided later, flagged now)

- One sandboxed environment for every task: an engineer always has a computer, so "with tools" is the realistic default. A no-tools ablation can be run on the subset of tasks where it makes sense.
- Run every model through one **common open harness** (UK AISI's Inspect is the leading candidate: open source, Docker sandboxing, already used by the Pre-Flight aviation benchmark). Vendor agents (Claude Code, Codex CLI, Gemini CLI) can be run as a secondary condition. Report each model–harness pair separately, with reasoning effort, limits and documentation access disclosed.
- Compute budget per task capped so the benchmark is rerunnable on a workstation (e.g. 2D or coarse 3D CFD that runs in under about 30 minutes on 8 cores).

### 3.5 Grading stack and metrics

Every layer is reported separately, so a good average never hides a safety-critical error:

| Layer | Checks | How |
|---|---|---|
| G0 Artefact | Deliverable exists and parses | Script |
| G1 Re-execution | Grader re-runs the submitted scripts and decks in a clean container; numbers the agent reports but cannot reproduce are ignored | Script |
| G2 Physics sanity | Convergence, balances, units, refinement | Script |
| G3 Quantities of interest | Within tolerance bands set from experiment, workshop scatter or verified reference, wider than measured solver run-to-run spread | Script |
| G4 Engineering judgement | Typed atomic rubric criteria (critical / major / minor; negative criteria for unsafe or invented content) covering facets F1–F10 | Expert grading, then an LLM judge once it has been validated against the experts |
| G5 Sign-off | "Would you sign this off as-is / with minor changes / not at all?" | Expert, on a sample |

Proposed metrics, to be fixed before the pilot runs:

- **Acceptable-work rate:** share of runs where every critical criterion passes and the G3 quantities are within tolerance. This is the headline engineers will care about.
- **Mean rubric score:** a continuous score with more statistical power.
- **Reliability:** pass^k over repeated runs (at least 3 per model–task pair).

Two grading ideas to test in the pilot:

- **Assumption-conditional grading:** where several assumptions are legitimate, the grader re-runs the reference pipeline with the agent's *declared* assumptions and checks consistency, rather than penalising a valid but different choice.
- **Interval scoring:** for estimates requested with uncertainty, score both whether the band contains the reference and whether it is sensibly tight.

## 4. Illustrative tasks (written by Claude, NOT benchmark items)

These sketches show the shape the benchmark could take and give us something concrete to test the task template against. They are **not** candidates for the benchmark: real tasks come from practitioner interviews. To avoid anchoring, they should **not** be shown to interviewees before their episodes are elicited.

| # | Sector · stage · mode | Brief (one line) | Facets | Open substrate | Ground truth and grading |
|---|---|---|---|---|---|
| E1 | Fixed-wing · analysis · Produce | "Estimate clean-wing CL<sub>max</sub> for the PDR package; state the method and its uncertainty." | F1, F5 | XFOIL, AVL or OpenVSP/VSPAERO | NACA/NASA wind-tunnel data for a lesser-known planform; interval score; critical: does not present an inviscid VLM result at high α as CL<sub>max</sub> |
| E2 | Fixed-wing · design · Produce | "Re-engining study: the new engine has 8% lower SFC but is 12% heavier. Quantify block-fuel and field-length effects and the balance and tail implications." | F6, F7 | Aviary / OpenMDAO or AeroSandbox | Reference runs plus expert bands; rubric on CG shift, reserves policy, second-order weight growth |
| E3 | Airframe structures · analysis · Produce | "Substantiate the attachment lug for these limit loads: margins of safety for all failure modes." | F3, F5, F8 | Python; CalculiX optional | Classical lug method reference with public MIL-HDBK-5J allowables; critical: ultimate factor 1.5 and fitting factor 1.15 (CS 25.303 / 25.625) applied, correct critical mode and sign of minimum margin |
| E4 | Composite structures · analysis · Check | "Independent check of this skin-panel buckling and strength calculation before it goes into the stress report." | F4, F8 | Python, laminate tools | Seeded errors from real practice (e.g. room-temperature-dry allowables used for a hot/wet-critical case); detection is critical; false alarms penalised |
| E5 | Manufacturing · quality · Produce | "Non-conformance: a fastener hole in a machined fitting has 1.6D edge distance instead of 2D. Use as is, repair or scrap?" | F5, F6, F8 | Python | Recomputed bearing and shear-out margins; accepted disposition set; rubric on repair scheme and concession substantiation |
| E6 | Aero-engine · operations · Diagnose | "Fleet EGT-margin trend stepped after a shop visit. From these gas-path deltas, which module is degraded and what do you recommend?" | F9, F4 | pyCycle model with injected faults | Injected fault known by construction; module identification is critical; magnitude band; action rubric |
| E7 | Rotorcraft · operations/performance · Produce | "Can the helicopter hover out of ground effect at 3,000 m, ISA+20, at this take-off mass for a mountain rescue? Give the margin." | F1, F5, F10 | Momentum/BEMT scripts; DUST optional | Reference calculation anchored to public rotor test data; critical: correct go/no-go; rubric on figure of merit, tail-rotor and transmission power, engine lapse |
| E8 | UAV · design · Produce | "Select motor, propeller and battery from this catalogue for 25 min hover with a 2 kg payload at 1,500 m; justify control-authority margin." | F6, F5 | Python propulsion model | Many valid answers: the grader evaluates the chosen configuration, checks constraints and the gap to the optimum; rubric on thrust-to-weight, current and thermal margins |
| E9 | Launch vehicle propulsion · test · Produce | "We want to sea-level hot-fire a vacuum nozzle (ε = 40). Will the flow separate, what are the risks, what do you recommend?" | F1, F10 | NASA CEA, Python | Separation-criterion range; rubric on side loads and test-article options. Textbook fidelity only (export control) |
| E10 | Spacecraft · design · Produce | "12U CubeSat, 550 km SSO, LTAN 10:30: size arrays and battery for this payload duty cycle. Is it power-feasible with the required margin?" | F2, F5, F10 | Orekit or Basilisk, Python | Eclipse fraction checked deterministically; power-balance bands; rubric on EOL degradation, depth of discharge, margin policy; critical: flags an unsustainable peak mode if the work package contains one |
| E11 | Spacecraft · operations · Diagnose | "Three days of reaction-wheel speed and current telemetry: what is happening, how urgent is it, what do you command?" | F9, F10 | Basilisk simulation with a fault modelled on real wheel failures | Injected fault known; rubric on diagnosis, urgency and a safe commanding plan |
| E12 | Flight dynamics · analysis · Produce | "From the JSBSim model, extract the short-period and phugoid modes at cruise and assess them against MIL-F-8785C Level 1. Is a pitch damper needed?" | F3, F8 | JSBSim, python-control | Reference linearisation with tolerances; rubric on class and flight-phase category |
| E13 | Systems · certification · Produce | "FHA and quantitative FTA for runaway of an electric pitch-trim actuator on a CS-23 Level 3 aircraft. Does the architecture meet AC 23.1309-1E?" | F8, F5, F7 | Python fault-tree tools | Severity classification is critical; probability arithmetic checked deterministically; rubric on latent failures, exposure times and mitigations |
| E14 | UAV flight software · maintenance · Diagnose | "The field log shows the geofence failsafe not triggering after a mode switch. Find and fix the bug and show SITL evidence." | F9, F4 | PX4 or ArduPilot SITL | A real issue fixed after the models' training cutoffs; hidden SITL tests plus a code-review rubric |
| E15 | Turboprop · maintenance · Diagnose | "Intermittent bleed-air fault on a (fictional) turboprop: write the fault-isolation plan and decide whether to dispatch." | F9, F10, F8 | Fictional but realistic documentation package | Expert rubric; critical: no unsafe dispatch decision |

## 5. Validation protocol (how a task earns its place)

A task enters the pilot only after passing every gate below; each gate's outcome is recorded.

1. **Traceability:** linked to a real interview episode (pseudonymous ID).
2. **Authenticity:** the source engineer and one independent engineer each rate "representative of real work" and "difficulty comes from the engineering" at ≥4/5, and the task passes the anti-pattern checklist below.
3. **Correctness:** blind, time-uncapped re-solve by a second competent engineer. Disagreements are reconciled, with a third expert if needed. Tolerances are justified from experiment, workshop scatter or measured solver spread.
4. **Solvability and grader checks:** the reference solution passes the grader; a null or trivial submission scores about zero; perturbed variants behave as expected.
5. **Human baseline:** the re-solver's *initial* blind deliverable is graded with the same rubric and timed, which gives a human score and time at no extra cost.
6. **Grader validity:** two experts double-grade a stratified sample of model outputs (target ~750–1,000 criterion-level judgements). Report κ or Krippendorff's α per criterion type; rewrite criteria below α = 0.667 rather than delegating them to an LLM judge. The judge must come from a different model family from the models being ranked, and must be blinded to model identity.
7. **Pre-release audit:** a stratified error audit of the finished set. Epoch AI rates a benchmark "Flawed" at ≥20% erroneous tasks; we should aim well below that (e.g. <5%) and publish the measured rate.

**Role of LLMs in development (no circularity):** models are never used to propose tasks. They may be run on draft tasks only to expose ambiguities, shortcuts and possible key errors; engineers decide every change, and each such use is logged in the task metadata.

**Anti-pattern checklist** (reject or revise the task if any apply):

- constraints that would not exist in the real job (puzzle constraints);
- information withheld that a real work package would contain;
- output formats engineers do not use;
- recall of obscure values an engineer would look up (provide the reference instead);
- reliance on incidental model weaknesses (character counting, long hand arithmetic when tools are available);
- a single "gotcha" with no engineering consequence;
- dependence on paywalled standard text or proprietary software.

## 6. Elicitation: interview protocol outline

About 90 minutes per engineer, recorded only with consent; target 2–3 usable episodes per interview.

1. **Context (10 min):** role, sector, experience, typical tools, typical work products.
2. **Work mapping (15 min):** recurring tasks, mapped live onto the O*NET task list; where errors are costly; what juniors get wrong.
3. **Critical incidents (40 min):** Critical Incident Technique and Critical Decision Method: "Tell me about a recent piece of work where getting it right needed judgement: choosing a method, a result that looked wrong, conflicting requirements, a fault you had to track down." Probe the timeline, decision points, cues, what a novice would miss, how the result was verified, and what would have happened if it were wrong.
4. **Task feasibility (15 min):** the inputs needed, what a correct deliverable looks like, how they would grade it, acceptable answer ranges, which software they would use and whether an open-source alternative exists, and confidentiality constraints.
5. **Knowledge audit (10 min):** Applied Cognitive Task Analysis questions on expertise ("What's hard here for someone two years in?").

Afterwards: the episode write-up is returned to the engineer for confirmation. Confidential details are replaced APEX-Agents-style with a fictional but realistic "world" that keeps the reasoning structure. Each task passes an export-control screen before it is written up.

## 7. Pilot size, people and effort (rough planning figures)

| Item | Proposal |
|---|---|
| Tasks | 30–60, recommended **~40**: 5 strata × ~8 tasks, with a per-engineer cap so no single source dominates |
| Engineers interviewed | 12–20 (at 2–3 usable episodes each) |
| Expert task length | Median ~2 h, range 1–4 h of competent-engineer time; keeps re-solving affordable |
| Models | 4–6: current frontier models from Anthropic, OpenAI and Google, plus 1–2 open-weight models |
| Runs | ≥3 per model–task pair (≈720 agent runs for 40 × 6 × 3) |
| What 40 tasks can show | ~±14 percentage points at 95% on a binary rate; only gaps of ≥~13 points between models are detectable. Enough to validate the instrument and size the full benchmark, not to rank close models |

Rough effort for 40 tasks (to be refined once we know task lengths):

- **Interviews:** ~25–30 h of engineers' time, plus your time to run and write them up.
- **Task authoring:** ~6–10 h per task for whoever converts episodes into packages (~240–400 h in total).
- **Source review:** ~1 h per task (~40 h).
- **Blind re-solves:** ~4–8 h per task (~160–320 h).
- **Double grading** for judge validation: ~50–70 h.

That is roughly **300–450 hours of external expert time**, the main cost and the main schedule risk. Model API costs are likely in the low thousands to low tens of thousands of USD, depending on task length; we will estimate them properly before any run.

## 8. Legal, ethics and release

- **Interview data and consent:** UK/EU GDPR applies to interview recordings and notes. If the work is published academically, run it through a university research-ethics process. Consent forms must cover recording, use of the episode, IP, and confirmation that nothing employer-confidential is disclosed. No published benchmark shares these documents, so publishing ours would exceed current practice.
- **Export control:** screen every task against the UK Strategic Export Control Lists, EU Regulation 2021/821 and, where US-origin content or contributors are involved, the EAR and ITAR. ITAR has no "posted online, therefore public" route. Recommendation: exclude military systems, and keep launch-vehicle propulsion and guidance at textbook or conceptual fidelity. Get counsel review before public release.
- **Data and tool licences:** prefer CC0, public-domain and permissively licensed data; fetch no-licence sources (e.g. UIUC airfoils) at runtime rather than re-hosting them. Avoid CEASIOMpy (proprietary since Feb 2026) and OpenSees (non-commercial only); treat AGPL tools (RCAIDE) with care.
- **Release:** a public development split with a canary string, and a private test split held back. Publish a datasheet with expert demographics, the elicitation protocol, review stages, agreement statistics and the measured error rate, plus a versioned changelog.

## 9. Main risks

| Risk | Mitigation |
|---|---|
| Expert time runs out | Keep tasks short; recruit re-solvers from academia (postdocs, PhD students) for analysis-heavy tasks and keep practitioners for judgement-heavy ones |
| Rubric grading of open-ended design is unreliable | Atomic typed criteria, double grading, α threshold, assumption-conditional grading |
| Contamination from famous public cases (NACA 0012, Caradonna–Tung) | Prefer lesser-known configurations, perturbed variants and post-cutoff material; log first exposure |
| Execution-tier tasks saturate fast | Weight toward judgement-heavy, cross-disciplinary, safety-gated tasks; plan refreshes from new interviews |
| Harness confounds model comparison | Common open harness; disclose everything; report per model–harness pair |
| Breadth ambition spreads the pilot too thin | 5 strata in depth for the pilot; broaden after the instrument is validated |
| Export control or confidentiality incident | Screen before authoring; fictionalised worlds; counsel review |

## 10. Phased plan and gates

| Phase | Output | Gate to proceed |
|---|---|---|
| 0 Landscape (done) | Research report | — |
| 1 Design spec (docs only) | Construct and taxonomy; task-package template and rubric schema; interview guide, consent and export-screening checklist; validation protocol and statistical analysis plan | You approve the spec |
| 2 Elicitation | Interview dry run with 1–2 friendly engineers, protocol revision, then 12–20 interviews and episode write-ups | Enough authentic episodes per stratum |
| 3 Task construction and verification | Task packages, containers, reference solutions, blind re-solves, reconciled keys | Every task passes §5 gates 1–5 |
| 4 Pilot runs and grading | Model runs, expert double grading, judge validation | Judge–expert agreement adequate |
| 5 Analysis and release decision | Error-rate audit, agreement and variance statistics, power analysis for the full benchmark, datasheet | Error rate below the threshold we set |

Implementation (harness, containers, task code, model runs) starts in Phase 3 and only after Phase 1 is agreed.

## 11. What Claude will draft next (Phase 1), once the open questions are answered

1. `docs/construct_and_taxonomy.md`: construct, coverage frame, facet definitions, O*NET mapping, pilot strata.
2. `docs/task_package_spec.md`: task template, deliverable conventions, rubric schema, metadata, worked example using one illustrative task.
3. `docs/interview_protocol.md`: interview guide, consent and IP language, export-screening checklist, episode write-up template.
4. `docs/validation_and_analysis_plan.md`: validation gates, grading-agreement study, metrics, statistical analysis plan, release criteria.
5. A targeted follow-up search to close the literature gaps the research could not reach (Connolly's SwRI AIAA 2025-0702 set; VFS, ERF, ICAS, RAeS and IAC proceedings; Chinese-language venues) before any novelty claim.
