# AerospaceBench design framework (draft v0.3)

*2026-10-07. This captures Guglielmo's framework (message of 2026-10-07) and Claude's proposed refinements, which are marked **[proposal]**. Nothing marked [proposal] is agreed until it appears in the decision log in `CLAUDE.md`. Open questions are in §11. This document supersedes the task-format parts of `pilot_proposal.md` (v0.1) and §4–§7 of `pilot_scope_options.md` (v0.2).*

## 1. Central question

> **Can the model produce a reproducible engineering result, demonstrate that it satisfies the requirements, and recognise when the available evidence does not justify its conclusion?**

The benchmark targets **autonomous engineering**: formulating, investigating, implementing, verifying and revising a solution in a controlled engineering environment. Success is established independently of how convincing the explanation sounds. It does not target question answering, recall or pure mathematical reasoning.

The bet is that the strongest design is not exceptionally hard aerospace questions. It is high-quality task families, trusted verifiers and a renewable difficulty ladder.

## 2. Unit of evaluation

| Level | Meaning |
|---|---|
| **Task family** | An engineering environment (e.g. "aero-structural wing modification") with its simulators, verifier and ladder generator |
| **Variant** | One point on the difficulty ladder: a setting of the ladder dimensions (§3), including its feasibility class (feasible, infeasible, underdetermined) |
| **Instance** | A concrete, procedurally generated parameterisation of a variant (numbers, geometry, injected faults), with hidden withheld conditions |
| **Episode** | One agent attempt on an instance under a declared resource budget. It may have several stages (requirement changes) |

### 2.1 Work package (what the agent receives)

- An authoritative **requirements specification** with requirement IDs.
- A bounded set of **editable project files** plus read-only files.
- **Documentation**, including the stated validity envelope of every supplied model.
- **Data:** component data, telemetry or test data, as the family needs.
- **Tools:** open-source solvers and Python in a sandbox.
- **[proposal]** A **development check**: the agent can evaluate designs at nominal conditions, as an engineer uses their analysis tools. Acceptance uses withheld conditions (§5.3).
- For diagnostic families, an **investigation interface** with a declared cost per action (§3, D3).
- A clearly defined **submission interface.**

### 2.2 Submission (what the agent delivers) [proposal for the concrete format]

- **Executable artifacts** (design files, code, input decks) runnable through a declared entry point.
- **Results** and an **assumptions register.**
- A structured **requirement-to-evidence map.** For each requirement ID:
  - status claimed: met, not met, or cannot determine;
  - evidence artifact and the command that reproduces it;
  - claimed value and margin;
  - confidence (0–1).
- For no-go or underdetermined conclusions, a **structured argument:**
  - the binding requirements;
  - the quantitative bound, or the specific missing information;
  - which decisions depend on it.
- A short engineering memo. Communication quality is not scored beyond clarity of evidence.

## 3. Difficulty ladder

Several independently adjustable dimensions replace a single "hardness" setting.

| Dimension | What it varies | Example |
|---|---|---|
| **D1 Coupling** | Feedback between decisions, not a longer list of calculations; harder variants change the coupling structure, add discrete architecture choices, or shift which constraints dominate | Wing geometry ↔ loads ↔ stiffness ↔ deformation ↔ aero performance ↔ mass |
| **D2 Choosing and validating the model** | The supplied analysis runs and looks plausible but is unsuitable for the decision: a linear model outside its envelope, an inappropriate boundary condition, an extrapolated surrogate | Recognise it, choose an alternative or additional test, revise the conclusion |
| **D3 Active investigation under a budget** | Information is not all given upfront. Each action costs budget: request a measurement, run a different simulation, inspect a subsystem, change an excitation, run a sensitivity study | Diagnose sensor vs actuator vs dynamics-model cause with limited tests |
| **D4 Requirement change and propagation** | Staged episodes: after the first submission, a predefined change arrives (payload mass, power, envelope, corrected material property) | Update affected work, preserve what remains valid; both stages scored |
| **D5 Genuine uncertainty and no-go** | A controlled mix of feasible, infeasible and underdetermined instances | Infeasible: authors hold a defensible argument (incompatible requirements, physical lower bound). Underdetermined: the model must name the missing information and what depends on it |
| **D6 Evidence quality** [proposal] | Measurement noise, sparse sensors, calibration offsets, conflicting data sources, kept separate from D2 (wrong model) | The same diagnosis with noisier or partially contradictory telemetry |

**Ladder construction rules [proposal]:**

1. **Rung 0 is a calibration rung:** single discipline, clean, feasible, all information given. A competent agent should pass it. If frontier agents fail rung 0, suspect the environment first.
2. **One dimension per rung** where possible, so failures can be attributed; a few combined rungs at the top.
3. **Feasibility classes are mixed and undisclosed.**
   - Claiming no-go on a feasible instance is a failure.
   - Delivering a "design" for an infeasible instance is a failure.
   - Target mix to be decided (§11).
4. **Faults planted in supplied projects** (D2) must be:
   - realistic: patterns seen in practice, confirmed by Guglielmo or an expert;
   - discoverable through competent practice: a documented validity envelope, residuals, comparison with test data;
   - consequential for the decision.

   Fault-free controls are included, so finding faults is never rewarded by default and false alarms are measured.
5. **Software friction is held low and constant; it is not a ladder dimension.** Installation pain, meshing headaches and obscure APIs make failures hard to attribute. We want engineering complexity, not toolchain friction.
6. **Difficulty is defined a priori by the ladder settings, then measured empirically.** A variant counts as beyond reliable capability only after reproducible failure under a fair, independently verified evaluation, and after task and harness defects are ruled out.

## 4. Evaluation: acceptance first, then quality

### 4.1 Acceptance (pass/fail per episode)

A submission is accepted only if:

- **A1 Reproducible:** the verifier re-runs the declared entry point in a clean container and regenerates the claimed results within tolerance.
- **A2 Requirements satisfied:** every hard requirement is met across the required test cases, including withheld operating conditions, as evaluated by the independent verifier.
- **A3 Evidence consistent:** every "met" claim in the evidence map is supported by the re-run and by the verifier. No critical requirement is claimed met while violated.
- **A4 No-go and underdetermined instances:** correct classification, the correct binding constraint or missing information, and a quantitative argument within tolerance of the authors' defensible argument.

An attractive objective value never compensates for a critical violation. A minor presentational defect never invalidates an otherwise correct engineering result.

### 4.2 Quality (among accepted submissions)

- **Task objective(s)** (mass, energy, mission performance, experiment cost), normalised RE-Bench-style **[proposal]**: the supplied baseline design scores 0 and the reference solution scores 1. Scores above 1 are possible.
- **Robustness:** margins across withheld conditions and perturbations.
- **Computational cost:** solver calls and CPU time.

### 4.3 Claim integrity and calibration

- **False-success rate:** how often the system claims success despite an important unresolved violation.
- **Unsupported claims:** claims whose evidence does not reproduce.
- **Calibration** **[proposal]:** per-requirement confidence from the evidence map compared with the verifier's outcome, via Brier score and a reliability diagram. This answers whether stated confidence matches whether claims survive independent tests.

### 4.4 Resource budgets and the primary result

- **Primary result:** a surface **P(verified completion | task family, difficulty, resource budget)**, where verified completion means the work package passes the family's acceptance process.
- **Budget axes:** inference expenditure (tokens or cost), solver calls, wall-clock time, allowed parallelism.
- **Anytime scoring [proposal]:** agents may submit checkpoints at any time. The verifier logs each checkpoint with a timestamp and resource counters, as RE-Bench's score log does. One long episode then yields a whole completion-versus-budget curve, which is much cheaper than separate runs per budget.
- **Parallelism:** best-of-k. Report both the agent-selected final submission and the oracle-selected upper bound.

### 4.5 Staged episodes (D4)

Each stage's work package is scored with the acceptance and quality rules above. In addition, **preservation** is scored: results unaffected by the change remain valid and unchanged, and all affected items are updated. This is checked via the evidence-map diff and the verifier.

### 4.6 Statistics

- The family is the top-level unit; instances are clustered within families and variants.
- Intervals come from bootstrapping over families and instances.
- The pilot mainly validates the instrument: verifier error rate, claim-integrity measurement, variance. It cannot rank close models.

## 5. Environments and verifiers

1. **Independence.** The grading physics is independent of the agent-visible model: higher fidelity, a different implementation (e.g. the agent gets a VLM-plus-beam model, the verifier runs OpenAeroStruct), and/or withheld operating conditions. Avoid reusing the code path that produced the reference solution.
2. **Trusted verifiers [proposal].** Every verifier ships with a test suite:
   - reference solutions that must pass;
   - known-bad solutions that must fail: plausible engineering mistakes, loophole exploits, hard-coded results;
   - a determinism check;
   - expert review of the physics where needed.

   Verifier versions are frozen with the family.
3. **Development check vs acceptance [proposal].** The agent sees nominal-condition checks; acceptance uses withheld conditions, like test or certification points. This also follows RE-Bench's lesson: agents that could see the test score overfitted noisy scorers.
4. **Sandbox:** editable and read-only files enforced; hidden material outside the container; no network; tamper checks; sampled transcript review for gaming.
5. **Compute:** fast solvers (seconds to minutes per call) so budgets measure engineering reasoning, not queueing; containerised, pinned versions, CPU-only target.
6. **Renewability:** procedural instance generation with hidden parameters, new withheld conditions and new change orders. Refresh on a regular cadence.

## 6. Task generation protocol: co-creation with an adversarial generator

**Roles (D22):**
- Claude proposes candidate families and variants across fields, and builds drafts.
- Guglielmo steers with experience, taste and engineering judgement, and decides what is included.
- Experts validate and enhance where needed.

**Why this is not circular (Guglielmo's argument, adopted).** Specifying a problem, solving it and verifying a solution are different capabilities. Generating a valid design space does not imply being able to navigate it. The real risks are that the generator cannot know beforehand whether a task exceeds its own capability, and that it may reproduce the same conceptual blind spots in task creation and evaluation. The protocol targets those two risks.

| Step | What happens |
|---|---|
| G1 Candidate brief (Claude) | Engineering context and why it is authentic; sources of difficulty; open tools; verifier concept; ladder sketch; feasibility classes; risks |
| G2 Selection (Guglielmo) | Steer, modify or reject |
| G3 Coherence and solvability | Physics checks (units, conservation, limit cases). Specification sufficiency: a competent engineer has what they need, with no artificial vagueness. A reference solution for feasible variants; a defensible proof for infeasible ones; the explicit missing-information set for underdetermined ones |
| G4 Verifier and test suite | §5.2 |
| G5 Cross-vendor red team [proposal] | A different vendor's model reviews the package for physics errors, ambiguities and shortcuts, to reduce shared blind spots |
| G6 Expert review (when needed) | Realism, verifier physics, estimated time for a competent engineer |
| G7 Fresh-instance trials | Clean sessions with no access to the generation context, repository history or verifier. At least 3 attempts per model on rung 0 and a top rung. Every failure is classified as a task defect, a harness defect or a genuine model failure |
| G8 Freeze | Version, provenance record, changelog |

**Measuring circularity [proposal]:** record the generator of each family (Claude, GPT, Guglielmo, expert). Report results by generator × solver. If cheap, seed some families with GPT as generator, so a Claude-generator advantage or disadvantage becomes measurable rather than assumed.

**Where task content lives [proposal]:** the GitHub repository is public. Candidate and final task content (instances, reference solutions, withheld conditions, verifier test data) must never be pushed there; anything public may enter future training data. See §11.

## 7. Difficulty calibration and human reference

- **Empirical:** pass rates across repeats and models, plus a ladder-monotonicity check. If a higher rung is easier than a lower one, investigate.
- **Expert time estimates:** collected during short consultations.
- **Human attempts:** RE-Bench paid its experts about US$1,855 per 8-hour attempt on average. Unpaid, we cannot do this at scale, so the options are in §11. Without human attempts, the paper must not claim human-relative performance.

## 8. RE-Bench: what we borrow and what we change

[RE-Bench](https://arxiv.org/abs/2411.15114) (METR, 2024): 7 ML research-engineering environments; 71 eight-hour attempts by 61 experts; scores normalised between the starting solution (0) and the reference solution (1); agents scored about 4× higher than humans with a 2-hour budget, while humans scored about 2× higher than the best agent with 32 hours.

**Borrow:**
- An environment = starting solution + scoring function + hidden reference solution.
- Normalised quality scores.
- A timestamped score log, giving anytime curves.
- Best-of-k and time-allocation analysis (many short attempts vs few long ones).
- An iteration-heavy creation process (they discarded more than 12 specifications and more than 5 implementations).
- Their documented agent failure modes:
  - stubborn incorrect assumptions;
  - not noticing contradictory information;
  - poor recovery from failures;
  - exploiting loopholes;
  - overfitting to visible noisy scores;
  - a widening human–AI gap with engineering complexity.

**Change:**
- Acceptance first (hard requirements pass/fail) instead of a pure optimisation score.
- Withheld test conditions.
- Claim integrity and calibration.
- Feasibility classes including no-go.
- Staged change orders.
- Costed investigations.
- No large paid human-baseline set, at least initially.

## 9. Implications for the pilot (option C, reinterpreted) [proposal]

- **Size.** Option C (D19) was sized for ~24 short tasks; a project-style family is one to two orders of magnitude more work. Proposed reinterpretation:
  - ~5–6 families across structures, aerodynamics/performance/design and propulsion;
  - at least one family outside fixed-wing and at least one explicitly cross-disciplinary;
  - each with a 4–5-rung ladder and several instances per rung, giving roughly 25–40 instances.
- **Pathfinder first.** Build one family end to end (environment, ladder, verifier and test suite, acceptance, fresh-instance trials) before committing to the others. RE-Bench's record of discarded designs suggests the first family will teach us the most.
- **Systems engineering** (Guglielmo's example 6) works as a cross-cutting dimension (D4 plus traceability) applicable to every family, rather than as a separate family. Guidance and control and spacecraft mission design sit in the expansion domains (flight dynamics and control come first after the pilot; spacecraft third) unless Guglielmo pulls one into the pilot.
- **Effort.** Very rough, to be calibrated by the pathfinder: ~60–120 h of combined work for the first family. If Claude does most of the building, Guglielmo's share is ~30–50 h for steering, physics review and decisions.
- **Run budget.** 6 families × 4 rungs × 3 repeats × 2 models ≈ 144 episodes of up to a few hours each. Subscription rate limits and wall-clock time may bind before cost does (Q15).

## 10. Risks specific to this design

| Risk | Mitigation |
|---|---|
| Build cost and schedule (one human at 10–15 h/week, plus Claude) | Pathfinder first; procedural instances; reuse of verifier infrastructure across families |
| Verifier error (wrong physics → wrong acceptance) | Independent implementations, verifier test suites, expert review |
| Environment defects mistaken for model failures (RE-Bench saw blocking technical issues in more than 5% of runs) | Rung 0 calibration; failure classification in G7 |
| Planted faults perceived as gotchas | Realism confirmation, discoverability rule, fault-free controls |
| Multi-hour runs hit rate limits or cost | Anytime scoring; few budgets; few repeats in the pilot |
| Contamination through the public repo | Private location for task content (§11) |
| Harness confound (vendor agents differ) | Decide in Q15; disclose; report per model–harness pair |

## 11. Open questions for Guglielmo

1. **Private task workspace.** The repo is public. Should task content go in a separate private repository (recommended), or should this repository become private until release?
2. **Git workflow.** Commit design docs directly to `main`, or use branches and pull requests?
3. **Pilot shape.** Confirm the reinterpretation of option C (§9): ~5–6 families with ladders, pathfinder first.
4. **Pilot scope.** Stay with the three pillars (plus sector variety and integration), with systems engineering as a cross-cutting dimension? Or pull guidance and control, or spacecraft mission design, into the pilot?
5. **Episode length.** Target agent wall-clock caps (e.g. 1 h, 4 h, 8 h), and the target competent-engineer time per rung (e.g. rung 0 ≈ 1–2 h, top rung ≈ 1–2 days).
6. **Feasibility mix.** For example ~70% feasible, ~15% infeasible, ~15% underdetermined, undisclosed per instance.
7. **Human reference.** None (model-calibrated plus expert time estimates), or occasional volunteer attempts later?
8. **Cross-vendor red team.** May ChatGPT review Claude-generated packages, and generate some families, so generator × solver effects can be measured?
9. **Q14–Q16 are now relevant.** Budget surfaces, inference accounting, checkpointed submissions and parallelism all depend on how models are run. Take Q15 (models and harness) next?
10. **Next step.** Should Claude come back with 2–3 candidate pathfinder family briefs (G1 format) for selection?
