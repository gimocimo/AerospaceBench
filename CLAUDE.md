# AerospaceBench — project memory

Read this first in every session. It holds the brief, the working rules, the decision log and an index of outputs. Detailed material lives in `docs/`, `reports/` and `research_notes/`. The repository is **public**: github.com/gimocimo/AerospaceBench.

## Brief (from Guglielmo, 2026-10-06; amended by later decisions as noted)

- **Goal:** a benchmark measuring how well frontier LLMs and LLM agents perform demanding aerospace engineering work: design, analysis, test, certification, manufacturing, operations and maintenance, across aircraft, rotorcraft, drones, launch vehicles, spacecraft and their propulsion, systems and software.
- **Secondary motivation (not the main goal):** aerospace draws on most engineering disciplines, so the benchmark may serve as a proxy for general engineering ability.
- **Coverage ambition:** broad, across aerospace sub-disciplines and sectors. The pilot covers selected areas in depth (option C, D19); the full benchmark is a curated core plus contributed modules (F3, D20).
- **Central question (D23):** "Can the model produce a reproducible engineering result, demonstrate that it satisfies the requirements, and recognise when the available evidence does not justify its conclusion?" Tasks are multi-hour, end-to-end engineering projects (autonomous engineering), not question answering.
- **Source of difficulty:** coupled physical reasoning, choosing and validating models, active investigation under a budget, maintaining correctness across an evolving project (requirement changes), and justified no-go decisions; plus using engineering software where the work needs it.
- **Avoid artificial difficulty:** no puzzle constraints, deliberately vague specifications, unrealistic formats, or anything exploiting incidental LLM weaknesses rather than engineering competence. Planted faults in supplied projects must mirror real practice and be discoverable through competent work.
- **First milestone:** a small, rigorously validated pilot whose results are established by independent verifiers and trusted by experts.
- **Task creation (D22):** co-created. Claude acts as an adversarial task generator and proposes candidates across fields; Guglielmo steers with experience, taste and engineering judgement and decides what is included; experts validate and enhance when needed. A task counts as beyond a model's reliable capability only after reproducible failure under a fair, independently verified evaluation.
- **Open-source only for the pilot:** anyone must be able to rerun it. No task may require a proprietary tool (MATLAB/Simulink, MSC Nastran, ANSYS, Abaqus, CATIA, STK, …). A licensed track may come later.
- **Purpose (D6):** an independent personal project. Core: scientific quality of the benchmark and a solid GitHub repository. Additions: a peer-reviewed conference paper on the validated pilot, release of the full benchmark, a public leaderboard.
- **Owner:** Guglielmo Cimolai — Italian, based in London. Aerospace Engineering BSc (Politecnico di Milano), Aerospace Structures MSc (Cranfield), AI MSc (Imperial College London); also an ISAE-Supaero alumnus. The only human author and decision-maker; 10–15 h/week; no target dates (D16).

## Working rules

- No implementation (harness, environments, verifiers, model runs) until the design and validation approach are agreed with Guglielmo.
- Keep the brief, decisions and outputs in this file and `docs/` as we go; update the decision log whenever Guglielmo decides something. Commit and push updates to GitHub, straight to `main` in both repos, with no branches or pull requests (D24, D27).
- **Never push task content to the public repo:** instances, reference solutions, withheld conditions, verifier test data, candidate family briefs or fault catalogues. Anything public may enter future training data. Task content lives only in the private repo `gimocimo/AerospaceBench-tasks`, cloned at `private/` (git-ignored here) (D26). Every private file carries the canary GUID recorded in `private/README.md`. Never copy that GUID into the public repo.
- Public docs may name families at a high level, but must not reveal hidden parameters, faults, references or withheld conditions.
- **People Guglielmo contacts stay private (D54):** contact lists, outreach drafts, consultation notes, transcripts, write-ups, and anything else that identifies a contacted person live only in the private repo (`private/contacts/`). Never put their names or identifying details in the public repo, its commit messages or `CLAUDE.md`. The public `.gitignore` has a safety net for such files.
- Never commit third-party PDFs (copyright); cite by link. `*.pdf` is git-ignored.
- Claude-generated candidate tasks enter the benchmark only after Guglielmo's selection and the generation protocol in `docs/benchmark_design.md` §6: coherence and solvability checks, a verifier test suite, fresh-instance trials, and provenance recorded.
- Sequence (D18): agree the pilot scope and task families first, then research contacts, then a very simple consultation plan.
- Do not assume answers to Q14–Q16 (environment and task modes, models and harness, release, licence and metric). Ask Guglielmo again when they become relevant (D17). They are now relevant to the evaluation design: ask before deciding anything that depends on them.

## Decision log

| ID | Date | Decision | Source |
|----|------|----------|--------|
| D1 | 2026-10-06 | Pilot uses open-source software only; licensed track possibly later | Guglielmo |
| D2 | 2026-10-06 | Defer all implementation until task design and validation approach are agreed | Guglielmo |
| D3 | 2026-10-06 | ~~Tasks grounded in interviews with practising engineers~~ (superseded by D9, then D22); expert-trusted grading still applies | Guglielmo |
| D4 | 2026-10-06 | First deliverable: deep research of existing benchmarks, then a proposal and questions | Guglielmo |
| D5 | 2026-10-06 | ~~Start expert interviews within days~~ (superseded by D18) | Guglielmo |
| D6 | 2026-10-06 | Purpose: scientific quality and a solid GitHub repo are the core; peer-reviewed conference paper on the validated pilot, full benchmark release and public leaderboard are additions. Independent personal project | Guglielmo (Q1) |
| D7 | 2026-10-06 | Run independently of any university. Experts are contributors, not research subjects: written consent plus a short contributor agreement. No co-authors expected | Guglielmo (Q2) |
| D8 | 2026-10-06 | Guglielmo is in London (UK); contributors are UK or European. Exclude military systems. Contributors speak in a personal capacity about non-confidential, non-controlled material | Guglielmo (Q3) |
| D9 | 2026-10-06 | Experts give short, directed input on specific tasks (choosing between alternatives, targeted domain expertise, validation); no large-scale elicitation; Claude researches suitable contacts once an initial task list exists. (~~Guglielmo authors all tasks~~, superseded by D22) | Guglielmo (Q4) |
| D10 | 2026-10-06 | Pilot covers selected areas in depth, then expands with the same methodology. Suggested areas: airframe structures; aerodynamics and aircraft performance/design; propulsion | Guglielmo (Q5) |
| D11 | 2026-10-06 | Consultations may be recorded with consent and transcribed locally; Claude may process transcripts to draft write-ups; details later | Guglielmo (Q6) |
| D12 | 2026-10-06 | English only | Guglielmo (Q7) |
| D13 | 2026-10-06 | Contributors unpaid, with acknowledgement; compensation for industry professionals may be discussed later | Guglielmo (Q8) |
| D14 | 2026-10-06 | Individuals only for the pilot; institutional partnerships reserved for future versions. Claude's advice: approach Imperial Aeronautics academics as named methodology advisers after the pathfinder, not a formal partnership now | Guglielmo (Q9) |
| D15 | 2026-10-06 | Minimise cost: ideally only LLM usage, preferably through existing Claude and ChatGPT subscription plans rather than API or third parties. Frontier models only. Details belong to Q15, still open | Guglielmo (Q10) |
| D16 | 2026-10-06 | No target dates; 10–15 h/week; Guglielmo is the only human author and decision-maker (co-creation with Claude per D22) | Guglielmo (Q11) |
| D17 | 2026-10-06 | Q14–Q16 deferred; Claude must ask again later and not assume | Guglielmo |
| D18 | 2026-10-06 | Work through pilot scope and candidate tasks first; then select experts; then a very simple consultation plan | Guglielmo |
| D19 | 2026-10-07 | Pilot option C: three pillars (structures; aero, performance and design; propulsion) with sector variety and cross-disciplinary integration. Its reinterpretation for project-style families is pending (`benchmark_design.md` §9) | Guglielmo |
| D20 | 2026-10-07 | Full benchmark option F3: curated core across 10 domains × 5 sectors plus contributed modules under the published methodology | Guglielmo |
| D21 | 2026-10-07 | Expansion order after the pilot: flight dynamics and control; systems, safety and certification; spacecraft and launch; test, operations and maintenance diagnosis; manufacturing and quality; rotorcraft; avionics and flight software; licensed track later | Guglielmo |
| D22 | 2026-10-07 | Co-creation of tasks: Claude proposes candidates as an adversarial generator and validates coherence and solvability; Guglielmo steers and decides inclusion; experts validate and enhance when needed. Beyond-capability claims only after reproducible failure under fair, independently verified evaluation | Guglielmo |
| D23 | 2026-10-07 | Design direction: multi-hour end-to-end engineering projects. Work package = requirements, editable project files, documentation, data, open tools, submission interface; delivery = executable artifacts, results, assumptions, requirement-to-evidence map. Independent simulators and withheld operating conditions; acceptance first (hard requirements pass/fail), quality among accepted; primary result P(verified completion given family, difficulty and budget); claim integrity and confidence calibration; multi-dimensional difficulty ladders; empirical difficulty calibration; RE-Bench as structural precedent. Details and Claude's refinements in `docs/benchmark_design.md` | Guglielmo |
| D24 | 2026-10-07 | GitHub repo github.com/gimocimo/AerospaceBench (public) is the system of record; push updates there | Guglielmo |
| D25 | 2026-10-07 | Connolly-2025 and Mangortey-2025 assessed (no overlap; notes in `docs/related_work_notes.md`) and moved to the macOS Trash. ReBench-2024.pdf kept locally, not committed | Guglielmo |
| D26 | 2026-10-07 | Task content lives in a second, private repo: `gimocimo/AerospaceBench-tasks` (created by Claude with `gh`), cloned at `private/` | Guglielmo |
| D27 | 2026-10-07 | Commit straight to `main`; no branches or pull requests | Guglielmo |
| D28 | 2026-10-07 | Pilot option C confirmed as ~5–6 families with difficulty ladders; pathfinder family first | Guglielmo |
| D29 | 2026-10-07 | Pilot stays within the three pillars (structures; aero, performance and design; propulsion) | Guglielmo |
| D30 | 2026-10-07 | Episode length and wall-clock caps to be decided during task design | Guglielmo |
| D31 | 2026-10-07 | Feasibility mix ~70% feasible / ~15% infeasible / ~15% underdetermined, undisclosed per instance | Guglielmo |
| D32 | 2026-10-07 | No human reference for now; look for one if the pilot succeeds | Guglielmo |
| D33 | 2026-10-07 | ChatGPT may red-team Claude-generated task packages. GPT-generated families not yet approved; ask first | Guglielmo |
| D34 | 2026-10-07 | Q15 (models and harness) opened for discussion; options in `docs/models_and_harness.md` | Guglielmo |
| D35 | 2026-10-07 | Claude to provide 2–3 candidate pathfinder briefs (delivered in `private/candidates/pathfinder_briefs.md`) | Guglielmo |
| D36 | 2026-10-12 | Pathfinder family: P-A (wing-box static-test correlation and substantiation) | Guglielmo |
| D37 | 2026-10-12 | Subscriptions: Claude Max 5x and ChatGPT Pro 5x | Guglielmo |
| D38 | 2026-10-12 | Systems under test (four): Claude Code + Claude Fable 5.1; Claude Code + Claude Opus 5.5; Codex CLI + GPT-6 Astra; Codex CLI + GPT-5.6 Sol | Guglielmo |
| D39 | 2026-10-12 | Results framed as model-plus-native-agent systems; token budgets measured, not enforced (option H1 in `docs/models_and_harness.md`) | Guglielmo |
| D40 | 2026-10-12 | Episodes run on this Mac in Docker containers | Guglielmo |
| D41 | 2026-10-12 | No API fallback budget for now; revisit if subscription policies or limits get in the way | Guglielmo |
| D42 | 2026-10-12 | GPT-generated task families approved (cross-vendor generation, so generator × solver effects can be measured) | Guglielmo |
| D43 | 2026-10-12 | P-A test article (metallic, unswept ~3 m wing-box segment) accepted as proposed | Guglielmo |
| D44 | 2026-10-12 | P-A fault catalogue F1–F7 accepted pending expert review; correlation criterion (±10% / ±50 µε; ±5% / 0.2 mm) and test-decision rule (GO / GO-WITH-HOLD / NO-GO; no skin buckling below limit) assumed until experts are asked | Guglielmo |
| D45 | 2026-10-12 | Supplied-tool fault (mid-surface strain extraction) removed as too much of a gotcha | Guglielmo |
| D46 | 2026-10-12 | P-A investigation costs and budgets accepted | Guglielmo |
| D47 | 2026-10-12 | P-A technical spike approved (run the same day; `private/families/pa-wingbox-correlation/spike/REPORT.md`) | Guglielmo |
| D48 | 2026-10-12 | 2-page scenario brief (PDF, with figures) for expert consultations; expert contacts research started | Guglielmo |
| D49 | 2026-10-12 | All five spike changes approved and folded into P-A spec v0.3 (fault magnitudes vs criterion with "excellence" items; discriminating withheld cases and margin locations via a wrong-model check; key-gauge rules; Z-stringers and realistic imperfections; deeper box with nonlinear truth at 100%) | Guglielmo |
| D50 | 2026-10-12 | A- vs B-basis design values are a judgement item, at clear-cut locations only, confirmed with experts | Guglielmo |
| D51 | 2026-10-12 | Episode caps shorter, to make the benchmark harder: halved to 0.5 / 1 / 1.5 / 2 / 3 / 3 + 1 h | Guglielmo |
| D52 | 2026-10-12 | Simple consultation plan drafted (`docs/consultation_plan.md`). Scenario brief rewritten as v0.2: max 5 pages, plain-language explanations of every term, assumption and decision, gradual introduction | Guglielmo |
| D53 | 2026-10-12 | Option (a): draft the remaining pilot family briefs (done: P-D turboshaft pass-off diagnosis, P-E electric VTOL drone sizing, P-F wing-skin repair; P-B/P-C updated; a GPT generation prompt prepared) before building P-A | Guglielmo |
| D54 | 2026-10-12 | Contact names and anything identifying contacted people stay out of the public repo (private repo only; public `.gitignore` safety net) | Guglielmo |
| D55 | 2026-10-12 | First outreach emails drafted for three reviewers (kept in `private/contacts/outreach_drafts.md`) | Guglielmo |
| D56 | 2026-10-12 | First three outreach emails sent; tracked in `private/contacts/tracker.md` (follow-up ~2026-10-20 if no reply) | Guglielmo |
| D57 | 2026-10-12 | First cross-vendor generation done: ChatGPT (GPT-6 Astra Max) proposed X-1 cold-gas spacecraft propulsion qualification, X-2 sailplane longitudinal control and CG, X-3 helicopter skid-gear impact. Claude's G3 assessment recommends X-1 for the G slot, X-3 as alternate, X-2 for expansion domain 1. Six fairness and abstention rules adopted into the framework (`benchmark_design.md` §4.6). Slate selection pending Guglielmo | Guglielmo |
| D58 | 2026-10-12 | Pilot slate confirmed: P-A, P-B, P-D, P-E, P-F, X-1 (cold-gas spacecraft propulsion, GPT-generated). Alternates P-C and X-3; X-2 kept for expansion domain 1 | Guglielmo |
| D59 | 2026-10-12 | Go-ahead to build the P-A environment (first implementation beyond the spike). Build in milestones, starting with an end-to-end walking skeleton; track effort to calibrate the other families | Guglielmo |
| D60 | 2026-10-13 | P-A build changes approved: lower skin 3.2 mm nominal (range 2.8–3.5 mm) and a Python structured mesher instead of Gmsh for the supplied model (spec v0.4 build notes) | Guglielmo |
| D61 | 2026-10-13 | Go-ahead for P-A milestone M2 (fault catalogue, discriminating margin locations, wrong-model check at generation) | Guglielmo |
| D62 | 2026-10-14 | Keep the 90% key-gauge rule for correlation and withheld predictions for now; for local faults the diagnosis and margin gates carry the discrimination. Revisit with the expert reviewers | Guglielmo |
| D63 | 2026-10-14 | Go-ahead for P-A milestone M3 (nonlinear truth, buckling rung R4, decision gate) | Guglielmo |

## Status

- **Phase 0 — landscape research:** done (2026-10-06).
- **Phase 1 — design:** framework v0.3 (`docs/benchmark_design.md`); its §11 questions were answered on 2026-10-07 (D26–D35).
- **Now (2026-10-12):**
  - Pilot slate confirmed (D58). Family briefs: `private/candidates/pilot_family_briefs.md` and `private/candidates/gpt_families_assessment.md`.
  - P-A spec v0.3 plus v0.4 build notes in `private/families/pa-wingbox-correlation/spec.md`; scenario brief v0.2 in `brief/`.
  - **P-A environment build (D59–D61): milestones M1 (walking skeleton, 2026-10-12) and M2 (fault catalogue and wrong-model check, 2026-10-14) done** in `private/families/pa-wingbox-correlation/env/`. The generator builds instances for rungs R0–R3 and admits one only if the reference solution passes and every wrong model in a fixed library is rejected. The self-test passes on seven instances. Plan, findings and open items: `env/BUILD_PLAN.md`, `env/BUILD_LOG.md`. One design observation awaits Guglielmo: for local faults, the diagnosis and margin gates do most of the discriminating (BUILD_LOG, M2).
  - Outreach: three emails sent 2026-10-12 (`private/contacts/tracker.md`); follow up ~2026-10-20 if no reply.
- **Next:**
  - P-A M3 in progress (nonlinear truth, buckling rung R4, decision gate). It found that the solver's buckling factors were misleading and that the M1/M2 test article buckled below limit; a revised test article awaits Guglielmo's approval (spec v0.4 note 10). Then M4–M7 per `env/BUILD_PLAN.md`.
  - Optionally, a ChatGPT red-team of the P-A spec (D33).
  - Expert replies feed spec v0.4.
- **Environment facts:**
  - Host: Apple M2 Pro (10 cores, 16 GB RAM).
  - Docker 29 installed (daemon not running on 2026-10-07).
  - Claude Code CLI 2.1.143 installed; Codex CLI not yet.
  - Solver images must support ARM: CalculiX 2.23 and Gmsh 4.15 are on conda-forge for linux-aarch64. PyPI Gmsh wheels are macOS-ARM only, so containers use conda/micromamba.
  - Docker Desktop is now running (8 GB, 10 CPUs). P-A image `abench-pa-env:0.1` (micromamba + CalculiX 2.23) is built locally.
- **Repo:** `main` pushed to GitHub on 2026-10-07, after transient GitHub "Internal Server Error" responses on the first attempts (they resolved within minutes).

## Key findings from Phase 0 (details in the report)

- No existing benchmark measures aerospace *engineering work*. About two dozen aerospace LLM evaluations exist (mostly 2025–2026), dominated by MCQ recall on aviation ops, maintenance and regulations (frontier models 60–83%).
- The closest analogues are small, single-sector, verifier-graded agent benchmarks: AstroAgentBench (satellite planning, physics verifier), the GTOC-12 agent study (no valid submission from any model), AeroCopilotBench (safety-gated cockpit procedures, pass^3), RocketBench (RocketPy; unreleased), APBench (astrodynamics, numeric tolerance).
- Essentially unmeasured: rotorcraft, aerostructures, certification and safety assessment, test and AIT, orbital launch-vehicle design, spacecraft subsystem budgets, UAV sizing, flight-software generation and verification.
- Adjacent lesson: textbook calculation is near saturation, while executable design is about one-third; "it ran" overstates CFD agents by about 50 points; failures are silent (boundary conditions, units, factors, no verification).
- Methodology lesson: answer-key verification and grader validity, not authors' credentials, set quality (HLE, FrontierMath v2, SWE-bench Verified, SciCode-Verified audits). Harness choice moves agent scores by tens of points.
- An open-source-only toolchain covers most disciplines at conceptual and preliminary fidelity. Hard holes: rotorcraft comprehensive analysis, damage tolerance, Nastran-class flutter, paywalled DO-178C and ARP standards. Licence traps: CEASIOMpy (proprietary since Feb 2026), OpenSees (non-commercial), RCAIDE (AGPL); FUN3D, Cart3D and OVERFLOW are US-only.
- Export control: ITAR has no internet-posting route to the public domain; authoring new launch-vehicle or military design content is the main risk.
- Caveat: AIAA ARC, VFS, ERF, ICAS, RAeS, IAC proceedings and Chinese-language venues were not systematically searchable, so "unmeasured" means absent from the indexed open literature. Connolly (AIAA 2025-0702) and ALUE (AIAA 2025-3247) have since been read: no overlap.

## File index

- `CLAUDE.md` — this file.
- `README.md` — public project description.
- `docs/benchmark_design.md` — **current** design framework (v0.3): central question, unit of evaluation, difficulty ladder, acceptance-first evaluation, verifiers, co-creation protocol, RE-Bench comparison, pilot implications, risks, open questions.
- `docs/related_work_notes.md` — notes on Connolly 2025, ALUE 2025 and RE-Bench.
- `docs/models_and_harness.md` — Q15 options for running models (vendor agents on subscriptions vs a common harness via APIs), with checked facts and a recommendation.
- `docs/consultation_plan.md` — simple expert consultation process: outreach templates, call outline, contributor note, write-up template.
- `private/` — clone of the private repo `gimocimo/AerospaceBench-tasks` (git-ignored here): candidate briefs, and later family specifications, instances and references.
- `docs/pilot_scope_options.md` — v0.2. Options C and F3 chosen; its short-task design and family menu are superseded but kept as a parts bin.
- `docs/pilot_proposal.md` — v0.1; partly superseded.
- `docs/open_questions.md` — Q1–Q16 with Guglielmo's answers; newer questions live in `benchmark_design.md` §11.
- `reports/Aerospace engineering LLM benchmarks.md` — Phase 0 landscape report.
- `research_notes/Aerospace engineering LLM benchmarks/` — six raw research-note files behind the report.
