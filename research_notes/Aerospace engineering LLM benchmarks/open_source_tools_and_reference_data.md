# Open-Source Aerospace Engineering Software and Public Reference/Validation Data for a Reproducible LLM-Agent Benchmark (AerospaceBench)

Research date: 2026-10-06. Method note: licence, last-push, latest-release and star data for GitHub/GitLab/PyPI projects were pulled directly from the GitHub REST API (authenticated `gh api`), the GitLab v4 API and the PyPI JSON API on 2026-10-06; each row cites the project page the data came from. "Push" = most recent push to the default repository (an activity proxy); "release" = latest tagged/published release. Where GitHub reports `NOASSERTION`, the licence text was read from the repository LICENSE file. Assessments of headless suitability, install difficulty and compute cost are the researcher's engineering judgement and are placed under "Inferences", not "Cited Findings". The session's web-search quota ran out partway through, so a few items were checked only by fetching primary pages directly or not at all; these are listed under "Gaps".

---

## (A) Open-source tools: licence, maintainer/activity, interface, headless suitability, validation pedigree

### Takeaway
A credible, fully open, containerisable toolchain now covers most aerospace disciplines at low-to-mid fidelity: aerodynamics (XFOIL, AVL, OpenVSP/VSPAERO, SU2, OpenFOAM), structures (CalculiX, Code_Aster, MYSTRAN, FEniCSx), MDO and aircraft sizing (OpenMDAO, Dymos, NASA Aviary, pyCycle, FAST-OAD, OpenAeroStruct, TiGL/CPACS), propulsion (Cantera, NASA CEA, which NASA open-sourced under Apache-2.0, plus RocketPy), flight dynamics (JSBSim, PX4/ArduPilot SITL, python-control), space (GMAT R2026a, Orekit, Basilisk, Tudat, SPICE, cFS, F Prime) and CAD/meshing (CadQuery, build123d, FreeCAD, Gmsh, OCCT). Almost all were updated in 2025–2026. Four licence surprises matter for the pilot. CEASIOMpy was relicensed from Apache-2.0 to a proprietary, no-cloud, no-redistribution licence in February 2026. OpenSees is non-commercial only. RCAIDE is AGPL-3.0. FUN3D, Cart3D and OVERFLOW are not open; they are restricted to U.S. users.

### Cited Findings

#### A1. Aerodynamics / CFD

| Tool | Licence (verified) | Maintainer / activity (as of 2026-10-06) | Notes | Source |
|---|---|---|---|---|
| XFOIL | GPL (page states GPL) | M. Drela, MIT; latest listed version XFOIL 6.99 (no newer version on the page) | 2D viscous–inviscid airfoil analysis | [XFOIL page](https://web.mit.edu/drela/Public/web/xfoil/) |
| xfoil-python wrapper | GPL-3.0 | DARcorporation; last push 2021-06, release 0.0.16 (2019) – dormant | Python binding to XFOIL | [GitHub](https://github.com/DARcorporation/xfoil-python); [PyPI xfoil 1.1.1 (2019)](https://pypi.org/project/xfoil/) |
| XFLR5 | GPLv3 | SourceForge; v6.62 files posted 24 Mar 2026 | Qt GUI around XFOIL + VLM/panel for wings/planes | [SourceForge](https://sourceforge.net/projects/xflr5/) |
| AVL | GPL ("By downloading the software you agree to abide by the GPL conditions") | M. Drela/H. Youngren, MIT; latest AVL 3.40b (binaries for Linux64, macOS x86/ARM, Windows) | Extended VLM + flight-dynamics eigenmodes | [AVL page](https://web.mit.edu/drela/Public/web/avl/) |
| avlwrapper (Python) | GPL-3.0-only | v0.5.0 uploaded 2026-10-04 | Python driver for AVL | [PyPI](https://pypi.org/project/avlwrapper/) |
| OpenVSP (+VSPAERO) | NASA Open Source Agreement (NOSA) 1.3 | NASA-originated, community-maintained; push 2026-10-06; tag `openvsp_3.49.0-fedora` (no GitHub "release" objects) | Parametric geometry, VSPAERO VLM/panel solver | [GitHub OpenVSP](https://github.com/OpenVSP/OpenVSP) |
| SU2 | LGPL-2.1 (LICENSE.md) | Stanford-origin, SU2 Foundation community; v8.5.0 released 2026-04-27; push 2026-10-06; ~1.8k stars | RANS/URANS, adjoint, FSI | [GitHub SU2](https://github.com/su2code/SU2) |
| OpenFOAM (Foundation) | GPL | OpenFOAM Foundation; OpenFOAM 14 released 14 July 2026 | General finite-volume CFD | [openfoam.org/download](https://openfoam.org/download/); [GitHub OpenFOAM-dev](https://github.com/OpenFOAM/OpenFOAM-dev) |
| OpenFOAM (ESI/OpenCFD) | GPL | OpenCFD/ESI; "The current release, OpenFOAM-v2606, was released on 26/06/2026" | Parallel fork with different release cadence | [openfoam.com/download](https://www.openfoam.com/download/) |
| DUST (PoliMi + A^3 by Airbus) | MIT ("DUST is free and open-source, released under the MIT License") | Official GitLab `dust_group/dust`: latest release 0.6.1-b (2022-02-07); last repo activity 2022-02-07 | Mid-fidelity vortex particle / panel / lifting-line solver for complex rotary-wing and VTOL configurations; aeroelastic coupling with MBDyn | [DUST site](https://www.dust.polimi.it/); [GitLab](https://gitlab.com/dust_group/dust) |
| AeroSandbox | MIT | P. Sharpe; v4.2.10 (2026-07-05); push 2026-09-15; ~1.36k stars | Differentiable Python aircraft design with VLM, XFOIL wrappers etc. | [GitHub](https://github.com/peterdsharpe/AeroSandbox); [PyPI](https://pypi.org/project/aerosandbox/) |
| NeuralFoil | MIT | P. Sharpe; v0.3.3 (2026-07-11) | Neural-network airfoil surrogate (physics-informed ML) | [GitHub](https://github.com/peterdsharpe/NeuralFoil); [PyPI](https://pypi.org/project/neuralfoil/) |
| FLOWUnsteady | MIT | BYU FLOW Lab; v3.4.0 (2025-08-25); push 2026-09-22 | Julia; reformulated VPM for rotors/eVTOL | [GitHub](https://github.com/byuflowlab/FLOWUnsteady) |
| pyvlm | MIT | v0.0.12 (2025-08-03) | Small Python VLM | [PyPI](https://pypi.org/project/pyvlm/) |
| CFL3D (NASA) | Apache-2.0 | `nasa/CFL3D`; last push 2022-08-12 (dormant, no releases) | Structured-grid RANS code with long validation history | [GitHub nasa/CFL3D](https://github.com/nasa/CFL3D) |
| ADflow (MDOLab) | LGPL-2.1 ("ADflow is licensed under the GNU Lesser General Public License (LGPL), version 2.1") | U. Michigan MDOLab; v2.13.1 (2026-05-12); push 2026-10-06 | Structured overset RANS with adjoint; core of MACH-Aero | [GitHub adflow](https://github.com/mdolab/adflow) |
| MACH-Aero | No licence file detected by GitHub | MDOLab; push 2026-03-24 | Docs/tutorial repo for the MACH framework (ADflow, pyGeo, IDWarp, pyOptSparse) | [GitHub MACH-Aero](https://github.com/mdolab/MACH-Aero) |
| **FUN3D (NASA)** | **Not open source** | Request form requires a "U.S. Person", U.S. mailing address and U.S. phone; current release 14.3 | Restricted to U.S. users | [FUN3D manual/request page](https://fun3d.larc.nasa.gov/chapter-1.html) |
| **Cart3D, OVERFLOW (NASA)** | **Not open source** | Offered as "U.S. Release Only"; OVERFLOW "available to U.S. companies, universities, and individuals under an appropriate Software Usage Agreement" | Restricted | [Rescale/NASA CFD apps](https://rescale.com/?p=82366); [NASA OVERFLOW page](https://www.nasa.gov/?p=899935) |

#### A2. Structures, aeroelasticity, multibody, fatigue

| Tool | Licence (verified) | Maintainer / activity | Notes | Source |
|---|---|---|---|---|
| CalculiX (ccx) | GNU GPL (site links GPL text) | G. Dhondt; current ccx 2.23 | Abaqus-like input deck FEA; static/dynamic/thermal/nonlinear | [dhondt.de](http://www.dhondt.de/) |
| Code_Aster | GPL-3.0 (GitLab licence field) | EDF; tags 18.1.9 and 17.5.6 on 2026-10-02; activity 2026-10-06 | Very broad nonlinear FEA; pairs with Salome | [GitLab codeaster/src](https://gitlab.com/codeaster/src) |
| Elmer FEM | GPL-2.0 (+ElmerGUI exception) | CSC Finland; release-26.2 (2026-04-28); push 2026-10-06 | Multiphysics FEM | [GitHub elmerfem](https://github.com/ElmerCSC/elmerfem) |
| FEniCSx (DOLFINx) | LGPL-3.0 | FEniCS Project; v0.11.0 (2026-06-10) | Python/C++ FEM from variational forms | [GitHub dolfinx](https://github.com/FEniCS/dolfinx) |
| MYSTRAN | MIT | MYSTRAN community; v19.0.0 (2026-06-29); push 2026-09-30; PyPI `mystran` 18.0.0.1 (2026-05-22) | Nastran-compatible bulk-data input; README lists "Nastran compatibility", "Linear Static Analysis", "Linear Elastic Buckling Analysis" | [GitHub MystranSolver](https://github.com/MystranSolver/MYSTRANSolver); [PyPI](https://pypi.org/project/mystran/) |
| NASTRAN-95 (NASA) | NOSA 1.3 | `nasa/NASTRAN-95`; last push 2024-04-27, no releases | Legacy original NASTRAN source + manuals | [GitHub](https://github.com/nasa/NASTRAN-95) |
| pyNastran | BSD-style ("Redistribution and use in source and binary forms…", © 2011-2026 S. Doyle) | Last release v1.4.1 (2024-03-26), but repo push 2026-10-05 | Read/write BDF/OP2/F06/OP4 (I/O and post-processing only, no solver) | [GitHub](https://github.com/SteveDoyle2/pyNastran); [PyPI](https://pypi.org/project/pyNastran/) |
| pyfe3d | BSD-3 | v0.10.0 (2026-09-25) | Lightweight Python FE solver for structural analysis/optimisation | [PyPI](https://pypi.org/project/pyfe3d/) |
| OpenSees | **Custom UC Regents licence: free use/redistribution "by educational, research, and non-profit entities for noncommercial purposes only"; other entities "for internal purposes only"** | v3.8.0 (2026-02-18) | Earthquake/civil-structural focus; low aerospace relevance | [GitHub OpenSees LICENSE](https://github.com/OpenSees/OpenSees) |
| SHARPy | BSD-3-Clause | Imperial College London; v2.4 (2025-03-21); push 2026-05-07 | Nonlinear aeroelasticity (geometrically exact beams + UVLM), flight dynamics of very flexible aircraft | [GitHub sharpy](https://github.com/ImperialCollegeLondon/sharpy) |
| MBDyn | GPL 2.1 ("released under GNU's GPL 2.1") | Politecnico di Milano DAER; GitLab `DAER/mbdyn` activity 2026-10-06 (no tags on GitLab); Debian ships libmbc 1.7.3 | Multibody multiphysics incl. rotorcraft aeroelasticity; couples with DUST | [mbdyn.org](https://www.mbdyn.org/); [GitLab PoliMi](https://public.gitlab.polimi.it/DAER/mbdyn); [Debian libmbc-dev 1.7.3](https://packages.debian.org/ja/sid/libmbc-dev) |
| preCICE | LGPL-3.0 | v3.4.1 (2026-04-21) | Partitioned multiphysics coupling (FSI) between CFD/FEM codes | [GitHub](https://github.com/precice/precice) |
| composipy | (licence not stated on PyPI) | v1.7.5 (2026-09-29) | Composite laminate (CLT) calculations | [PyPI](https://pypi.org/project/composipy/) |
| fatpack / rainflow / py-fatigue | ISC / MIT / (not stated) | 0.7.8 (2024-09) / 3.2.0 (2023-04, ASTM E1049-85 rainflow) / 2.1.1 (2026-06-09) | Open fatigue-analysis building blocks (cycle counting, S-N, damage summation) | [fatpack](https://pypi.org/project/fatpack/); [rainflow](https://pypi.org/project/rainflow/); [py-fatigue](https://pypi.org/project/py-fatigue/) |
| **NASGRO** | **Commercial** (royalty-free for NASA, FAA, ESA, consortium) | SwRI + NASA; v11.1 released 3 Sep 2025 | Industry-standard fracture/fatigue crack growth | [SwRI NASGRO](https://www.swri.org/nasgro); [NASA OSMA 2026 news](https://sma.nasa.gov/news/articles/newsitem/2026/03/26/osma-continues-development-of-nasgro-software) |
| **AFGROW** | **Commercial** (v5.4, US$1,650); legacy free versions run only on Windows XP or earlier | LexTech, Inc. (originated at AFRL) | Damage-tolerance analysis | [afgrow.net](https://afgrow.net/); [AFGROW FAQ](https://www.afgrow.net/support/faq.aspx) |

#### A3. Aircraft design / MDO

| Tool | Licence (verified) | Maintainer / activity | Notes | Source |
|---|---|---|---|---|
| OpenMDAO | Apache-2.0 | NASA Glenn; v3.45.1 (2026-09-10) | MDO framework with analytic derivatives | [GitHub](https://github.com/OpenMDAO/OpenMDAO); [PyPI](https://pypi.org/project/openmdao/) |
| Dymos | Apache-2.0 | NASA/OpenMDAO; v1.15.1 (2026-03-12) | Optimal-control / trajectory optimisation | [GitHub](https://github.com/OpenMDAO/dymos) |
| NASA Aviary | Apache-2.0 (© 2023 U.S. Government/NASA) | v1.0.2 (2026-08-11); push 2026-10-06 | Aircraft sizing + mission; "incorporates aircraft sizing equations from its predecessors GASP … and FLOPS"; "validated using output and data from the GASP and FLOPS codes themselves" | [GitHub Aviary README](https://github.com/OpenMDAO/Aviary); [PyPI](https://pypi.org/project/aviary/) |
| pyCycle | Apache-2.0 | NASA/OpenMDAO; v4.4.0 (2025-10-15); push 2026-05-20 | Thermodynamic cycle analysis for gas turbines with derivatives | [GitHub](https://github.com/OpenMDAO/pyCycle) |
| OpenAeroStruct | Apache-2.0 (LICENSE file); PyPI metadata says "BSD-3" (inconsistent) | MDOLab; v2.12.0 (2025-10-06) | Coupled VLM + beam aerostructural optimisation | [GitHub](https://github.com/mdolab/OpenAeroStruct); [PyPI](https://pypi.org/project/openaerostruct/) |
| FAST-OAD | GPL-3.0 | ONERA/ISAE-SUPAERO; v1.10.0 (2026-06-02) | Aircraft sizing built on OpenMDAO | [GitHub](https://github.com/fast-aircraft-design/FAST-OAD); [PyPI](https://pypi.org/project/fast-oad/) |
| RCAIDE (successor to SUAVE) | **AGPL-3.0** | Stanford/LEADS group; push 2026-10-06; no GitHub releases; not on PyPI | Conventional and electric aircraft/rotorcraft conceptual design | [GitHub RCAIDE_LEADS](https://github.com/leadsgroup/RCAIDE_LEADS) |
| SUAVE | LGPL-2.1 | Last release 2.5.2 (2022-03-17); last push 2024-02-14 (dormant) | Legacy conceptual design | [GitHub](https://github.com/suavecode/SUAVE) |
| **CEASIOMpy** | **Relicensed 2026-02-05 from Apache-2.0 to "Proprietary and Confidential": evaluation/academic only; prohibits commercial use, "Cloud & HPC Deployment", SaaS and "Redistribution"** | CFS Engineering (CH); v2.0.0 (2026-02-24) | Conceptual-to-CFD workflow around CPACS/SU2/Gmsh | [GitHub LICENSE](https://github.com/cfsengineering/CEASIOMpy/blob/main/LICENSE); earlier LICENSE in git history was Apache-2.0 (verified via commit history of LICENSE) |
| TiGL | Apache-2.0 | DLR; v3.5.0 (2026-09-01) | CPACS geometry library (OCCT-based) | [GitHub](https://github.com/DLR-SC/tigl) |
| TiXI | Apache-2.0 | DLR; v3.3.2 (2026-05-13) | XML interface for CPACS | [GitHub](https://github.com/DLR-SC/tixi) |
| CPACS | Apache-2.0 | DLR; v3.5.1 (2026-09-04) | Aircraft data-exchange schema | [GitHub](https://github.com/DLR-SL/CPACS) |

#### A4. Rotorcraft

| Item | Status (verified) | Source |
|---|---|---|
| CAMRAD II / NDARC | Not open. NASA's RCOTOOLS (catalog ID ARC-18184-1, "U.S. and Foreign Release") ships only *wrappers* for NDARC and CAMRAD II, for use in OpenMDAO | [NASA RCOTOOLS](https://software.nasa.gov/software/ARC-18184-1) |
| RCAS (U.S. Army) | No primary source found this session on its licence/availability | (see Gaps) |
| DUST + MBDyn | Open (MIT + GPL-2.1). DUST site cites the coupled aeroelastic rotary-wing workflow (Savino et al., 2021) | [DUST site](https://www.dust.polimi.it/) |
| FLOWUnsteady | MIT; rotor/eVTOL VPM aerodynamics | [GitHub](https://github.com/byuflowlab/FLOWUnsteady) |
| OpenFAST (wind turbine aeroelastic, BEM/AeroDyn) | Apache-2.0; v5.0.0 (2026-03-12) | [GitHub](https://github.com/OpenFAST/openfast) |
| CCBlade (BEM) | NLRWindSystems/CCBlade; v1.3 (2022); push 2025-08-07; GitHub licence NOASSERTION | [GitHub](https://github.com/NLRWindSystems/CCBlade) |
| pyBEMT | MIT; last push 2022-10 (dormant) | [GitHub](https://github.com/kegiljarhus/pyBEMT) |

#### A5. Propulsion

| Tool | Licence (verified) | Activity | Notes | Source |
|---|---|---|---|---|
| **NASA CEA** (Chemical Equilibrium with Applications) | **Apache-2.0, now open source on GitHub** | `nasa/cea` v3.3.4 (2026-08-28); push 2026-09-25 | Reference code for equilibrium/rocket performance | [GitHub nasa/cea](https://github.com/nasa/cea) |
| RocketCEA | GPL-3.0 | v1.2.3 (2026-01-19) | Python wrapper of CEA Fortran | [GitHub](https://github.com/sonofeft/RocketCEA); [PyPI](https://pypi.org/project/rocketcea/) |
| RocketIsp | GPL-3 | v0.1.13 (2026-07-19) | Liquid rocket Isp/efficiency estimates | [PyPI](https://pypi.org/project/rocketisp/) |
| Cantera | BSD-3-Clause | v3.2.0 (2025-11-18) | Chemical kinetics, thermo, transport | [GitHub](https://github.com/Cantera/cantera); [PyPI](https://pypi.org/project/cantera/) |
| pyCycle | Apache-2.0 | v4.4.0 (2025-10-15) | Gas-turbine cycle | [GitHub](https://github.com/OpenMDAO/pyCycle) |
| RocketPy | MIT | v1.13.0 (2026-07-22); ~1.1k stars | 6-DOF rocket trajectory, Monte Carlo dispersion | [GitHub](https://github.com/RocketPy-Team/RocketPy) |
| OpenRocket | GPL v3 or later | Release tag `release-24.12` (published 2025-07-27); push 2026-10-05; ~3.2k stars | Java GUI model-rocket simulator; Python scripting via `orhelper` (0.1.3, 2021) | [GitHub](https://github.com/openrocket/openrocket); [orhelper PyPI](https://pypi.org/project/orhelper/) |
| proptools | MIT | Last push 2022-10; PyPI 0.2 (2016) (dormant) | Nozzle/turbopump/rocket formulae | [GitHub](https://github.com/mvernacc/proptools) |
| T-MATS (NASA) | Apache-2.0 | v1.3.3 (2024-04) | **Requires MATLAB/Simulink → excluded by pilot rules** | [GitHub](https://github.com/nasa/T-MATS) |
| **NPSS** | Not open; maintained by SwRI with an NPSS Consortium ("SwRI's NPSS development team has been adding new capabilities… since the release of NPSS v2.6.1 in December 2013") | — | Industry engine-cycle standard | [SwRI NPSS](https://www.swri.org/npss) |

#### A6. Flight dynamics, simulation, control

| Tool | Licence (verified) | Activity | Source |
|---|---|---|---|
| JSBSim | LGPL-2.1 | v1.3.1 (2026-05-17); pip `jsbsim`; ~2.3k stars | [GitHub](https://github.com/JSBSim-Team/jsbsim); [PyPI](https://pypi.org/project/jsbsim/) |
| FlightGear | GPL-2.0-or-later | Tag 2024.1.7 (2026-08-09) | [GitLab](https://gitlab.com/flightgear/flightgear); [download](https://www.flightgear.org/download/) |
| PX4 Autopilot | BSD-3-Clause | v1.17.0 (2026-05-13); ~12.8k stars | [GitHub](https://github.com/PX4/PX4-Autopilot) |
| ArduPilot | GPL-3.0 | Plane-4.7.1 (2026-09-03); ~16k stars | [GitHub](https://github.com/ArduPilot/ardupilot) |
| Gazebo (gz-sim) | Apache-2.0 | gz-sim10 10.0.0 (2025-10-14); push 2026-10-05 | [GitHub](https://github.com/gazebosim/gz-sim) |
| python-control | BSD-3-Clause | 0.10.2 (2025-07-05); push 2026-09-29 | [GitHub](https://github.com/python-control/python-control) |
| Scilab (Xcos) | (licence not returned by GitLab API) | 2026.1.0 (2026-05-19) | [GitLab](https://gitlab.com/scilab/scilab) |
| GNU Octave | GPL (GNU project) | "GNU Octave 11.3.0 Released" | [octave.org news](https://octave.org/news.html) |
| OpenModelica | OSMC Public License | v1.27.1 (2026-09-08) | [GitHub](https://github.com/OpenModelica/OpenModelica) |

#### A7. Space: astrodynamics, GNC simulation, flight software, ground systems

| Tool | Licence (verified) | Activity | Source |
|---|---|---|---|
| NASA GMAT | Apache License 2.0 (SourceForge project licence) | **R2026a** released (files incl. Linux/macOS/Windows binaries and source, ~2 Apr 2026). GitHub mirror ChristopherRabotin/GMAT is stale (R2018a) | [SourceForge GMAT](https://sourceforge.net/projects/gmat/) |
| Orekit | Apache-2.0 | 13.1.9 (2026-10-03); Python via `orekit-jpype` 13.1.9.0 (2026-10-04) | [GitLab Orekit](https://gitlab.orekit.org/orekit/orekit); [PyPI](https://pypi.org/project/orekit-jpype/) |
| poliastro | MIT | **Archived** (last push 2023-10-14; v0.17.0 2022) | [GitHub](https://github.com/poliastro/poliastro) |
| hapsira (poliastro fork) | MIT | v0.18.0 (2023-12-24); last push 2024-05 (stalled) | [GitHub](https://github.com/pleiszenburg/hapsira) |
| Basilisk | ISC | AVS Lab (CU Boulder); v2.12.0 (2026-09-21); pip `bsk` | [GitHub](https://github.com/AVSLab/basilisk); [PyPI](https://pypi.org/project/bsk/) |
| NASA 42 | NOSA ("NASA Open Source Software Agreement.pdf" in /License) | E. Stoneking (NASA GSFC); push 2026-08-13; no releases | [GitHub](https://github.com/ericstoneking/42) |
| Tudat / tudatpy (TU Delft) | BSD-3-Clause | tudatpy push 2026-10-06, tag v1.0.0; distributed via conda (not on PyPI); separate `tudat` C++ repo archived | [GitHub tudatpy](https://github.com/tudat-team/tudatpy); [GitHub tudat](https://github.com/tudat-team/tudat) |
| SpiceyPy (NAIF SPICE wrapper) | MIT | v8.2.0 (2026-07-24) | [GitHub](https://github.com/AndrewAnnex/SpiceyPy) |
| pykep / heyoka | MPL-2.0 | 3.0.1 (2026-07-06) / 7.13.2 (2026-09-25) | [pykep](https://pypi.org/project/pykep/); [heyoka](https://pypi.org/project/heyoka/) |
| Nyx Space | AGPL-3.0-or-later | 2.6.0 (2026-09-12) | [PyPI](https://pypi.org/project/nyx-space/) |
| Skyfield / sgp4 / Astropy | MIT / MIT / BSD-3 | 1.55 (2026-08) / 2.27 (2026-07) / 8.0.1 (2026-07) | [skyfield](https://pypi.org/project/skyfield/); [sgp4](https://pypi.org/project/sgp4/); [astropy](https://pypi.org/project/astropy/) |
| NASA Trick | NOSA 1.3 | 25.1.1 (2026-09-04) | [GitHub](https://github.com/nasa/trick) |
| NASA cFS | Apache-2.0 | v7.0.1 (2026-05-14); ~1.5k stars | [GitHub](https://github.com/nasa/cFS) |
| NASA F Prime | Apache-2.0 | v4.4.0 (2026-09-30); ~11.8k stars; `fprime-tools` 4.4.0 on PyPI | [GitHub](https://github.com/nasa/fprime); [PyPI](https://pypi.org/project/fprime-tools/) |
| NASA Open MCT | Apache-2.0 (LICENSE: "Open MCT is licensed under the Apache License") | v4.3.1 (2026-08-31); ~13k stars | [GitHub](https://github.com/nasa/openmct) |

#### A8. Systems engineering, safety, reliability, prognostics

| Tool | Licence (verified) | Activity | Source |
|---|---|---|---|
| Eclipse Capella (Arcadia MBSE) | EPL-2.0 | v7.1.0 (2026-08-03) | [GitHub](https://github.com/eclipse-capella/capella) |
| capellambse (Python access to Capella models) | Apache-2.0 AND OFL-1.1 | 0.8.1 (2026-01-20) | [PyPI](https://pypi.org/project/capellambse/) |
| SysML v2 Pilot Implementation | EPL-2.0 | Release "2026-08" (2026-09-11) | [GitHub](https://github.com/Systems-Modeling/SysML-v2-Pilot-Implementation) |
| OMG SysML 2.0 specification | "Document Status: formal", "Publication Date: September 2025", IPR mode Non-Assert | — | [OMG SysML spec](https://www.omg.org/spec/SysML/) |
| Syside Automator (SysML v2 Python) | **Proprietary** (LicenseRef-Proprietary) | 0.11.0 (2026-09-29) | [PyPI](https://pypi.org/project/syside/) |
| SCRAM (fault tree / event tree) | GPL-3.0 | rakhimov/scram; last push 2023-09-19 (dormant) | [GitHub](https://github.com/rakhimov/scram) |
| reliability (Python) | LGPL-3.0 | v0.9.0 (2025-03-07) | [GitHub](https://github.com/MatthewReid854/reliability) |
| NASA ProgPy (prognostics) | NOSA 1.3 ("NASA-1.3" on PyPI) | GitHub v1.8.0 (2025-06-08); PyPI 1.7.1 | [GitHub](https://github.com/nasa/progpy); [PyPI](https://pypi.org/project/progpy/) |

#### A9. CAD, geometry, meshing, manufacturing

| Tool | Licence (verified) | Activity | Source |
|---|---|---|---|
| FreeCAD | LGPL-2.1 | 1.1.4 (2026-09-28); ~34k stars | [GitHub](https://github.com/FreeCAD/FreeCAD) |
| CadQuery | Apache-2.0 | v2.8.0 (2026-06-20) | [GitHub](https://github.com/CadQuery/cadquery) |
| build123d | Apache-2.0 | v0.13.0 (2026-09-21) | [GitHub](https://github.com/gumyr/build123d) |
| Open CASCADE Technology (OCCT) | LGPL-2.1 | V8.0.1 (2026-07-30) | [GitHub](https://github.com/Open-Cascade-SAS/OCCT) |
| Gmsh | GPLv2+ | 4.15.2 (2026-03-24); pip `gmsh` | [GitLab](https://gitlab.onelab.info/gmsh/gmsh); [PyPI](https://pypi.org/project/gmsh/) |
| meshio | MIT | 5.3.5 (2024-01) | [PyPI](https://pypi.org/project/meshio/) |

### Inferences
- **Suitability ratings (researcher's assessment; interface and cost details come from general domain knowledge and were not re-verified this session).** Rating legend: H = good for headless containerised agent use; M = workable with wrappers or effort; L = GUI-centric or hard.

| Discipline | H (pip/conda or simple CLI, seconds–minutes on CPU) | M (heavier build, input-deck driven, minutes–hours) | L / exclude |
|---|---|---|---|
| Aero | XFOIL (stdin-scripted, X11 optional), AVL (+avlwrapper), AeroSandbox, NeuralFoil, OpenVSP Python API/vspscript + VSPAERO | SU2, OpenFOAM (both CLI/Docker-friendly but CPU-hours for 3D RANS), ADflow (complex build), DUST (Fortran build, dormant), FLOWUnsteady (Julia), CFL3D (dormant) | XFLR5 (GUI); FUN3D/Cart3D/OVERFLOW (not open) |
| Structures | MYSTRAN, CalculiX, pyNastran (I/O), pyfe3d, composipy, fatpack/rainflow | Code_Aster, Elmer, FEniCSx, SHARPy, MBDyn, preCICE couplings | OpenSees (licence); NASGRO/AFGROW (commercial) |
| MDO/design | OpenMDAO, Dymos, Aviary, pyCycle, OpenAeroStruct, FAST-OAD, TiGL/TiXI/CPACS (conda) | RCAIDE (AGPL, no releases), MACH-Aero stack | CEASIOMpy (≥ Feb 2026 licence forbids cloud/HPC and redistribution); SUAVE (dormant) |
| Propulsion | NASA CEA (Apache), RocketCEA, Cantera, RocketPy, RocketIsp | OpenRocket (Java, scriptable via orhelper/JPype) | T-MATS (needs MATLAB); NPSS (not open) |
| Flight dyn./control | JSBSim (pip), python-control, PX4/ArduPilot SITL (headless SITL builds) | Gazebo (GPU optional), OpenModelica, Octave, Scilab | FlightGear (visual sim, GUI) |
| Space | Orekit (orekit-jpype), Basilisk (pip bsk), SpiceyPy, skyfield/sgp4, pykep/heyoka, GMAT (console mode and Python API exist, not verified this session) | Tudat (conda), NASA 42 (C build), Trick, cFS, F Prime (builds + tooling), Open MCT (web app) | poliastro (archived; prefer hapsira only for legacy) |
| SE/safety | SysML v2 textual notation + Pilot Implementation (Jupyter kernel), reliability, ProgPy | Capella via capellambse (headless model reading/writing) | Capella GUI authoring; SCRAM (dormant) |
| CAD/mesh | CadQuery, build123d, Gmsh, OCCT (via Python bindings) | FreeCAD (FreeCADCmd headless) | — |

- **Ground-truth pedigree is uneven.** The best-validated open codes for graded tasks are XFOIL and AVL (decades of use), SU2 and OpenFOAM (extensive workshop participation), CFL3D (the canonical TMR reference code), NASA CEA (the reference implementation itself), Aviary (validated against GASP/FLOPS outputs per its README), JSBSim, GMAT and Orekit. Tasks can be graded against these codes' outputs only when the code itself has been validated against experiment for that regime.
- **Activity risk.** Several "classic" tools are dormant or fragile: DUST (no GitLab release since 2022), CFL3D (no push since 2022), SUAVE, poliastro (archived), hapsira, proptools, SCRAM and pyBEMT. Benchmark containers should pin exact versions or commits and vendor the sources.
- **Licence hygiene.** The most container-friendly licences are Apache-2.0, MIT, BSD and ISC. GPL tools (XFOIL, AVL, CalculiX, Code_Aster, ArduPilot, Gmsh, FAST-OAD, RocketCEA) can be redistributed in images if the corresponding source is offered. AGPL tools (RCAIDE, Nyx) add obligations if they are exposed as a network service. NOSA 1.3 (OpenVSP, Trick, NASTRAN-95, 42, ProgPy) permits redistribution but has its own terms; its OSI/GPL-compatibility status was not checked this session.

### Gaps
- Interface details (exact Python APIs, CLI flags, Docker images), install difficulty and compute cost were not verified tool by tool this session. They are given above as engineering judgement only.
- RCAS (U.S. Army) licence and availability: no primary source was retrieved. NDARC's own NASA-catalog release type was not retrieved; only the RCOTOOLS wrapper entry was.
- XFOIL 6.99 release date, OpenVSP's exact latest release number and date, MBDyn's latest stable version (only Debian's libmbc 1.7.3 was seen), Salome's current version and Papyrus status were not confirmed.
- Scilab's licence was not returned by the GitLab API. The composipy and py-fatigue licences are not stated on PyPI.
- The NASA SPICE Toolkit's own (NAIF) licence and distribution terms were not fetched. Only the SpiceyPy wrapper was verified.
- GMAT's console/Python API availability in R2026a was not verified.
- Whether a more recent DUST development branch exists outside `gitlab.com/dust_group/dust` is unknown.
- Whether any CEASIOMpy commit or tag before 2026-02-05 can be pinned as Apache-2.0 was only partly confirmed. The LICENSE history shows Apache-2.0 before the "redef" commit of 2026-02-05, but the tags v1.9.9 and v2.0.0 both date from 2026-02-24.

---

## (B) Public reference data and validation cases

### Takeaway
High-quality, freely accessible ground truth exists for CFD (NASA TMR, now CC0 on GitHub; AIAA DPW/HLPW/AePW/SBPW workshop cases on the NASA CRM), for prognostics (NASA PCoE C-MAPSS/N-CMAPSS and 19 other datasets), for safety and operations (NTSB database downloads, NASA ASRS, EASA AD tool), for spacecraft operations (ESA-ADB, CC BY 3.0 IGO), and for regulations (EASA CS-25 Easy Access Rules, free and in XML). Rotorcraft ground truth is thinner. HART II is publicly released worldwide through DLR. The full UH-60A Airloads database is restricted to "qualified users", although its many NASA reports are public. Licensing is clearest for U.S.-government works (no U.S. copyright) and for CC0/CC-BY data. UIUC airfoil coordinates carry no stated licence.

### Cited Findings
**CFD verification/validation**
- **NASA Turbulence Modeling Resource (TMR).** TMR describes itself as "the authoritative resource for CFD developers looking to implement and verify turbulence models". It contains turbulence and transition model definitions, verification and validation cases, experimental data and DNS/LES datasets. It is maintained by NASA researchers (contacts E. Vogel and C. Pederson) with the Turbulence Model Benchmarking Working Group. The site "relocated to GitHub at https://tmbwg.github.io/turbmodels/" — [NASA TMR page](https://www.nasa.gov/nasa-turbulence-modeling-resource/).
- The TMR GitHub repository `TMBWG/turbmodels` is licensed **CC0-1.0** (last push 2026-09-02) — [GitHub TMBWG/turbmodels](https://github.com/TMBWG/turbmodels).
- The TMR verification cases with grids are VERIF/2DZP (zero-pressure-gradient flat plate), 2DCJ (coflowing jet), 2DB (bump-in-channel), 2DANW (airfoil near-wake), 2DMEA (multi-element airfoil) and 3DB (3D bump). The validation section includes the NASA hump — [TMR on GitHub Pages](https://tmbwg.github.io/turbmodels/); [NASA TMR page](https://www.nasa.gov/nasa-turbulence-modeling-resource/).
- **AIAA Drag Prediction Workshop (DPW-8) and Aeroelastic Prediction Workshop (AePW-4).** These are co-hosted at AIAA Aviation 2026 in San Diego with 7 working groups. DPW-8 uses NASA CRM wing/body/[pylon/nacelle] configurations. Its buffet test cases are Test Case 2, a rigid CRM with unsteady CFD at pre- and post-buffet conditions, and Test Case 3, a dynamic CRM with unsteady CFD/FSI using a committee-supplied jig wing — [DPW site](https://aiaa-dpw.larc.nasa.gov/index.html); [DPW SciTech 2025 tag-up](https://aiaa-dpw.larc.nasa.gov/ref/scitech_2025.pdf); [AePW-4 SciTech 2025 tag-up](https://nescacademy.nasa.gov/workshop/AePW4/Slides/all/SciTech%20Tagup%202025.pdf).
- AePW-4 High-Angle working group: the mandatory cases are 3D flutter at M=0.80 with α swept 0–6° and 2D BSCW flutter at M=0.80, α 0–6°. The BSCW wing was to be retested in the NASA Transonic Dynamics Tunnel in September 2025 for flutter and buffet data — [AePW-4 High-Angle slides](https://nescacademy.nasa.gov/workshop/AePW4/Slides/high_angle/AePW-4_HighAngle_June13_Website.pdf); [AePW-4 April 2025](https://nescacademy.nasa.gov/workshop/AePW4/Slides/high_angle/AePW-4_April2025.pdf).
- **High Lift Prediction Workshop.** HLPW-5 test-case documents (v1.7/v1.9) and the 2024 ICAS summary paper are public. The summary paper lists possible future directions (revisiting earlier cases, icing, landing gear, aeroelastic deformation). No HLPW-6 test-case definition was found — [HLPW5 Test Cases v1.9](https://hiliftpw.larc.nasa.gov/Workshop5/Documents/HLPW5_Test_Cases_v1.9.pdf); [ICAS 2024 Rumsey paper](https://hiliftpw.larc.nasa.gov/Workshop5/Publications/ICAS-2024-1217-rumsey-hilift_finalpaper.pdf).
- **NASA Common Research Model (CRM).** The site is titled "providing data worldwide". It offers High-Speed CRM geometry (DPW-6 and DPW-7 geometries, IGES, STEP and original CAD files) — [CRM site](https://commonresearchmodel.larc.nasa.gov/).
- **AIAA Sonic Boom Prediction Workshop (SBPW-2).** Cases were an axisymmetric equivalent-area body, a JAXA wing-body and a NASA low-boom configuration with flow-through nacelles, all at M=1.6, α=0°, Re=5.7×10⁶/m. Participants used workshop-provided uniformly refined grid families. Summary papers are on NTRS — [NTRS 20200002353](https://ntrs.nasa.gov/citations/20200002353); [NTRS 20170006501](https://ntrs.nasa.gov/citations/20170006501).

**Airfoil and report archives**
- **UIUC Airfoil Coordinates Database.** About 1,650 airfoils in Selig `.dat` format (plus DXF/DWG). The zip was updated 2026-02-23. No explicit licence or terms of use were found on the page — [UIUC coord database](https://m-selig.ae.illinois.edu/ads/coord_database.html).
- **NASA NTRS / NACA reports.** "Generally, United States government works … are not protected by copyright in the U.S. (17 U.S.C. §105)". However, "U.S. government works may contain privately created, copyrighted works", and incorporating them "does not place the private work in the public domain" — [NASA STI disclaimers/copyright](https://sti.nasa.gov/disclaimers/). NASA media guidance likewise notes that third-party copyrighted items are marked as such — [NASA images & media guidelines](https://www.nasa.gov/nasa-brand-center/images-and-media/).
- **AGARD reports.** NATO STO hosts AGARD reports, lecture series and AGARDographs as free PDFs (e.g., AGARD-R-804, AGARD-AR-319, AGARD-LS-208) — [STO AGARD-R-804](https://www.sto.nato.int/publications/AGARD/AGARD-R-804/AGARDR804.pdf); [STO AGARD-LS-208](https://www.sto.nato.int/publications/AGARD/AGARD-LS-208/AGARD-LS-208-ALL.pdf).

**Prognostics, health management and telemetry**
- **NASA PCoE Prognostics Data Repository.** It lists 21 datasets, including Turbofan Engine Degradation Simulation (C-MAPSS), Turbofan-2 (N-CMAPSS), CFRP composites run-to-failure, Fatigue Crack Growth in Aluminum Lap Joint, HIRF Battery (aircraft), Small Satellite Power Simulation, Li-ion batteries, bearings, IGBT/MOSFET aging and the PHM08 challenge. Terms: acknowledge the repository and donors, and "Users employ the data at their own risk" — [NASA PCoE repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/). Mirrors: [PHM Society mirror](https://data.phmsociety.org/nasa/); N-CMAPSS zip on the [PHM Society S3 bucket](https://phm-datasets.s3.amazonaws.com/NASA/17.+Turbofan+Engine+Degradation+Simulation+Data+Set+2.zip) (URL taken from search results, not downloaded).
- **ESA Anomaly Detection Benchmark (ESA-ADB).** Real telemetry from 3 ESA missions (ESA-Mission1/2/3 zips), published on Zenodo 2024-06-25 under CC BY 3.0 IGO — [arXiv 2406.17826](https://arxiv.org/abs/2406.17826v1); [Zenodo record 12528696](https://doi.org/10.5281/zenodo.12528696).
- **NASA SMAP/MSL telemetry anomaly data (telemanom).** The repository licence is a Caltech copyright assertion, "All rights reserved". Redistribution terms need checking before data is re-hosted — [GitHub khundman/telemanom](https://github.com/khundman/telemanom).

**Safety, accident and operational data**
- **NTSB.** Downloadable datasets: `avall.zip` (1982–present, MS Access MDB, updated monthly with weekly change files) and `PRE1982.zip`, plus coding manuals and data definitions. Recent cases are in CAROL — [NTSB Aviation Accident Database](https://www.ntsb.gov/Pages/AviationQuery.aspx); [NTSB data help](https://www.ntsb.gov/Pages/AviationQueryHelp.aspx).
- **NASA ASRS.** Voluntary, confidential reports, de-identified (names removed, dates and times generalised). The database is publicly searchable online with export to Word, Excel and CSV — [ASRS Database Online](https://asrs.arc.nasa.gov/search/database.html); [ASRS database overview](https://asrs.arc.nasa.gov/overview/database.html).
- **EASA Safety Publications Tool.** Public access to ADs, proposed ADs, emergency ADs and Canadian ADs: "a total of 17,422 publications", searchable, with PDF downloads. Registration is optional (for notifications) — [ad.easa.europa.eu](https://ad.easa.europa.eu/).
- **EASA CS-25 Easy Access Rules.** Free download as PDF, online and XML. Latest is Amendment 27; files replaced 30 Jan 2023 — [EASA Easy Access Rules CS-25](https://www.easa.europa.eu/en/document-library/easy-access-rules/easy-access-rules-large-aeroplanes-cs-25).

**Rotorcraft test data**
- **HART II (DLR/NASA/ONERA/US Army, 2001 test).** It covers rotor loads, blade motion, blade pressures, acoustics and flow-field (PIV) data. "A sub-set of these were released for public access worldwide". The workshop ran 2005–2012. DLR resumed hosting in July 2022 after NASA's site closed (~2018) and DLR's FTP site shut down (spring 2022) — [DLR HART II workshop](https://www.dlr.de/en/site/hart-ii/about-hart-ii/hart-ii-international-workshop); [HART II site](https://www.dlr.de/en/site/hart-ii).
- **UH-60A Airloads (NASA/Army, 31 flights, Aug 1993–Feb 1994).** The flight data sit in an electronic database at NASA Ames "for qualified users". Access requires approval for both the host and the database (stored in TRENDS). Many program notes and TMs are public, e.g., ON 1999-02 "Data Base Description" and Kufeld TM-2019-220014 — [UH-60 Airloads program page](https://rotorcraft.arc.nasa.gov/Research/Programs/uh_60_program.html); [ON 1999-02](https://rotorcraft.arc.nasa.gov/Research/Programs/UH-60%20pdfs/1999-02.pdf); [Kufeld TM-2019-220014](https://rotorcraft.arc.nasa.gov/Publications/files/Kufeld_TM_2019_220014_Vol_I_Final.pdf).

### Inferences
- **Best "drop-in" graded tasks.**
  - TMR verification cases (CC0, with grids and reference solutions from multiple codes) suit "run SU2/OpenFOAM/CFL3D with SA/SST and match skin friction and drag within a tolerance" tasks.
  - CRM/DPW geometry and published workshop statistics suit "predict CL/CD/CM, report grid-convergence" tasks. Grade against the workshop median and scatter band, not a single number.
  - C-MAPSS/N-CMAPSS suit RUL-estimation tasks with standard RMSE/score metrics.
  - NTSB/ASRS/EASA AD records suit operations, maintenance and certification reasoning tasks, such as AD applicability, compliance intervals and causal-factor coding.
  - CS-25 XML suits requirement-tracing tasks.
- Workshop data is "trustworthy" mainly as **ensemble** statistics. Single-code answers can disagree by more than the experimental uncertainty, so graders should use tolerance bands derived from the workshop scatter or the experiment.
- The UIUC database has no stated licence and NASA SMAP/MSL data carries "All rights reserved". Prefer linking and fetching at runtime, or obtaining permission, over re-hosting.
- HART II is the most redistributable high-quality rotorcraft validation set found. For UH-60A, the published NASA reports, which are U.S.-government works, can ground tasks even though the raw database is gated.

### Gaps
- Several items were not verified with primary sources this session: the ONERA M6 wing data location and terms; the FAA Dynamic Regulatory System (TCDS/AD) terms and bulk-download options; FAA AD/TCDS data formats; the NASA Sonic Boom workshop case files' direct download location; DPW-8/AePW-4 final results publication status; and whether HLPW-6 has been announced.
- The HART II site's individual data pages render via JavaScript. Exact access mechanics (direct download vs registration) and the redistribution terms for the public subset could not be read.
- Launch-vehicle and spacecraft public data (NASA SP reports, launcher user guides such as Ariane/Falcon payload user's guides) were not surveyed this session, including their copyright: manufacturer user guides are typically copyrighted, while NASA SPs are U.S.-government works.
- Licences of ESA/NASA telemetry datasets other than ESA-ADB and telemanom were not surveyed.

---

## (C) Legal: export control (ITAR/EAR/EU dual-use), licences, copyright of standards

### Takeaway
Under U.S. rules, open-source software and data already published without restriction are generally outside the EAR, explicitly including internet posting (15 CFR 734.7). The ITAR "public domain" definition (22 CFR 120.34) has **no internet-posting route**. New ITAR technical data becomes public only through the listed channels, e.g., government-approved public release or U.S. university fundamental research. A benchmark that *authors new* detailed design content for defence articles (launch vehicles, missiles, military aircraft) therefore carries more risk than one that re-uses already-published material. The EU's General Technology Note likewise exempts technology "in the public domain" and basic scientific research. On standards: EASA CS-25 and U.S. government publications are free to reuse; ECSS is free after registration under a licence agreement; RTCA DO-178C and SAE ARPs are sold.

### Cited Findings
- **ITAR §120.34(a)** defines "public domain" as information "published and … generally accessible or available to the public" through: (1) sales at newsstands and bookstores; (2) unrestricted subscriptions; (3) second-class mailing; (4) public libraries; (5) patents; (6) unlimited distribution at a conference, meeting or trade show "generally accessible to the public, in the United States"; (7) public release "after approval by the cognizant U.S. Government department or agency"; and (8) fundamental research at accredited U.S. institutions of higher learning. University research is excluded if the university accepts publication restrictions or if the research is U.S.-government funded with specific access and dissemination controls. Paragraph (b) is "[Reserved]" — [22 CFR 120.34 (LII)](https://www.law.cornell.edu/cfr/text/22/120.34).
- **EAR §734.7(a)**: unclassified technology or software is "published" and so not subject to the EAR when "made available to the public without restrictions upon its further dissemination". The listed routes include "(4) Public dissemination … including posting on the Internet on sites available to the public". Exceptions are 5D002 encryption software (§734.7(b)) and firearm production files (§734.7(c)) — [15 CFR 734.7 (LII)](https://www.law.cornell.edu/cfr/text/15/734.7).
- **EAR §734.8**: technology or software "that arises during, or results from, fundamental research and is intended to be published is not subject to the EAR". If researchers decide to keep results restricted or proprietary, those results become subject to the EAR — [15 CFR 734.8 (LII)](https://www.law.cornell.edu/cfr/text/15/734.8).
- **EU Regulation 2021/821, General Technology Note.** "In the public domain" means "available without restriction upon further dissemination (no account being taken of restrictions arising solely from copyright)", and basic scientific research is exempt. The intention to publish does not by itself make controlled technology public, and pre-publication sharing (e.g., peer review abroad) may need a licence — [University of Liverpool export-control exemptions](https://www.liverpool.ac.uk/legal/exportcontrols/exemptionsfortheacademiccommunity/); [Imperial College exemptions](https://www.imperial.ac.uk/research-and-innovation/research-office/research-security/research-security-legislation/export-controls/do-i-need-an-export-licence/exemptions/); regulation text at [EUR-Lex 2021/821](https://eur-lex.europa.eu/eli/reg/2021/821/oj).
- **U.S.-only NASA codes** (FUN3D: "U.S. Person" required; Cart3D/OVERFLOW: "U.S. Release Only") cannot be in a globally rerunnable benchmark — [FUN3D request page](https://fun3d.larc.nasa.gov/chapter-1.html); [Rescale/NASA](https://rescale.com/?p=82366).
- **Copyright of U.S.-government works**: there is no U.S. copyright under 17 U.S.C. §105, but embedded third-party material stays protected — [NASA STI copyright notice](https://sti.nasa.gov/disclaimers/).
- **Standards access.**
  - EASA CS-25 Easy Access Rules: free (PDF/online/XML) — [EASA](https://www.easa.europa.eu/en/document-library/easy-access-rules/easy-access-rules-large-aeroplanes-cs-25).
  - ECSS: registration required, and users agree to the ECSS licence agreement/disclaimer; standards download as PDF/Word — [ecss.nl](https://ecss.nl/organization).
  - RTCA DO-178C/DO-278A/DO-248C: sold through the RTCA store, with member pricing — [RTCA DO-178 page](https://www.rtca.org/do-178/).
  - SAE ARP4754B/ARP4761A: price pages could not be retrieved (see Gaps).
  - OMG SysML 2.0: formal spec, Sept 2025, Non-Assert IPR mode — [OMG](https://www.omg.org/spec/SysML/).
- **Dataset licences.** TMR is CC0-1.0 ([GitHub](https://github.com/TMBWG/turbmodels)); ESA-ADB is CC BY 3.0 IGO ([arXiv](https://arxiv.org/abs/2406.17826v1)); NASA PCoE data requires acknowledgement and is used at the user's own risk ([NASA PCoE](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)); the telemanom repo is "All rights reserved" ([GitHub](https://github.com/khundman/telemanom)).
- **Software licence red flags.** CEASIOMpy forbids "Cloud & HPC Deployment" and "Redistribution" since Feb 2026 ([LICENSE](https://github.com/cfsengineering/CEASIOMpy/blob/main/LICENSE)). OpenSees is non-commercial only ([LICENSE](https://github.com/OpenSees/OpenSees)). Syside (SysML v2) is proprietary ([PyPI](https://pypi.org/project/syside/)).

### Inferences
- **Design rule for task authors (not legal advice).**
  1. Build tasks only from material that is already published without dissemination restrictions: NTRS public-release reports, workshop cases, open-source code docs, EASA/FAA public regulations.
  2. Do not generate novel, detailed, design-to-build content for items likely on the USML (e.g., rocket/missile propulsion and guidance, reentry vehicles, military aircraft) or high-sensitivity EAR/MTCR items.
  3. Keep tasks at textbook or conceptual-design fidelity in those areas.
  4. Have export counsel review the launch-vehicle and spacecraft propulsion/GNC subsets before publication, because ITAR §120.34 offers no "we posted it online" safe harbour.
- Because the EU GTN ignores copyright when judging "public domain", copyrighted but freely available material can be export-control-free and still not be redistributable. The two analyses (export control and copyright) must be done separately.
- For standards-dependent tasks (DO-178C objectives, ARP4761A safety-assessment methods), the benchmark should either (a) restrict itself to free sources (EASA CS/AMC, FAA ACs and orders, eCFR 14 CFR, NASA-STD/NASA-HDBK, ECSS for space, MIL-HDBKs that are public), or (b) write tasks that require only general, widely published knowledge of the standard and never reproduce its text. Graders must not depend on paywalled clause text.
- Prefer Apache/MIT/BSD/ISC tools in the reference container. Include GPL tools with source offers. Avoid AGPL tools in any hosted grading service unless their source is also served. Exclude CEASIOMpy ≥ Feb 2026, OpenSees (if any commercial party will rerun the benchmark) and anything U.S.-only.

### Gaps
- The exact USML category text (Cat IV launch vehicles/missiles, Cat VIII aircraft, Cat XV spacecraft) and EAR ECCNs most relevant to aerospace task content (e.g., 9E003 gas-turbine hot-section technology, 9A515 spacecraft, 9A012 UAVs, MTCR Category I) could not be retrieved before the search budget ran out. These need primary-source confirmation.
- SAE ARP4754B/ARP4761A prices and licence terms, and MMPDS (successor to the public MIL-HDBK-5) access terms, were not retrieved.
- ECSS licence agreement text (whether derived works or quotation in benchmark tasks are allowed) was not read.
- Whether NOSA 1.3 is OSI-approved and GPL-compatible was not verified this session.
- The UIUC airfoil data licence is unknown (no statement found).

---

## (D) Coverage gaps where no adequate open-source tool exists

### Takeaway
The biggest open-source gaps are:
- **rotorcraft comprehensive analysis** (CAMRAD II/NDARC/RCAS-class trim, aeroelastic and loads codes);
- **fracture mechanics and damage tolerance** for certification (NASGRO/AFGROW);
- **industrial engine cycle decks** (NPSS);
- **Nastran-class dynamic aeroelasticity** (flutter/gust SOL 145/146-type), since MYSTRAN covers only part of Nastran;
- **certified-process software and safety standards** (DO-178C/ARP4754B/ARP4761A texts are paywalled);
- the **U.S.-only NASA CFD codes** (FUN3D, Cart3D, OVERFLOW).

Partial open substitutes exist for most of these (DUST+MBDyn, FLOWUnsteady, OpenFAST, pyCycle, SHARPy/OpenAeroStruct, fatpack/py-fatigue, SU2/OpenFOAM/CFL3D/ADflow). Their validation pedigree is thinner, so tasks in these areas should be scoped to what the open tools can credibly ground.

### Cited Findings
- Rotorcraft: NDARC and CAMRAD II are accessed in NASA's open tooling only through wrappers (RCOTOOLS) — [NASA RCOTOOLS](https://software.nasa.gov/software/ARC-18184-1). Open mid-fidelity options are DUST (MIT, last release 2022) — [DUST](https://www.dust.polimi.it/), [GitLab](https://gitlab.com/dust_group/dust); MBDyn (GPL-2.1) — [listing](https://www.linuxlinks.com/mbdyn-multibody-dynamics-analysis/); and FLOWUnsteady (MIT) — [GitHub](https://github.com/byuflowlab/FLOWUnsteady).
- Fatigue/fracture: NASGRO v11.1 (Sept 2025) is commercially licensed (royalty-free only to NASA/FAA/ESA/consortium) — [SwRI](https://www.swri.org/nasgro). AFGROW 5.4 costs US$1,650 — [afgrow.net](https://afgrow.net/). Open libraries cover cycle counting and S-N damage only (fatpack, rainflow per ASTM E1049-85, py-fatigue) — [fatpack](https://pypi.org/project/fatpack/), [rainflow](https://pypi.org/project/rainflow/), [py-fatigue](https://pypi.org/project/py-fatigue/).
- Engine cycle: NPSS is maintained by SwRI with an NPSS Consortium — [SwRI NPSS](https://www.swri.org/npss). The open alternative is pyCycle (Apache-2.0) — [GitHub](https://github.com/OpenMDAO/pyCycle). NASA's open T-MATS needs MATLAB/Simulink — [GitHub](https://github.com/nasa/T-MATS).
- Structures: MYSTRAN lists Nastran compatibility, linear statics and linear buckling — [MYSTRAN README](https://github.com/MystranSolver/MYSTRANSolver). pyNastran is I/O only — [GitHub](https://github.com/SteveDoyle2/pyNastran).
- CFD: FUN3D requires U.S.-person status — [FUN3D](https://fun3d.larc.nasa.gov/chapter-1.html). Cart3D and OVERFLOW are "U.S. Release Only" — [Rescale/NASA](https://rescale.com/?p=82366).
- MBSE: Capella (EPL-2.0) and the SysML v2 Pilot Implementation (EPL-2.0) are open — [Capella](https://github.com/eclipse-capella/capella), [SysML v2 Pilot](https://github.com/Systems-Modeling/SysML-v2-Pilot-Implementation). A notable SysML v2 Python automation library (Syside) is proprietary — [PyPI](https://pypi.org/project/syside/).
- Conceptual design workflow: CEASIOMpy is no longer open as of Feb 2026 — [LICENSE](https://github.com/cfsengineering/CEASIOMpy/blob/main/LICENSE).

### Inferences
Gap matrix: work types, the industry tools normally used (named from general domain knowledge, not verified this session unless cited above), and the best open substitutes.

| Work type | Typical industrial tool | Open substitute | Adequacy for graded tasks |
|---|---|---|---|
| Rotorcraft trim/loads/aeroelastic stability | CAMRAD II, RCAS, Dymore, FLIGHTLAB | DUST+MBDyn, FLOWUnsteady, OpenFAST/CCBlade, BEMT scripts | Medium-low: hover/forward-flight performance OK; full trim + loads weak |
| Rotorcraft sizing | NDARC | RCAIDE (AGPL), custom OpenMDAO | Low-medium |
| Fracture / damage tolerance | NASGRO, AFGROW | fatpack/py-fatigue + hand-coded LEFM (Paris/NASGRO equation, published SIF solutions) | Low for certification-grade; OK for textbook crack growth |
| Dynamic aeroelasticity / flutter / gust loads | MSC Nastran SOL 145/146, ZAERO | SHARPy, OpenAeroStruct (static), Code_Aster + coupling, preCICE + SU2 | Medium-low |
| Engine performance decks | NPSS, GasTurb, PROOSIS | pyCycle, Cantera, NASA CEA | Medium (design point and off-design cycle OK) |
| High-fidelity unstructured/overset CFD | FUN3D, OVERFLOW, Fluent/STAR-CCM+, CFD++ | SU2, OpenFOAM, ADflow, CFL3D | Good, but compute-heavy (needs CPU-hours budget) |
| Mission/space analysis | STK, FreeFlyer | GMAT, Orekit, Basilisk, Tudat, SPICE | Good |
| CAD/PLM, composites layup, CAM | CATIA, NX, Fibersim, Teamcenter | FreeCAD, CadQuery, build123d, composipy | Medium for parametric CAD; poor for PLM, composites manufacturing and CAM |
| Certification/safety process (DO-178C, ARP4754B/4761A, DO-254) | Commercial tools + paywalled standards | SysML v2 pilot, Capella, SCRAM (dormant), reliability | Medium for FTA/FMEA math; low for standard-clause compliance |
| Missile/aero prediction handbooks | Missile DATCOM etc. (export-sensitive) | Digital DATCOM-class public methods, AeroSandbox | Avoid for export reasons |
| Flight-test data reduction | Proprietary/in-house | python-control, SciPy-based sysid, JSBSim | Medium; flight-data sources limited |

- Pilot recommendation: scope rotorcraft, fracture and aeroelastic-flutter tasks to closed-form or textbook-verified sub-problems, or to HART II/UH-60A report comparisons, rather than full-industrial analyses. Put the CPU-heavy CFD (SU2/OpenFOAM on DPW/HLPW grids) in an optional "heavy" tier.

### Gaps
- The proprietary tools named in the gap matrix (FLIGHTLAB, Dymore, ZAERO, GasTurb, PROOSIS, CFD++, FreeFlyer, Fibersim) were not checked against primary sources this session. They are listed from domain knowledge as typical industrial choices.
- No open-source tool for DO-254 hardware assurance, or for aircraft maintenance planning (MSG-3, MRO optimisation), was searched for or identified.
- Open-source structural sizing and stress-check tools (e.g., margin-of-safety calculators based on MIL-HDBK-5/MMPDS allowables) were not surveyed. MMPDS itself is believed to be paywalled (unverified).
