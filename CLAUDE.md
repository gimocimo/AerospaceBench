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
- Keep the brief, decisions and outputs in this file and `docs/` as we go; update the decision log whenever Guglielmo decides something. Commit and push updates to the GitHub repo (D24).
- **Never push task content to the public repo:** instances, reference solutions, withheld conditions, verifier test data, or candidate task packages. Anything public may enter future training data. Keep such material in `private/` (git-ignored) until a private location is agreed (open question in `docs/benchmark_design.md` §11).
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

## Status

- **Phase 0 — landscape research:** done (2026-10-06).
- **Phase 1 — design:** framework v0.3 drafted (`docs/benchmark_design.md`), combining Guglielmo's framework with Claude's [proposal] refinements. Waiting for Guglielmo's answers to its §11 questions: private task workspace, git workflow, pilot shape and scope, episode length, feasibility mix, human reference, cross-vendor red team, Q15 timing, next step.
- **Next:** 2–3 candidate pathfinder family briefs (G1 format) for Guglielmo to select from; then contacts research; then a very simple consultation plan.
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
- `docs/pilot_scope_options.md` — v0.2. Options C and F3 chosen; its short-task design and family menu are superseded but kept as a parts bin.
- `docs/pilot_proposal.md` — v0.1; partly superseded.
- `docs/open_questions.md` — Q1–Q16 with Guglielmo's answers; newer questions live in `benchmark_design.md` §11.
- `reports/Aerospace engineering LLM benchmarks.md` — Phase 0 landscape report.
- `research_notes/Aerospace engineering LLM benchmarks/` — six raw research-note files behind the report.
