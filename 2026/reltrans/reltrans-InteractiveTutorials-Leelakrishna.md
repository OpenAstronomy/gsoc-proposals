# Application Title
GSoC 2026 Proposal: Interactive Tutorials and Automated Testing for Reltrans (Leelakrishna)

## Contributor Information
* **Name:** Leelakrishna Rajasimha Yadav Doddakula
* **Time-zone:** IST(Indian Standard Time(UTC + 5:30))
* **Matrix/slack/IRC Handle:** Leelakrishna (Slack)
* **Github/forge username:** leelakrishnaRajasimha
* **Blog:** N/A
* **Blog RSS feed:** N/A
* **PR link(s):**
  * . https://github.com/reltrans/reltrans/pull/117

### Background
My name is Leelakrishna Rajasimha Yadav Doddukula (Leelakrishna in short). I am an 18 year old , 2 year BTech student at NIT Nagpur (VNIT),INDIA. I am a Python developer with a deep interest in how high-performance scientific code like the Fortran 95/2003 cores used in astrophysics can be made more accessible through modern Pythonic wrappers .
I consider myself a fast, independent learner who enjoys digging into the "inner workings" of a codebase to understand how data flows from a numerical engine to a user's plot. I am particularly interested in software sustainability ensuring that complex research tools remain stable and easy for new students to learn through better testing and interactive documentation.

### Interest in OpenAstronomy
I am deeply motivated by the OpenAstronomy mission to provide high-quality, sustainable open-source software for the astrophysics community. I am choosing Reltrans over other projects because I believe that reproducible software is the only way to advance modern black hole research. My goal is to help the next generation of researchers see the physics clearly, without being blocked by compilation errors or undocumented code. I want to ensure that complex relativistic models are as stable and easy to use as standard Astropy tools.


## Project Proposal Application
**Proposal Title:**
Interactive Tutorials and Automated Testing for the Reltrans Code

**Organisation:**
OpenAstronomy (Reltrans)

### **Summary:**
The primary objective of this project is to transform the Reltrans relativistic X-ray framework from a specialized research tool into a robust, self-verifying, and accessible software ecosystem. Reltrans serves as a critical computational bridge for X-ray reverberation mapping, using a lamp-post geometry to model the spacetime surrounding black holes. However, because the core is 95% Fortran, it currently presents a steep learning curve and significant maintenance risks. To address this, the project implements a dual-layer infrastructure: Interactive Tutorials to provide executable documentation and a Pro-Level Automated Testing suite to ensure numerical integrity.

### Deliverables
**1.** Interactive Tutorial Suite: A series of 8-10 Jupyter notebooks covering workflows from basic installation to advanced spectral-lag analysis.

**2.** Automated Testing Hierarchy: A comprehensive pytest framework including unit tests for Python-Fortran bindings and physics-validation for coordinate distance and light-travel time constraints.

**3.** Visual Regression Suite: Implementation of pytest-mpl to compare generated lag-energy plots against validated baselines, preventing subtle "physics drift" in model outputs.

**4.** Continuous Integration (CI) Pipeline: A GitHub Actions workflow that automatically builds the HEASoft/XSPEC environment and executes the full test suite on every pull request.

**5.** Environment Auditor (Quick-Start): A CLI-based tool called reltrans-check that detects a user’s local configuration and generates customized setup scripts to reduce onboarding friction.

### Description/timeline
| Phase / Week | Technical Goals & Key Deliverables |
| :--- | :--- |
| **Phase 1: Foundation** | **Software Engineering & Wrapper Audit** |
| Week 1 | Environment Setup: Audit HEASoft/XSPEC integration; verify gfortran compatibility for Fortran 95/2003 cores. |
| Week 2 | Numerical Baseline: Re-implement canonical "test-case" calls in Python; implement bit-perfect verification for numerical kernels. |
| Week 3 | Testing Infrastructure: Initialize pytest framework; develop the `reltrans-check` CLI auditor tool. |
| **Phase 2: Pedagogy** | **Interactive Tutorial Development** |
| Week 4 | Core Tutorials: Map API surface; develop tutorials for Installation, Basic Model Calling, and Iron Kα visualization. |
| Week 5 | Science Workflows: Create tutorials for X-ray Reverberation Mapping and Lamp-Post Geometry parameter sweeps. |
| Week 6 | Cloud Compatibility: Finalize "Learn Reltrans" structure; ensure MyBinder and JupyterLite compatibility. |
| **MID Evaluation** | **Assessment of Phase 1 & 2 Deliverables**  |
| **Phase 3: Rigor** | **Automated Verification & Visual Regression** |
| Week 7 | Property-Based Testing: Implement Hypothesis routines to verify physics invariants and parameter bounds. |
| Week 8 | Visual Regression: Integrate `pytest-mpl`; generate validated baseline images for all tutorial plots. |
| Week 9 | Notebook Testing: Implement `nbval` in the CI pipeline to ensure tutorial notebooks execute without errors. |
| **Phase 4: Integration** | **CI Automation & Community Handoff** |
| Week 10 | Performance Profiling: Configure multi-tier CI pipeline; perform computational latency benchmarking for Python wrappers. |
| Week 11 | Environment Hardening: Test environment auditor across Linux/macOS; finalize Read The Docs rendering. |
| Week 12, 13 | Stress Testing & Final Buffer: Validate extreme physical edge-cases (e.g., $a \rightarrow 0.998$); submit final report and production code. |

#### Weekly Project Breakdown:
Phase 1: Deep Software Engineering & Wrapper Audit (Weeks 1–3)

Week 1: High-Throughput Ingestion & Environment Setup

I will perform a full system audit of the HEASoft/XSPEC integration, focusing on ensuring the Fortran 95/2003 core compiles consistently across Linux and macOS environments.
I will refine the data-loading pipeline using uproot and awkward-array to efficiently handle large simulated Reltrans spectral datasets without memory overhead.

Week 2: Numerical Baseline Extraction & Verification

I will recreate baseline lag-energy models to visualize the effects of varying black hole spin on the Iron Kα line.
I will implement bit-perfect verification between the raw Fortran outputs and the Python wrappers to ensure that the "handshake" between legacy kernels and modern interfaces is mathematically sound.

Week 3: The Environment Auditor & CLI Development

I will develop the reltrans-check CLI tool, which will automatically audit a user's local configuration (detecting gfortran, RELTRANS_PATH, and XSPEC paths) to generate customized installation scripts and reduce onboarding friction.

Phase 2: Pedagogy & Interactive Documentation (Weeks 4–6)

Week 4: API Mapping & Core Tutorials

I will map the full Python-Fortran API surface to identify and document all available physics parameters within the core subroutines.
I will develop the first three Jupyter tutorials: "Installation & Environment Setup," "Basic Model Calling," and "Visualizing the Iron Kα Line Profile".

Week 5: Advanced Science Workflows & Parameter Sweeps

I will create advanced tutorials for X-ray Reverberation Mapping and Lamp-Post Geometry, implementing parameter sweeps to teach researchers how to estimate black hole spin (a) and corona height (h).

Week 6: Tutorial Hardening & Cloud Compatibility

I will ensure all tutorials are fully compatible with MyBinder and JupyterLite for zero-install, browser-based execution, following the "Learn Astropy" pedagogical model.

Phase 3: Automated Rigor & Visual Verification (Weeks 7–9)

Week 7: Property-Based Testing & Bound Validation

I will implement property-based testing using the Hypothesis library to verify physics invariants across a wide parameter space (e.g., ensuring |a| < 1 and h > R_h).

Week 8: Visual Regression Suite (pytest-mpl)

I will integrate pytest-mpl to generate validated baseline images for all canonical tutorial plots, ensuring that future code updates never subtly distort the science output.

Week 9: CI Pipeline Integration & Notebook Verification

I will implement nbval in the GitHub Actions CI pipeline to verify that every tutorial notebook executes correctly and yields the expected numerical results on every commit.

Phase 4: Integration & Community Handoff (Weeks 10–13)

Week 10: Performance Profiling & Documentation

I will perform computational latency benchmarking on the Python wrappers to ensure they are optimized for large-scale parameter sweeps used in MCMC fits.

Week 11:Cross-Platform Environment Hardening

I will write a Validation Suite to run the Python-Fortran model against legacy XSPEC outputs, generating comparison plots to quantify numerical consistency.

Week 12: Edge-Case Stress Testing & Stability Monitoring

I will test the model against extreme physical edge-cases (e.g., maximally rotating black holes where a -> 0.998) to ensure production-readiness and numerical stability.

Week 13: OpenAstronomy Community Handoff

I will finalize documentation on ReadTheDocs, prepare a final report for the OpenAstronomy and Reltrans development teams, and ensure all code adheres to production-grade standards.

#### Availability and Commitment:
Total Project Commitment: 350+ Hours.

Weekly Commitment Breakdown:

1.) Monday – Friday: 8–10 hours per day. My primary focus during these days will be Fortran
Python wrapper optimization, tutorial design, and implementing the automated testing hierarchy.

2.) Saturday: ~6 hours. This day is dedicated to addressing any technical debt, debugging complex 
HEASoft/XSPEC integration errors, and finalizing the week's documentation.

3.) Sunday: Holiday / Rest Day. This ensures I remain sharp and productive for the following week's 
high-intensity tasks.

Community Bonding Period: May 1st – May 20th. During this time, I will be active on the Reltrans Slack/Matrix channels, setting up the development environment, and finalizing the project's technical requirements with my mentors.

Proposed Start Date: May 21st, 2026 (Negotiable).

I would like to propose a 'Pre-Phase' start on May 21st. Since my university exams end in mid
May, starting 10 days early allows me to build the base foundation—environment setup 
(HEASoft/XSPEC), data ingestion, and baseline reproduction independently. This ensures that by the 
time the mentors become available in June, we can dive straight into advanced automated testing 
and tutorial development without setup delays. Please note that all hours and start dates are fully 
negotiable based on the mentors' preferences and the project's evolving needs.

Communication & Logistics:

1.) Timezone Sync: I will adjust my schedule to ensure a significant daily overlap with the 
CERN/Geneva (CEST) timezone for meetings and live debugging sessions.

2.) Progress Tracking: I will maintain a private GitHub Project Board, shared with mentors, to 
provide real-time visibility into sub-tasks and milestone completion.

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time applying for Google Summer of Code.

### Are you also applying to other projects?
Yes, I have also applied to a project with CERN-HSF (ATLAS). I am equally passionate about both fields; however, my existing experience refactoring the Reltrans write_components logic (PR #117) gives me a significant head start in delivering high-quality results here . If selected for both, I will choose the project where my skills can provide the most long-term impact for the open-source scientific community .

### Schedule availability
I have no major holiday trips or long-distance trips planned during the GSoC timeline. My summer break runs from May 13th through the end of July, allowing me to focus entirely on the program. While there may be 1 or 2 days for minor personal tasks, I will seek prior permission from my mentors. Any work missed on those days will be compensated for by working extra hours during the week or on Sundays to ensure all weekly milestones are met on time .


## Other comments
### AI Usage & Academic Integrity:
I am committed to the high standards of academic and technical integrity required by the OpenAstronomy community and the Reltrans development team. My policy on Generative AI is as follows:

Theoretical Research: I may use AI as a high-level research assistant to cross-check X-ray reverberation documentation or summarize complex astrophysical papers regarding lamp-post geometry.

Core Development: All core testing frameworks, physics-validation logic, and Fortran-Python bindings will be written manually. This is mandatory to ensure 100% mathematical transparency and maintain the rigorous standards of the YNOGK ray-tracing engine.

Collaboration & Transparency: If a specific need arises to use AI for boilerplate documentation or complex debugging of HEASoft integration, I will seek explicit approval from my mentors first. Every line of assisted code will be clearly documented and verified against the official Reltrans test snapshots.