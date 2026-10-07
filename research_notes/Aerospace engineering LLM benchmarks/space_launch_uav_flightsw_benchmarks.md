# LLM / LLM-agent benchmarks and evaluations for space, launch vehicles, UAVs and flight software (status as of 2026-10-06)

Scope note: this covers spacecraft/space systems, space operations, launch vehicles/rocket propulsion, drones/UAVs and flight/safety-critical software. Fixed-wing/rotorcraft aviation and generic engineering benchmarks are covered by other researchers and appear only as brief cross-references. Items before 2023 are marked [PRE-2023]. "Abstract-only" means I could read only the arXiv abstract page, not the full paper. Where a detail came only from a search-engine snippet and not a fetched page, it is marked [snippet-only].

---

## Q1. Spacecraft and space systems: design, systems engineering, astrodynamics, trajectory optimisation, GNC, LLMs as spacecraft operators

### Takeaway
There are only a handful of real space-engineering LLM benchmarks. Three stand out: **APBench**, a textbook-style astrodynamics benchmark with numeric answers (MIT, 2025); **GTOC-12 agent evaluation**, an agentic interplanetary trajectory design task graded by an expert rubric and an LLM judge (UPM/Auckland, 2026); and **AstroAgentBench**, agentic mission planning checked by an external physics verifier (Fudan, 2026). Everything else is a one-off case study, such as KSPDG agents, fine-tuned controllers, LLM-plus-MBSE architecture generation, OrbitTAMP and HELIOS. These case studies have no released benchmark or tiny item counts. Frontier models are knowledgeable but fail at execution. In the GTOC-12 study no model produced a valid submission, and errors with units and boundary conditions dominated. **No benchmark exists for spacecraft subsystem sizing (power, thermal, ADCS or propulsion budgets), CubeSat design or GNC design.**

### Cited Findings

#### APBench (Astrodynamics Problems Benchmark)
- **Who / when / where:** Di Wu, Raymond Zhang, Enrico M. Zucchelli, Yongchao Chen and Richard Linares (MIT Aeronautics & Astronautics). Published in *Scientific Reports* 15:7944, March 2025. DOI 10.1038/s41598-025-91150-5, PMCID PMC11885820 — [Europe PMC record](https://www.ebi.ac.uk/europepmc/webservices/rest/search?query=APBench%20astrodynamics&format=json&resultType=core)
- **Sector:** astrodynamics and space engineering. Topics are 2-body and 3-body problems, terminology, state-vector conversions, orbit determination, manoeuvre optimisation, rendezvous and docking, interplanetary planning, and perturbations — [Nature / search summary](https://www.nature.com/articles/s41598-025-91150-5)
- **Size and sourcing:** The open set has 299 questions:
  - Gordon, *Introduction to Aerospace Flight Vehicles*: 17
  - a PhD qualifying exam: 8
  - Braeunig, *Rocket & Space Technology: Orbital Mechanics*: 144
  - "Lynnane", *Introduction to Orbital Mechanics*: 130
  
  These are nested into APBench-α (25), β (169) and γ (299). A **private** split of 1,990 questions comes from 8 textbooks (Curtis, Vallado, Kluever, Schaub and others). It is withheld for copyright reasons — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- **Format and grading:** Answers are either "numeric" (a realistic number with units) or "message" (relaxed structure). There is deliberately **no multiple choice**. Grading is automatic, with a per-answer tolerance margin scaled to the answer's magnitude — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- **Scores on APBench-γ (late 2024 models):**
  - Closed models: o1-preview 66.6%, o1-mini 59.4%, Claude 3.5 Sonnet 57.5%, GPT-4o 54.8%, GPT-4 Turbo 49.2%
  - Open models: Qwen2.5-Math-72B 52.2%, Reflection-Llama-70B 32.4%, Llama 3.1 70B 32.1%, Llama 3.2 variants 8.4–15.4%, AstroLLaMA 1.0%
  - The authors use 60% as a "C-grade" pass mark. Only o1-preview cleared it on every sub-benchmark.
  - Source: [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- **Contamination and licence:** A canary string is embedded. The listed licence is CC BY-NC-ND 4.0, though it is unclear whether that covers the article or the data. I found no repository link in the text I could access — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- **Limitations stated by the authors:** The private split cannot be validated independently. Grading "message" answers is hard. Their prediction that models would reach "grade A" in 2025–26 is an extrapolation — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)

#### "Can LLMs Do Rocket Science?" — agentic GTOC-12 evaluation
- **Who / when / where:** Iñaki del Campo, Pablo Cuervo and Victor Rodriguez-Fernandez (Universidad Politécnica de Madrid), with Roberto Armellin and Jack Yarndley (University of Auckland). AIAA SciTech 2026; arXiv 2602.03630, February 2026 — [arXiv abs](https://arxiv.org/abs/2602.03630)
- **Task:** Solve the GTOC-12 problem, a large-scale asteroid-mining campaign with low-thrust trajectories. The agent uses an AIDE-based architecture adapted from the MLE-Bench framework. Each model produced 100 initial drafts, with up to 10 hours and 50 steps per attempt and staged debugging (5 attempts, then 20 for the top-20 drafts) — [arXiv HTML](https://arxiv.org/html/2602.03630)
- **Available tools:** PyKEP, PyGMO, TudatPy, poliastro, SpiceyPy, CasADi, Dymos, Skyfield, Astropy, NumPy/SciPy and JAX. All are open source — [arXiv HTML](https://arxiv.org/html/2602.03630)
- **Grading:** An expert-designed rubric of 26 binary criteria in 5 sections (objective understanding, asteroid filtering, low-thrust transfer estimation, mission architecture, multi-mission coupling). The judge is Gemini 2.5 Pro.
  - Intra-rater reliability: ICC(2,1)=0.748 and ICC(2,k)=0.937.
  - Cross-judge difference: Δ = −0.17.
  - There are no expert ground-truth annotations; the authors acknowledge this.
  - Source: [arXiv HTML](https://arxiv.org/html/2602.03630)
- **Scores (out of 26):** Claude Sonnet 4.5 21.15; Gemini 2.5 Pro 17.19; o3 15.33; GPT-5 13.54; Gemini 3 Pro 13.49; DeepSeek-R1 11.10; GPT-4-Turbo 9.30; GPT-4o 8.45; o1 5.46. **No model produced a GTOC submission that passed the validator** — [arXiv HTML](https://arxiv.org/html/2602.03630)
  - The abstract describes the scores as nearly doubling over two years, "from 9.3 to 17.2". That headline appears to predate the Claude Sonnet 4.5 result (21.15) in the HTML table — [arXiv abs](https://arxiv.org/abs/2602.03630)
- **Failure modes:** mixing units (km vs m, days vs seconds), boundary-condition errors, crashes from file formatting, "blind" debugging loops, and rarely printing or logging variables — [arXiv HTML](https://arxiv.org/html/2602.03630)
- **Release:** github.com/inaki11/GTOC-Agent-Bench and github.com/inaki11/aide-gtoc — [arXiv HTML](https://arxiv.org/html/2602.03630)

#### AstroMind — spacecraft behaviour reasoning (space domain awareness)
- **Who / when:** Hao Liu, Qinglei Hu and Dongyu Li (Beihang) with Siyuan Yang (KTH). arXiv 2605.24573, 23 May 2026 — [arXiv HTML](https://arxiv.org/html/2605.24573)
- **Size:** 133 scenarios and 399 questions: 66 intent-inference, 241 parameter-estimation and 92 threat-assessment. They span 8 categories and 29 subcategories, including orbital manoeuvres, on-orbit interactions, non-cooperative activities, collision/fragmentation, attitude, deployments, mission phases and anomalies — [arXiv HTML](https://arxiv.org/html/2605.24573)
- **Generation:** Scenarios come from poliastro v0.17 propagation (Cowell's method with Runge-Kutta, J2, exponential drag and impulsive burns). Ground-based azimuth/elevation measurement series are synthesised with injected noise of 1–10 arcsec and 1–10 m — [arXiv HTML](https://arxiv.org/html/2605.24573)
- **Grading:**
  - classification accuracy for intent and threat questions
  - relative error on the scalars that could be parsed
  - a 5-point reasoning-quality rubric scored by a DeepSeek-R1 judge
  - a three-phase "HSNE" protocol
  - Source: [arXiv HTML](https://arxiv.org/html/2605.24573)
- **Scores:** Only **open-weight models of 4B–32B** were tested. Closed frontier models were excluded because "data sovereignty policy bars cloud access". Intent accuracy: Qwen3-32B 67.74%, GPT-OSS-20B 64.62%, Gemma-3-27B 56.92%, QwQ-32B 56.25% — [arXiv HTML](https://arxiv.org/html/2605.24573)
- **Limitations:** no human-expert baseline, a single judge model, small categories, no release link, and no real sensor noise or adversarial deception — [arXiv HTML](https://arxiv.org/html/2605.24573)

#### KSPDG (Kerbal Space Program Differential Games) and the "LLMs as spacecraft operators" line of work
- **Environment:** KSPDG is from MIT Lincoln Laboratory (Allen, Rachlin, Ruprecht, Loughran, Varey, Viggh). It is cited as "SpaceGym", IEEE Aerospace Conference 2023.
  - Scenarios: pursuit-evasion (PE1), lady-bandit-guard (LBG1) and sun-blocking (SB1).
  - Interfaces: Gymnasium and PettingZoo APIs. Game time does not pause while the agent decides.
  - **Proprietary dependency:** the KSP v1 game plus the Making History expansion, the kRPC mod and PhysicsRangeExtender.
  - Licence: MIT.
  - Source: [GitHub mit-ll/spacegym-kspdg](https://github.com/mit-ll/spacegym-kspdg)
- **"Language Models are Spacecraft Operators"** (Rodriguez-Fernandez, Carrasco, Cheng, Scharf, Siew, Linares; arXiv 2404.00413, March 2024). Telemetry is converted to text prompts and the LLM emits control commands over RPC. This pure-LLM agent **ranked 2nd in the KSPDG challenge** — [arXiv abs](https://arxiv.org/abs/2404.00413)
- **Follow-up papers:**
  - Fine-tuning GPT-3.5 and LLaMA on KSPDG (ESA SPAICE 2024; arXiv 2408.08676) — [arXiv abs](https://arxiv.org/abs/2408.08676)
  - A journal version for *Advances in Space Research* (arXiv 2505.19896), which releases models on Hugging Face (huggingface.co/OhhTuRnz) and code at github.com/ARCLab-MIT/kspdg — [arXiv abs](https://arxiv.org/abs/2505.19896)
  - GUIDE (CVPR 2026 AI4Space; arXiv 2603.27306), which evolves a natural-language "playbook" of decision rules across episodes in an adversarial interception scenario. The abstract gives no numeric scores — [arXiv abs](https://arxiv.org/abs/2603.27306)
- **LLMSat** (David Maranto, B.A.Sc. thesis; arXiv 2405.01392, 2024): an LLM is the goal-oriented spacecraft controller in deep-space KSP scenarios. The key finding is that LLM reasoning and planning "do not scale well" as mission complexity grows — [arXiv abs](https://arxiv.org/abs/2405.01392)

#### Fine-tuned language models as space-systems controllers
- Zucchelli, Wu, Briden, Hofmann, Rodriguez-Fernandez and Linares. AAS/AIAA Astrodynamics Specialist Conference, August 2024 (AAS 24-445); arXiv 2501.16588 — [arXiv abs](https://arxiv.org/abs/2501.16588)
- 7–13B models were fine-tuned on four tasks: a 3D spring toy problem, low-thrust orbit transfer, low-thrust cislunar control and **powered-descent guidance**. They produced vector outputs with up to 10 significant digits and generalised beyond the training data. No code or data was released — [arXiv abs](https://arxiv.org/abs/2501.16588)

#### OrbitTAMP — LLM task-and-motion planning for rendezvous (abstract-only)
- Takubo, Gammelli, Pavone and D'Amico; arXiv 2610.01093, 1 October 2026.
- It turns natural-language commands into dynamically feasible rendezvous trajectories.
- Frontier LLMs recover partial mission specifications exactly 98% of the time.
- A 9B model scores 75%, rising to 88% with verifier-guided revision.
- Frontier model names, split sizes and a release link were not in the abstract.
- Source: [arXiv abs](https://arxiv.org/abs/2610.01093)

#### HELIOS — LLM agent for indirect low-thrust trajectory optimisation (abstract-only)
- An-yi Huang; arXiv 2607.24051, July 2026.
- The LLM derives the Pontryagin optimality conditions and transversality conditions, generates code and solves the shooting problem.
- It was tested on 11 scenarios, from rendezvous to multi-leg, gravity-assist and solar-sail cases. The best configuration compiled 11/11, and several scenarios matched reference optima, including a 48-variable multiple-shooting problem.
- Eight open-source LLM backends were compared, with total scores of 250–905. Score correlated positively with model scale.
- Source: [arXiv abs](https://arxiv.org/abs/2607.24051); [search summary](https://arxiv.org/pdf/2607.24051)

#### LLM + MBSE spacecraft architecture generation
- Timperley, Berthoud, Snider and Tryfonas (University of Bristol). *Journal of Engineering Design* 36(4), 2025. DOI 10.1080/09544828.2025.2453401 — [Crossref](https://api.crossref.org/works/10.1080/09544828.2025.2453401)
- A Python tool couples the **Capella** MBSE tool (open source) to an LLM. It was tested on three tasks: an ESA Earth-observation mission, a CubeSat payload and an EO spacecraft. Outputs were compared with those of human designers, and the generated modes and components were rated good quality — [search summary via NASA/T&F results](https://doi.org/10.1080/09544828.2025.2453401) [snippet-only; the full paper was paywalled (403), and the LLM versions are unverified]

#### Space-engineering QA datasets
- **SpaceQA** [PRE-2023] (García-Silva et al., expert.ai with ESA ESOC/ESTEC; arXiv 2210.03422, 2022): an open-domain QA system for mission design using a dense retriever and a neural reader, evaluated on an ESA-produced test set — [arXiv](https://arxiv.org/pdf/2210.03422)
  - Belo, Guimarães and Soares (NOVA LINCS/Neuraspace, arXiv 2605.27444, May 2026) reuse **60 expert-written ESA SpaceQA triplets** to evaluate RAG over space-debris-mitigation documents and ESA CDF reports. Judges are Llama 3.3 70B and Qwen 2.5 72B. Llama 3 8B scores 56/60 with retrieved context versus 3/60 without, i.e. RAG raises accuracy from 5% to 93% — [arXiv HTML](https://arxiv.org/html/2605.27444)
- **Astro-MCQ** (Patrick Fleith, Hugging Face, 2025): 193 expert-written MCQs drawn from master's-level courses in mission design and operations, each with 3 permutations (about 579 instances).
  - Topics: orbital mechanics, propulsion, mission operations and design, human spaceflight, space environment, spacecraft subsystems.
  - Licence CC-BY-4.0. No model results are reported. It is billed as "the first dataset in the upcoming AstroBench collection".
  - Source: [HF README](https://huggingface.co/datasets/patrickfleith/astro-mcq/blob/main/README.md)
  - The same author keeps a collection of space-engineering LLM datasets — [HF collection](https://huggingface.co/collections/patrickfleith/llm-datasets-for-ai-and-space-engineers) — and an awesome-list — [GitHub](https://github.com/patrickfleith/awesome-spacecraft-engineering-datasets)
- **SwRI "Aerospace Engineering Evaluation Set"** (Brian J. Connolly, Southwest Research Institute; AIAA SciTech 2025, DOI 10.2514/6.2025-0702) — [Crossref](https://api.crossref.org/works/10.2514/6.2025-0702)
  - Described as a multimodal set of aerospace test questions and tasks, including true/false and short-response — [AIAA listing, snippet-only](https://arc.aiaa.org/doi/10.2514/6.2025-0702). The abstract and the AIAA page were inaccessible (403).
  - The precursor internal R&D project (May–Sept 2023, centred on GPT-4) found LLMs "struggled with … math (if not given access to other tools) … rigorous physical reasoning" — [SwRI IR&D page](https://www.swri.org/what-we-do/internal-research-development/2023/energy-environment/evaluating-ai-large-language-models-mechanical-aerospace-engineering-applications-18-r6365)

#### Astronomy and space-science benchmarks (science, not engineering — cross-reference only)
- **AstroMLab 1:** 4,425 MCQs generated from 885 *Annual Review of Astronomy & Astrophysics* articles. Claude 3.5 Sonnet scores 85.0%, LLaMA-3-70B 80.6% and Qwen-2-72B 77.7% (2024) — [ORNL record](https://impact.ornl.gov/en/publications/astromlab-1-who-wins-astronomy-jeopardy/); [AstroMLab 2](https://arxiv.org/pdf/2409.19750); [AstroMLab 3](https://arxiv.org/pdf/2411.09012)
- **INDUS** (IBM, NASA and others; EMNLP 2024 Industry): science encoders plus the NASA-QA (extractive QA) and NASA-IR benchmarks — [arXiv 2405.10725](https://arxiv.org/abs/2405.10725v3)
- AstroLLaMA, an astronomy fine-tune, scored only **1.0%** on APBench. Domain science pretraining does not transfer to engineering calculation — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)

#### Non-LLM resources that could be repurposed
- **GTOC** (ESA-initiated in 2005) supplies problem statements and validators — see the GTOC-12 paper above — [arXiv abs](https://arxiv.org/abs/2602.03630)
- **ESA ACT SpOC** (with GECCO): 2022 TRAPPIST-1, 2023 "Settling New Mars", 2024 "Orbital Megastructures" — [ESA SpOC 2023](https://www.esa.int/gsp/ACT/projects/spoc-2023); [ESA SpOC 2024](https://www.esa.int/gsp/ACT/news/spoc-2024)
- **Kelvins** competition platform — [ESA](https://kelvins.esa.int/)
- **European Satellite Benchmark for Control Education and Industrial Training** (Sanfedino et al.; arXiv 2406.12603): a high-pointing AOCS/GNC benchmark mission. Its software stack was not stated in the abstract; it may be MATLAB/Simulink, which is unverified — [arXiv abs](https://arxiv.org/abs/2406.12603)

### Inferences
- The field has moved from knowledge QA (2024: AstroMLab, APBench) to **agentic, tool-using, verifier-graded** tasks (2026: GTOC-12 agent, AstroAgentBench). On astrodynamics problems with a single numeric answer, early-2025 reasoning models were already near the 60–67% range. By late 2026 that format is probably close to saturation for frontier models (no newer APBench numbers were found). Agentic design tasks still show large headroom: no valid GTOC submission, and a top rubric score of 21/26.
- The recurring bottleneck is execution. Unit consistency, boundary conditions and debugging hygiene fail far more often than conceptual knowledge. AerospaceBench should grade **executable artefacts with physics validators**, not explanations.
- One group of related labs (MIT ARCLab/Linares, UPM/Rodriguez-Fernandez) wrote much of this literature: APBench, KSPDG agents, the fine-tuned controllers and the GTOC-12 study. AerospaceBench could reuse their artefacts or collaborate with them.

### Gaps
- **No benchmark found for spacecraft subsystem sizing or budgets** (power, thermal, ADCS, propulsion, link, mass/ΔV budgets), CubeSat design, or GNC algorithm design or tuning. Coverage is limited to knowledge MCQs (Astro-MCQ) and textbook problems (APBench).
- No LLM evaluation using GMAT, Orekit, Basilisk, STK or FreeFlyer was found. Open-source tools appear only as agent toolkits: poliastro, PyKEP, TudatPy and SGP4.
- No NASA- or ESA-published systematic LLM benchmark for spacecraft engineering was found. ESA's work is RAG/QA (SpaceQA) and operations assistants. NASA's is assurance-argument analysis (see Q5).
- I could not obtain the abstract or contents of SwRI AIAA 2025-0702, the LLM versions in the Bristol MBSE study, or the Strathclyde paper "Connecting space system requirements and design models with LLMs" ([title only](https://pureportal.strath.ac.uk/en/publications/connecting-space-system-requirements-and-design-models-with-large/)).
- I could not find the APBench public repository URL.

---

## Q2. Space operations: telemetry anomaly diagnosis, operations procedures and assistants, ground segment, mission planning

### Takeaway
LLM evaluations in space operations are early-stage. Four exist:
- **ATSADBench**: LLMs on aerospace telemetry time series; near-random on multivariate data
- **SpaceHMchat**: power-system health management with a hardware-in-the-loop fault dataset
- **ESA OCAI**: an operational assistant with no public benchmark
- **AstroAgentBench**: the strongest agentic benchmark; satellite scheduling, constellation and relay planning, graded by an SGP4-based verifier, with GPT-5.4, Claude Opus 4.6, Kimi K2.6 and DeepSeek V4 scored

The real-telemetry datasets **ESA-ADB**, **OPS-SAT-AD** and **NASA SMAP/MSL** are mature non-LLM benchmarks that could be reframed into agentic diagnosis tasks.

### Cited Findings

#### AstroAgentBench (v1 title: "AstroReason-Bench")
- **Who / when:** Weiyi Wang, Xinchi Chen, Jingjing Gong, Xuanjing Huang and Xipeng Qiu (Fudan University, Shanghai Innovation Institute, OpenMOSS). arXiv 2601.11354; v3 is dated 2 October 2026 — [arXiv HTML](https://arxiv.org/html/2601.11354); [v1 title](https://arxiv.org/pdf/2601.11354)
- **Seven task families:** agile EO scheduling (AEOSSP), SatNet ground-station contact assignment, SPOT-5 photo selection, stereo imaging, regional coverage, revisit-constellation design and scheduling, and relay-constellation design. There are 116 cases in total, of which 35 are held-out test cases (5 per family) — [arXiv HTML](https://arxiv.org/html/2601.11354)
- **Verifier:**
  - The submission must match the schema and arrive before timeout.
  - Hard constraints are then checked: SGP4/TEME propagation from TLEs, J2 for constellation design, GCRF→ITRF frames, the WGS84 ellipsoid, elevation and off-nadir geometry, slew and settling models, battery state of charge, and antenna occupancy and link routing.
  - Score is mission value.
  - Source: [arXiv HTML](https://arxiv.org/html/2601.11354)
- **Agent harnesses and results** (2-hour wall clock, 8 CPU cores):

  | Harness + model | Valid submissions (of 35) | Notable family scores |
  |---|---|---|
  | Codex CLI + GPT-5.4 | 35 | AEOSSP 72.91, Relay 64.91, SatNet 69.45, Stereo 68.91 |
  | Kimi CLI + Kimi K2.6 | 35 | Regional 73.15, Revisit 73.36, Stereo 0.25 |
  | OpenCode + DeepSeek V4 Pro | 34 | — |
  | Claude Code + Claude Opus 4.6 | 26 | AEOSSP 58.45, Regional 21.22, Stereo 19.16 |
  | OpenCode + MiniMax M2.7 | 23 | — |

  Source: [arXiv HTML](https://arxiv.org/html/2601.11354)
- **Comparison with task-specific solvers:**
  - The best agents beat the solver baselines on Relay, Revisit, SatNet and SPOT-5.
  - They trail on geometry-heavy tasks: Regional 73.15 vs 86.44, and Stereo 68.91 vs 96.05.
  - Solvers used include MWIS, LNS, MILP and constraint programming.
  - Source: [arXiv HTML](https://arxiv.org/html/2601.11354)
- **Process failures:**
  - 12.6% of submissions were invalid.
  - 9.1% were valid but scored zero.
  - In 22.3% of runs the agent committed to an artefact before reading the workspace contract.
  - Source: [arXiv HTML](https://arxiv.org/html/2601.11354)
- **Release:** github.com/Mtrya/AstroAgentBench and HF dataset kaupane/AstroAgentBench, with run traces on Zenodo (DOI 10.5281/zenodo.23084446). Code is MIT; instances are CC BY 4.0. Limitations: 5 held-out cases per family, solver baselines that are not certified optima, and a single difficulty level — [arXiv HTML](https://arxiv.org/html/2601.11354); [GitHub](https://github.com/Mtrya/AstroAgentBench)

#### EOS-Bench (non-LLM, could be repurposed)
- Yin et al. (25+ authors, multiple institutions); arXiv 2604.25782, April 2026.
- 1,390 scenarios and 13,900 instances, up to 1,000 satellites and 10,000 requests.
- Evaluates MIP, heuristics, metaheuristics and deep RL only, with **no LLMs**.
- CC BY 4.0, github.com/Ethan19YQ/EOS-Bench.
- Source: [arXiv abs](https://arxiv.org/abs/2604.25782)

#### ATSADBench — LLMs for time-series anomaly detection in aerospace software (abstract-only)
- Yang Liu, Yixing Luo, Xiaofeng Li, Xiaogang Dong, Bin Gu and Zhi Jin. Accepted at ASE 2025; arXiv 2601.12448.
- 9 tasks and 108,000 data points, covering three anomaly patterns, univariate and multivariate signals, and in-loop vs out-of-loop feedback.
- New window-level metrics: Alarm Accuracy, Alarm Latency and Alarm Contiguity.
- Findings:
  - LLMs do well on univariate tasks, but **multivariate performance approaches random guessing**.
  - Few-shot prompting gives modest gains.
  - **RAG gives no significant benefit**.
  - The abstract does not name the models.
- Source: [arXiv abs](https://arxiv.org/abs/2601.12448)

#### SpaceHMchat — spacecraft power-system health management
- Di, Zhao, Wang, Liu, Tang, Ren, Zhai and Chen; arXiv 2601.12667, January/March 2026.
- Releases "the first-ever" all-in-loop health-management dataset for a spacecraft power system: 4 sub-task types, 17 fault types and more than 700,000 timestamps, from a hardware-realistic fault-injection platform. The simulation model is open-sourced.
- Reported results:
  - 100% logical-reasoning accuracy on work-condition recognition
  - more than 99% success invoking the anomaly-detection tool
  - more than 90% fault-localisation precision
- Source: [arXiv abs](https://arxiv.org/abs/2601.12667)

#### ESA Operations CompAnIon (OCAI)
- An LLM application that helps flight control teams investigate anomaly root causes. It retrieves and correlates TM/TC archives, procedures, anomaly reports, manuals and logs — [ResearchGate](https://www.researchgate.net/publication/392587215_Bridging_ESA_Innovations_with_Commercial_Space_Operations_Integration_of_an_Ops_Companion_for_Satellite_Decision_Support); [ESOC A2I Roadmap](https://esoc.esa.int/index.php/a2i-roadmap-esas-missions-operations)
- **Conflicting deployment claims** [snippet-only]: one source says it was operationally validated for 14 missions, another says it was operationalised on two ESA missions. I found no public benchmark or quantitative evaluation.

#### Real-telemetry anomaly benchmarks (non-LLM, could be repurposed)
- **ESA-ADB**:
  - Kotowski, Haskamp, Andrzejewski, Ruszczak, Nalepa, Lakey, Collins, Kolmas, Bartesaghi, Martínez-Heras and De Canio (KP Labs, Airbus DS, ESOC and others); arXiv 2406.17826 (v2 Aug 2025) — [arXiv abs](https://arxiv.org/abs/2406.17826)
  - Telemetry from 3 large ESA missions, annotated over more than a year by spacecraft operations engineers working with ML engineers. Events fall into four categories: anomalies, rare nominal events, communication gaps and invalid segments.
  - Data on Zenodo 12528696; code at github.com/kplabs-pl/ESA-ADB; a Kaggle challenge.
  - Source: [SpaceOps-2025 paper #152](https://publications.spaceops.org/2025/download_by_id.php?id=0152)
- **OPS-SAT-AD** (Ruszczak, Kotowski, Evans, Nalepa; *Scientific Data*, 29 April 2025): 2,123 single-channel telemetry fragments from 9 channels, about 20% of them anomalous, with 18 handcrafted features and 30 ML baselines — [PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12041257/); [arXiv 2407.04730](https://arxiv.org/html/2407.04730v1)
- **NASA SMAP/MSL** [PRE-2023] (Hundman et al., 2018): expert-labelled anomalies from SMAP and Curiosity telemetry — [arXiv 1802.04431](https://arxiv.org/pdf/1802.04431); [Kaggle mirror](https://www.kaggle.com/datasets/patrickfleith/nasa-anomaly-detection-dataset-smap-msl)
- Other datasets listed by the awesome-list: CATS (5M points, 200 injected anomalies) and LASP telemetry — [GitHub awesome list](https://github.com/patrickfleith/awesome-spacecraft-engineering-datasets)

#### Ops-adjacent cross-reference
- An LLM-agent EO benchmark of 140 yes/no questions from NASA Earth Observatory articles (arXiv 2504.12110). This is science/EO, not operations engineering — [arXiv](https://arxiv.org/abs/2504.12110) [snippet-only]

### Inferences
- AstroAgentBench is the closest existing analogue to what AerospaceBench needs: executable artefacts, a physics verifier, solver baselines and named frontier agent harnesses. Its results show that **harness and model choice matter a lot**. Claude Code + Opus 4.6 produced only 26/35 valid submissions, while Codex + GPT-5.4 produced 35/35. Validity rate is therefore a key metric.
- LLMs are weak at raw multivariate telemetry (ATSADBench). Operations tasks should therefore be **tool-using**: the agent calls detectors, plots and queries archives. They should not ask the model to read raw numbers in context. ESA-ADB and OPS-SAT-AD supply realistic data with ground-truth anomaly labels that operators curated.
- Operations-assistant claims (OCAI, SpaceHMchat) are self-reported with no independent benchmark. A held-out anomaly-investigation task set with ground-truth root causes would fill a clear gap.

### Gaps
- No benchmark found for **writing or validating operations procedures**, generating telecommand sequences, FDIR design, conjunction-assessment and collision-avoidance decisions (AstroMind threat assessment only partly covers this), or ground-segment configuration.
- ATSADBench model names and scores were not visible in the abstract.
- I could not quantitatively verify OCAI's evaluation.

---

## Q3. Launch vehicles and rocket propulsion

### Takeaway
LLM evaluation for launch vehicles has reached only **amateur and high-power rocketry** and a **single-case liquid-engine design agent**:
- **RocketBench** (RocketPy, 2025): Claude 3.7 and o1 fall below a human expert. A 7B model trained with RL beats the human.
- **RocketSmith** (OpenRocket + CAD + 3D printing): four real rockets flown in 2026.
- **RocketAgent** (Oct 2026): a NASA-CEA-based thrust-chamber design agent demonstrated on one 500 N LOX/RP-1 case, with no model comparison.

There is **no benchmark for orbital launch vehicle sizing, staging, ascent trajectory optimisation, engine cycle analysis, recovery or reuse, or propulsion test-data analysis**.

### Cited Findings

#### RocketBench ("LLMs for Engineering: Teaching models to design High Powered Rockets")
- Toby Simonds (Tufa Labs); arXiv 2504.19394, April 2025 — [arXiv HTML](https://arxiv.org/html/2504.19394v2)
- **Tasks:**
  - Target altitude, modelled on the Spaceport America Cup 10,000 ft category. Score weights: 50% altitude accuracy, 10% structural integrity, 10% horizontal drift, 15% cost and 15% landing safety.
  - Precision landing. Score weights: 75% landing accuracy, 15% structure, 5% cost and 5% safety.
  - The LLM chooses the motor (from 7 commercial options), body, nose cone, fins, parachutes, launch rail settings and payload as JSON. **RocketPy 6-DOF** simulates the result (open source). Models get up to 30 refinement iterations.
  - Source: [arXiv HTML](https://arxiv.org/html/2504.19394v2)
- **Scores:**
  - Target altitude (average): Claude 3.7 62.14 (peak 74.79), o1 60.57, DeepSeek-V3 57.36
  - Precision landing (average): o1 31.21, DeepSeek-V3 29.29, Claude 3.7 28.54, GPT-4o 15.29
  - Human expert (peak of 10 attempts): 76.57 and 91.60
  - **Qwen2.5-7B trained with GRPO RL** reached 79.98 and 95.6, landing within about 12 m, after about 3,000 simulations
  - Source: [arXiv HTML](https://arxiv.org/html/2504.19394v2)
- **Limitations:** a single human expert, high variance, and no manufacturing realism. **Not released** (only a sample prompt is published) — [arXiv HTML](https://arxiv.org/html/2504.19394v2)

#### RocketSmith — agentic design-for-additive-manufacturing of high-power rockets
- Pak, Barkley, Loghmani, Baich, Pamal and Barati Farimani (CMU, Tripoli Rocketry Association); arXiv 2606.00097, v3 June 2026 — [arXiv HTML](https://arxiv.org/html/2606.00097v3)
- **Setup:** a Claude Code plugin with subagents and skills that orchestrates **OpenRocket** (stability and flight simulation), **build123d** (parametric CAD) and **PrusaSlicer** — [arXiv HTML](https://arxiv.org/html/2606.00097v3)
- **Evaluation:** **four Class-2 rockets were printed and flown** on 3 May 2026. Measured apogee was about 80–84% of prediction. Two were recovered reflyable; one failed structurally at ejection and one had a recovery deployment problem. No benchmark was released. Code is at github.com/ppak10/RocketSmith — [arXiv HTML](https://arxiv.org/html/2606.00097v3)

#### RocketAgent — long-horizon agent for multidisciplinary design of liquid-rocket thrust chambers
- He, Mao and Chen (Peking University) with He, Zhang, Zheng and Xiao (Legendspace); arXiv 2610.06044, 5 October 2026 — [arXiv HTML](https://arxiv.org/html/2610.06044)
- **Workflow:**
  - performance sizing with **RocketCEA/NASA CEA** and RPA
  - regenerative-cooling optimisation with a genetic algorithm
  - injector design with a surrogate model
  - voxel geometry with PicoGK
  - multiphysics checks with DeepFlame (OpenFOAM) and **ANSYS Fluent and ABAQUS (both proprietary)**
  - Source: [arXiv HTML](https://arxiv.org/html/2610.06044)
- **Evaluation:** a **single** 500 N LOX/RP-1 chamber at 1.0 MPa. Thrust, Isp and chamber pressure had to be within ±10% of reference. Hard constraints applied to wall temperature, pressure loss and safety factors. No LLM was named, there was no model comparison and no human baseline, and **no public repository** — [arXiv HTML](https://arxiv.org/html/2610.06044)

#### Related items
- APBench's open set includes 144 items from Braeunig's *Rocket & Space Technology*, which covers orbital mechanics — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- Powered-descent guidance appears among the fine-tuned-controller tasks — [arXiv 2501.16588](https://arxiv.org/abs/2501.16588)
- A non-LLM quality-diversity optimisation of model rockets (pyribs plus OpenRocket via orhelper) could serve as a solver baseline — [arXiv 2504.02177](https://arxiv.org/pdf/2504.02177)
- RocketPy itself: open-source 6-DOF with variable mass, parachutes, multi-stage and dispersion analysis — [SciPy proceedings](https://proceedings.scipy.org/articles/majora-212e5952-020)
- RocketCEA wraps NASA CEA — [docs](https://rocketcea.readthedocs.io/en/latest/)

### Inferences
- RocketBench shows that simulation-in-the-loop design tasks have **steep difficulty gradients**. Precision landing averaged roughly 15–31 points for frontier models against a human peak of about 92. A modest RL loop beat both. This makes it a good template, but scores may move fast once labs train on similar environments.
- The open-tool stack for launch-vehicle tasks is ready: RocketPy, OpenRocket, RocketCEA/CEA, CoolProp, and for the trajectory side, as used in the GTOC agent work, Dymos, CasADi and Tudat. A benchmark with no proprietary dependencies is feasible. The exception is CFD and FEA at RocketAgent's level of fidelity.

### Gaps
- No LLM evaluation found for:
  - orbital launch-vehicle conceptual sizing and staging
  - ascent trajectory or max-Q and load analysis
  - solid or hybrid motor grain design
  - engine cycle and turbopump design
  - propellant feed systems
  - hot-fire or static-test data analysis
  - recovery and reuse (other than amateur parachutes)
  - range safety and FAA Part 450 licensing
- No frontier-model scores exist for any liquid-engine design task.

---

## Q4. Drones/UAVs: (a) embodied navigation and control benchmarks vs (b) engineering design and analysis, and LLMs with PX4/ArduPilot/Gazebo/AirSim SITL

### Takeaway
UAV LLM work splits into two groups.

**(a) Embodied VLN and VLA navigation benchmarks:** AerialVLN, CityNav, OpenUAV/TravelUAV, OpenFly, UAV-Flow and AeroVerse. These are large robotics datasets where success is measured by reaching a goal in a photorealistic simulator. Machine success is far below expert-pilot success.

**(b) LLM-driven SITL and command benchmarks:**
- DroneServer/MCP across ArduPilot and PX4, with 11 model families
- PX4/ROS2 natural-language control
- AutoSimTest (test generation)
- RisConFix (ArduPilot configuration repair)
- UAVBench, with 50k LLM-generated MCQs

**Engineering design and analysis of UAVs** (airframe sizing, propulsion matching, endurance estimation) has **no benchmark**; only one case study and hobby projects exist.

### Cited Findings

#### (a) Embodied navigation (robotics-style)
- **AerialVLN** [2023] (Liu et al., ICCV 2023; arXiv 2308.06735) — [CVF](https://openaccess.thecvf.com/content/ICCV2023/html/Liu_AerialVLN_Vision-and-Language_Navigation_for_UAVs_ICCV_2023_paper.html):
  - 25 city-level environments in a near-realistic simulator
  - instructions collected from 2,444 annotators over 5,074.51 hours
  - test-unseen success rate: Seq2Seq 1.0%, CMA 1.6%, **AOPA-licensed human pilots 80.8%**
  - Source: [search summary of the paper](https://arxiv.org/abs/2308.06735v1)
- **CityNav** (arXiv 2406.14240): aerial VLN over **real-world** city scenes, as opposed to synthetic ones — [arXiv](https://arxiv.org/pdf/2406.14240)
- **OpenUAV / TravelUAV / UAV-Need-Help** (ICLR 2025; arXiv 2410.07087): a realistic 6-DoF continuous-flight platform with about 12k target-oriented trajectories, an assistant-guided object-search benchmark and a multimodal "UAV navigation LLM". A considerable gap to human operators remains — [arXiv](https://arxiv.org/pdf/2410.07087)
- **OpenFly** (arXiv 2502.18041, 2025): a toolchain plus 100k trajectories across 18 scenes, the largest aerial VLN benchmark — [arXiv HTML](https://arxiv.org/html/2502.18041v6)
- **UAV-Flow Colosseo** (Wang et al.; NeurIPS 2025 Datasets & Benchmarks; arXiv 2505.15725): "Flying-on-a-Word" fine-grained language-conditioned control learned by imitating real pilots. It has a simulation suite and a real-world VLA deployment. VLA models beat VLN baselines — [arXiv HTML](https://arxiv.org/html/2505.15725v2); [NeurIPS](https://papers.neurips.cc/paper_files/paper/2025/hash/92cfa104db7fdd053492df3589ddfd49-Abstract-Datasets_and_Benchmarks_Track.html)
- **AeroVerse** (arXiv 2408.15511): a UAV-agent benchmark suite for "aerospace embodied world models" — [arXiv](https://arxiv.org/pdf/2408.15511) [title-level only]
- **DroneCATS-Agent** (Park et al.; arXiv 2609.01404, Sept 2026): multimodal LLMs as generalist VLA agents for approaching, tracking, searching and fleet commanding. "Small open models often navigate into the success radius more reliably than frontier models" but then declare arrival wrongly — [arXiv abs](https://arxiv.org/abs/2609.01404)
- **Survey** (Xia et al.; arXiv 2604.07705, April 2026): evaluation infrastructure has gaps in scale, environmental diversity, real-world grounding and metric coverage — [arXiv abs](https://arxiv.org/abs/2604.07705)

#### (b) LLMs with autopilots, SITL and command interfaces
- **DroneServer — an LLM-agnostic MAVLink/MCP harness** (Ramos Silva & Burke, UC Irvine; arXiv 2601.15486; v1 January 2026, v3 September 2026) — [arXiv HTML](https://arxiv.org/html/2601.15486v3); [versions](https://arxiv.org/abs/2601.15486):
  - **Missions:** 10 standard missions in **ArduPilot and PX4 SITL**. They include takeoff and land, waypoints, a survey, return-to-launch, parameter configuration, a geofence-violation attempt, a prompt-injection attack and a mission longer than 10 minutes.
  - **Grading:** automatic, from telemetry.
  - **Results:**
    - Eight of 11 model families completed 90–100% of the six flying missions. The core campaign included Claude 3.5 Sonnet, GPT-4o, GPT-4 Turbo, Gemma 2 9B and Llama 3.1 70B.
    - In 110 geofence trials no aircraft left the permitted zone.
    - The adversarial cases were handled 29/29.
  - **Release:** CC BY 4.0, github.com/PeterJBurke/droneserver, Zenodo DOI 10.5281/zenodo.22310050.
  - **Limitations:** the adversarial suite was self-generated, and hallucination remains a residual risk.
- **PX4 natural-language control** (arXiv 2506.07509): PX4 with ROS 2 and locally hosted LLM+VLM pairs. Four model families were tested in simulation and on a custom quadcopter. The best mission success was **40%** (Gemma3) — [arXiv abs](https://arxiv.org/abs/2506.07509)
- **AutoSimTest** (Duvvuru, Zhang, Vierhauser, Agrawal; ICSE 2025; arXiv 2501.11864): LLM agents generate test scenarios, environments and missions for small UAS. Tested against PX4 and ArduPilot through AirSim and ArduPilot SITL. The evaluation is qualitative and efficiency-based — [arXiv abs](https://arxiv.org/abs/2501.11864)
- **RisConFix** (Han, Nie, Yu, Hu, Yue; arXiv 2512.07122, December 2025): LLM repair of risky **ArduPilot** parameter configurations. On 1,421 misconfiguration groups it reached a best repair success of 97% with 1.17 repairs on average — [arXiv abs](https://arxiv.org/abs/2512.07122)
- **UAVBench** (Ferrag, Lakas [UAE University], Debbah [Khalifa University]; arXiv 2511.11252, November 2025) — [arXiv HTML](https://arxiv.org/html/2511.11252v1); [GitHub](https://github.com/maferrag/UAVBench):
  - 50,000 scenarios generated by "taxonomy-guided LLM prompting" and validated in four stages (schema, constraints, geometry, safety).
  - **UAVBench_MCQ** has 50,000 **LLM-generated** MCQs across 10 reasoning styles, including aerodynamics/physics, navigation, energy management, cyber-physical security and ethics.
  - 32 LLMs were tested. On perception, Qwen3-235B-A22B scored 89.8%, ChatGPT-4o 85.5% and GPT-5 Chat 85.3%. On planning, Qwen3-235B averaged 76.5%. Multi-agent coordination and energy management remain hardest.
  - CC BY 4.0.
- **TypeFly** [Dec 2023] (arXiv 2312.14950): GPT-4 writes programs in MiniSpec, a small custom language, to fly a camera drone. Design choices cut LLM cost and task time by more than 2x — [arXiv](https://arxiv.org/abs/2312.14950v2)
- **ChatGPT for Robotics / PromptCraft** [2023] (Vemprala, Bonatti, Bucker, Kapoor, Microsoft; arXiv 2306.17582): an AirSim drone sample with ChatGPT and a function library — [arXiv](https://arxiv.org/html/2306.17582v1); [GitHub](https://github.com/microsoft/promptcraft-robotics)

#### UAV engineering design and analysis (the target area for AerospaceBench)
- **Mission-driven UAV conceptual design with LLM + RAG** (*Future Internet* 18(7):360, DOI 10.3390/fi18070360): turns a cargo-UAV mission brief into source-grounded sweep-angle ranges. CAD, CFD screening and a closed-loop 6-DoF simulation then act as feasibility gates. This is a case study, not a benchmark — [DOI](https://doi.org/10.3390/fi18070360) [snippet-only]
- **LLM-ML_DroneConstructor** (GitHub hobby project): 17 LangGraph/Ollama agents turn a mission into propulsion selection (motor/prop/ESC from all-up weight and T/W), battery sizing, CAD, wiring and flight-controller configuration. No evaluation — [GitHub](https://github.com/AKSHAY-RSOL/LLM-ML_DroneConstructor)
- UAVBench_MCQ includes "Aerodynamics & Physics" and "Energy & Resource Management" styles, but they are LLM-generated MCQs, not design tasks — [arXiv HTML](https://arxiv.org/html/2511.11252v1)

### Inferences
- **The two groups must be kept apart in AerospaceBench.** VLN/VLA benchmarks (AerialVLN, OpenFly, UAV-Flow and others) measure perception-to-action policy learning. They are robotics benchmarks, not engineering work, and should at most be cross-referenced.
- The SITL harnesses (DroneServer, PX4-ROS2, AutoSimTest) show that **telemetry-graded missions in ArduPilot/PX4 SITL are cheap, reproducible and fully open-source**. They are a natural substrate for UAV operations and flight-test-style tasks such as parameter tuning, geofence and failsafe configuration, log analysis and test-plan generation. The basic command-execution tier is near saturation: 8 of 11 families reach 90–100% on simple missions.
- No benchmark asks a model to size a multirotor or VTOL (motor/prop/battery matching, hover and cruise endurance, payload trade) and checks the result against a physics model or real data. This is a clear, low-cost gap. Possible checkers include an eCalc-style analytic model, a PX4 SITL hover-time check, or bench-test data.

### Gaps
- Leaderboard numbers for the latest models on AerialVLN, CityNav, OpenFly and TravelUAV were not collected. These are robotics benchmarks and outside the engineering focus.
- Per-model scores for the PX4-ROS2 and AutoSimTest papers were not visible in the abstracts.
- The "BEDI" embodied UAV benchmark and "PilotBench" were mentioned in snippets but not verified.

---

## Q5. Flight software and safety-critical aerospace code (cFS, F Prime, PX4, DO-178C-style verification, formal methods, autocoding)

### Takeaway
**No executable benchmark exists for generating or verifying flight software in NASA cFS, F Prime or PX4.** Work in this area is limited to:
- **RepoSpace** (CAS; 825 repository-level samples of spaceborne-equipment code; data availability unverified)
- NL-requirements-to-temporal-logic work on aerospace requirements: SpecVerify (Lockheed Martin CPS + ESBMC), Req2LTL, and LTL generation from industrial aerospace requirements
- an empirical study of issue classification on NASA cFS/F´ issues (fine-tuned small encoders beat zero-shot LLMs)
- a NASA Langley memo cautioning against LLM-generated assurance arguments

No benchmark targets DO-178C, ECSS-E-ST-40C, MC/DC test generation or autocoding.

### Cited Findings
- **RepoSpace** — "Using LLMs for Aerospace Code Generation: Methods, Benchmarks, and Potential Values":
  - Rui He, Liang Zhang, Mengyao Lyu, Liangqing Lyu and Changbin Xue (National Space Science Center, CAS). *Aerospace* 12(6):498, 30 May 2025 — [Crossref](https://api.crossref.org/works/10.3390/aerospace12060498)
  - Described as a "warehouse-level" (i.e. repository-level) benchmark for **spaceborne equipment code**: 825 samples from five real projects. Seven LLMs were tested. Domain-specific factors significantly affect performance. RAG, good prompt templates and high-quality docstrings help — [Crossref abstract](https://api.crossref.org/works/10.3390/aerospace12060498)
  - The MDPI page returned 403, so the programming languages, the metric (pass@k or similarity), per-model scores and public availability are **unverified**.
- **SpecVerify** (Wang, Farrell, Cordeiro, Zhao; IEEE RE 2025; arXiv 2507.04857):
  - derives formal properties from NL requirements for **nine Lockheed Martin cyber-physical systems** (aerospace control components, per search summary) and verifies them with **ESBMC**
  - Claude 3.5 Sonnet + ESBMC reached **46.5% verification accuracy**, "comparable to NASA's CoCoSim" with fewer false positives; ChatGPT and Llama were also compared
  - Source: [arXiv abs](https://arxiv.org/abs/2507.04857)
- **Req2LTL** (Ma, Wen, Su, Liang, Tian, Qin, Yang; ASE 2025; arXiv 2512.17334): LLM decomposition into an "OnionL" intermediate representation followed by rule-based LTL synthesis. On real-world aerospace requirements it scored 88.4% semantic accuracy and 100% syntactic correctness — [arXiv abs](https://arxiv.org/abs/2512.17334)
- **Automated LTL Specification Generation from Industrial Aerospace Requirements** (Ma et al.; arXiv 2604.21715, April 2026): 85% precision and 88% recall on "a real aerospace dataset". Dataset source, size and availability are not stated in the abstract — [arXiv abs](https://arxiv.org/abs/2604.21715)
- **NASA FRET context:** FRET formalises "FRETish" structured NL into LTL and Lustre, and CoCoSim attaches the result to Simulink models. NASA's own FRET material shows no LLM benchmark — [NTRS FRET](https://ntrs.nasa.gov/api/citations/20200001989/downloads/20200001989.pdf); [SpecVerify search summary](https://arxiv.org/html/2507.04857)
- **Issue classification for NASA flight software** (Colavito, Lanubile, Novielli [University of Bari]; Arreza, Shi [NASA GSFC]; *Journal of Systems and Software*, March 2026) — [NTRS citation](https://ntrs.nasa.gov/citations/20260002137):
  - classifies bug vs non-bug issue reports on cFS and F´ issues
  - fine-tuned SetFit encoders consistently beat zero-shot generative LLMs on F1 and bug recall — [search summary](https://www.sciencedirect.com/science/article/abs/pii/S0164121226000853) [snippet-only]
- **NASA/TM-20250001849, "Examining Proposed Uses of LLMs to Produce or Assess Assurance Arguments"** (Graydon & Lehman, NASA Langley, March 2025): a literature-based review concluding that much remains to be demonstrated before LLMs are fit to generate or assess assurance arguments in certification practice — [NTRS PDF](https://ntrs.nasa.gov/api/citations/20250001849/downloads/NASA-TM-20250001849.pdf); [search summary](https://ntrs.nasa.gov/api/citations/20250001849/downloads/NASA-TM-20250001849.pdf)
- **ESA:** an ESA activity is reportedly exploring open-source LLMs (Llama2, Mistral, CodeLlama) across the space-software engineering process — [ESA activity page, snippet-only; fetch returned 404](https://activities.esa.int/node/2563)
- **Autopilot software:** RisConFix covers ArduPilot configuration repair (see Q4). An exploratory study of PX4/ArduPilot bugs gives a taxonomy of eight UAV-specific bug types (limit, math, inconsistency, priority, parameter, hardware support, correction, initialisation). It could seed code-repair tasks — [ResearchGate](https://www.researchgate.net/publication/354057906_An_exploratory_study_of_autopilot_software_bugs_in_unmanned_aerial_vehicles)
- **Generic embedded cross-references** (covered elsewhere): EmbedAgent (arXiv 2506.11003) and ESP32 embedded benchmarks — [arXiv](https://arxiv.org/pdf/2506.11003)

### Inferences
- The FSW gap is large and easy to fill with open-source substrates:
  - cFS and F Prime are open source and have unit-test frameworks
  - PX4 and ArduPilot have SITL and CI test suites, plus large public bug and PR histories
  - SWE-bench-style tasks could be mined from merged fixes in these repositories
- **Contamination risk is high** because these repositories are public and heavily crawled. Use post-cutoff commits or private forks.
- On aerospace-style requirements, NL→LTL formalisation reaches about 85–88% semantic accuracy, while end-to-end verification reaches only about 46.5% (SpecVerify). The hard part is the full requirements→properties→verified-code chain, which makes it a strong candidate task for a DO-178C-style evaluation.

### Gaps
- No LLM benchmark found for:
  - cFS app or F Prime component generation
  - PX4 module code generation or bug-fixing with SITL validation
  - MISRA/JSF/NASA "Power of 10" compliance
  - MC/DC test generation
  - DO-178C/DO-330 certification artefacts (plans, traceability matrices)
  - ECSS-E-ST-40C/Q-ST-80C
  - Simulink/autocoded GNC
- RepoSpace details (languages, metrics, scores, licence) are unverified.

---

## Q6. Benchmarks spanning multiple aerospace sectors (2025–2026), and cross-references

### Takeaway
**No 2025–2026 benchmark spans space, launch, UAV and flight software with authentic engineering tasks.** Multi-topic aerospace sets fall into three groups:
- small QA/MCQ collections: SwRI's evaluation set (SciTech 2025), AeroEngQA (80 aircraft-design QA pairs) and AeroMfg-QA (LLM-generated manufacturing MCQs)
- aviation-operations benchmarks, which are out of scope: ALUE, Pre-Flight, CAMB, AeroCopilotBench and PilotBench
- generic engineering suites with some aerospace items: EngDesign and IndustryCode

I found **no existing benchmark named "AerospaceBench" or "AeroBench"**, so the name appears free.

### Cited Findings
- A search for "AerospaceBench" / "AeroBench" found no benchmark with either name — [search results](https://arxiv.org/pdf/2608.16349)
- **AeroEngQA** (Silva, Marsh, Yong, Middleton, Sóbester; University of Southampton; AIAA 2025 preprint): **80 aircraft-engineering QA pairs** from NASA and NTSB reports and patents. Experts wrote the questions and answers. It is balanced between answerable and unanswerable, simple and complex. GPT-3.5-turbo, GPT-4 and LLaMa3 were evaluated qualitatively with zero-shot, in-context and RAG prompting. The data is released as open source. Aviation-leaning — [preprint PDF](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- **AeroMfg-QA** (Liu et al.; arXiv 2501.17183, January 2025): multi-answer MCQs **generated by LLMs** from aerospace-manufacturing textbooks and guidelines. Abstract conclusion: "capabilities of LLMs in aerospace professional knowledge are in urgent need of improvement" — [arXiv abs](https://arxiv.org/abs/2501.17183)
  - In the Aerospace Assembly category, Gemini-2.0-flash-exp scored 52.66% and Claude-3.5-Sonnet-20241022 52.35% — [search summary](https://arxiv.org/pdf/2501.17183) [snippet-only]
- **SwRI evaluation set** (AIAA 2025-0702): see Q1 — [Crossref](https://api.crossref.org/works/10.2514/6.2025-0702)
- **Aviation, out of scope (cross-reference only):**
  - **ALUE**, FAA/MITRE, September 2025 (AIAA 2025-3247) — [MITRE](https://www.mitre.org/news-insights/news-release/mitre-and-faa-introduce-novel-aerospace-large-language-model-evaluation)
  - **Pre-Flight**: 300 MCQs; best model 82.7%, up from about 75% in early 2025 — [arXiv 2607.01829](https://arxiv.org/abs/2607.01829)
  - **CAMB**, civil aviation maintenance — [arXiv 2508.20420](https://arxiv.org/html/2508.20420v1)
  - **AeroCopilotBench** — [arXiv 2608.16349](https://arxiv.org/pdf/2608.16349)
- **Generic engineering, covered elsewhere:**
  - **EngDesign**: nine domains including aerospace, simulation-based grading — [arXiv 2509.16204](https://arxiv.org/abs/2509.16204)
  - **IndustryCode**: 125 problems and 579 sub-problems across sectors including aerospace, in Python, C++, Matlab and Stata — [arXiv 2604.02729](https://arxiv.org/pdf/2604.02729)
  - **"On the Evaluation of Engineering AGI"**: a Bloom's-taxonomy-based framework — [arXiv 2505.10653](https://arxiv.org/abs/2505.10653)
  - **AVPD**: 18 expert-designed aerospace geometry tasks in Grasshopper, using GPT 5.4 — [arXiv 2606.16806](https://arxiv.org/pdf/2606.16806)

### Inferences
- Multi-sector aerospace evaluation is still dominated by **small, static QA** (80 items for AeroEngQA, 60 for SpaceQA, 193 for Astro-MCQ). It is often **LLM-generated** (AeroMfg-QA, UAVBench), and scores tend to sit in the 50–90% band. The agentic, verifier-graded sector benchmarks (AstroAgentBench, GTOC-12 agent, RocketBench, DroneServer) are each single-sector and small in case count. A cross-sector benchmark of authentic, executable, physics-verified tasks would be new.

### Gaps
- I could not access the full content of SwRI AIAA 2025-0702 or verify its size or scores.
- Chinese-language aerospace benchmarks could exist (CAS/Beihang groups are active), but none were surfaced by English searches.

---

## Q7. Relevance to AerospaceBench: reusable ideas, pitfalls, and uncovered sub-disciplines and work types

### Takeaway
Six reusable patterns exist:
1. external physics and constraint verifiers with solver baselines (AstroAgentBench)
2. per-answer numeric tolerances, a private held-out split and canary strings (APBench)
3. expert-designed binary rubrics with LLM judges checked by ICC (GTOC-12)
4. weighted multi-objective scoring in simulation with a human-expert baseline (RocketBench)
5. telemetry-graded SITL missions that include safety and adversarial cases (DroneServer)
6. real-data operations datasets with operator-curated labels (ESA-ADB, OPS-SAT-AD)

The main pitfalls are LLM-generated items, textbook contamination, tiny case counts, open-weight-only testing, judge bias and proprietary dependencies. The biggest uncovered areas are:
- subsystem sizing and budgets
- structures, thermal and loads
- test/AIT and verification
- certification and standards compliance
- orbital launch-vehicle design
- operations procedures and FDIR
- flight-software generation and verification
- UAV airframe and propulsion sizing

### Cited Findings
**Reusable designs**
- AstroAgentBench grades in layers: schema/timeout, then hard physical constraints (SGP4, frames, geometry, power, slew, comms), then mission value. It is anchored to MILP/CP/heuristic solver baselines. It reports process metrics such as invalid rate and commit-before-reading-contract — [arXiv HTML](https://arxiv.org/html/2601.11354)
- APBench uses magnitude-scaled numeric tolerances, avoids MCQ, keeps a private 1,990-item split and embeds a canary string — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- The GTOC-12 study uses a 26-criterion binary expert rubric, a judge with reported ICC and cross-judge Δ, 100 drafts per model and fixed time and step budgets — [arXiv HTML](https://arxiv.org/html/2602.03630)
- RocketBench uses RocketPy simulation with weighted objectives, iterative refinement up to 30 iterations, best-of-10 sampling and a human baseline — [arXiv HTML](https://arxiv.org/html/2504.19394v2)
- DroneServer grades automatically from telemetry in two autopilots and includes geofence and prompt-injection safety cases — [arXiv HTML](https://arxiv.org/html/2601.15486v3)
- RocketSmith validated designs physically with real flights — strong evidence but expensive and slow — [arXiv HTML](https://arxiv.org/html/2606.00097v3)

**Pitfalls**
- **LLM-generated items:** UAVBench's 50k MCQs and AeroMfg-QA's questions are model-generated, so their validity depends on generator quality — [UAVBench](https://arxiv.org/html/2511.11252v1); [AeroMfg-QA](https://arxiv.org/abs/2501.17183)
- **Textbook and public-source contamination:** APBench's open items come from public textbooks and web resources (Braeunig, for example), and the authors used canary strings to reduce leakage — [PMC full text](https://www.ebi.ac.uk/europepmc/webservices/rest/PMC11885820/fullTextXML)
- **Tiny evaluation sets:** 5 held-out cases per family in AstroAgentBench; 60 QA in SpaceQA; 80 in AeroEngQA — [AstroAgentBench](https://arxiv.org/html/2601.11354); [Belo et al.](https://arxiv.org/html/2605.27444); [AeroEngQA](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- **No frontier models tested:** AstroMind tested only 4–32B open models — [arXiv HTML](https://arxiv.org/html/2605.24573)
- **Single-judge bias, no expert ground truth:** AstroMind uses one DeepSeek-R1 judge. The GTOC-12 study acknowledges it has no expert ground-truth annotations — [AstroMind](https://arxiv.org/html/2605.24573); [GTOC-12](https://arxiv.org/html/2602.03630)
- **Proprietary dependencies:** KSP (a commercial game) for KSPDG; ANSYS Fluent and ABAQUS in RocketAgent — [KSPDG](https://github.com/mit-ll/spacegym-kspdg); [RocketAgent](https://arxiv.org/html/2610.06044)
- **No release:** RocketBench and RocketAgent published no repository, which hurts reproducibility — [RocketBench](https://arxiv.org/html/2504.19394v2); [RocketAgent](https://arxiv.org/html/2610.06044)
- **Harness confound:** each model in AstroAgentBench was evaluated inside its vendor's own CLI harness (Claude Code, Codex CLI, Kimi CLI, OpenCode), so model and harness effects are confounded — [arXiv HTML](https://arxiv.org/html/2601.11354)

### Inferences
- **Uncovered sub-disciplines and work types** (no LLM benchmark found in my searches):
  1. spacecraft power, thermal, ADCS, propulsion, comms-link and mass/ΔV budgeting, including CubeSat design
  2. structures and launch loads (random vibration, shock, coupled loads), radiation and EEE-parts selection
  3. AIT and test: environmental test planning, interpreting test data, non-conformance reports
  4. certification and standards: ECSS, NASA-STD-7009/8739, DO-178C/254, FAA Part 450, range safety
  5. orbital launch vehicles: sizing and staging, ascent trajectory, engine cycles, solid/hybrid motors, reuse
  6. operations: procedure and telecommand authoring, FDIR design, anomaly root-cause analysis with ground truth, conjunction-avoidance decisions
  7. flight software: cFS/F Prime/PX4 code generation and repair with tests, MC/DC, static-analysis compliance
  8. UAV engineering: airframe, propulsion and battery sizing, endurance and performance estimation, log-based flight-test analysis
  9. space manufacturing and maintenance (only aviation-leaning AeroMfg-QA and CAMB exist)
- **Open-source tool stack that AerospaceBench could standardise on**, avoiding STK, FreeFlyer and MATLAB, none of which appeared in any LLM benchmark found:
  - astrodynamics: poliastro/hapsira, PyKEP/PyGMO, TudatPy, SGP4/Skyfield, Orekit, GMAT, Basilisk
  - launch: RocketPy, OpenRocket, RocketCEA, CoolProp
  - UAV: PX4 and ArduPilot SITL, Gazebo
  - FSW: cFS, F Prime, ESBMC, Kind2/CoCoSim
  - systems engineering: Capella
  - (GMAT, Orekit and Basilisk are the only items here that are my suggestion rather than tools reported in the papers above.)
- **Saturation outlook:**
  - Knowledge MCQ is near or past saturation: AstroMLab 85% in 2024, UAVBench perception about 85–90%, Pre-Flight 82.7%.
  - Single-answer textbook astrodynamics was around 67% for o1-preview in early 2025 and is likely higher now (unverified).
  - Agentic design tasks keep large headroom: GTOC-12 has no valid submission, RocketBench precision landing is far below the human, and geometry-heavy AstroAgentBench families trail solvers by 13–27 points.
- **Contamination strategy:** borrow APBench's private split and canary strings, AstroAgentBench's procedurally generated instances with solver anchors, and post-2026 problem statements or competition editions. Examples are new GTOC/SpOC editions and new ESA-ADB anomalies.

### Gaps
- I did not verify current (late-2026) frontier scores on APBench, AstroMLab or the VLN benchmarks. Newer models (Claude Opus 4.6, GPT-5.4 and others) appear only in AstroAgentBench and the GTOC-12 study.
- I found no evaluations from industry primes (Airbus, Thales, Lockheed, SpaceX) or from JAXA, CNES or DLR. Such work may exist in non-public reports or in non-English literature.
- IAC 2024–2026 proceedings and AIAA ASCEND papers were not systematically searchable here. There may be additional LLM case studies (e.g., LLM assistants for concurrent design facilities) that are not indexed on arXiv.
