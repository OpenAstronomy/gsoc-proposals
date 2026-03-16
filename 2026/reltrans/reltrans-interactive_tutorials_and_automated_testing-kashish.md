# Project Proposal

## [Interactive Tutorials and Automated Testing for the Reltrans Code](https://openastronomy.org/gsoc/gsoc2026/#/projects?project=interactive_tutorials_and_automated_testing_for_the_reltrans_code)

**Mentors:** `fjebaker`, `mgullik`

---

## Personal Information

Field | Details
-- | --
Name | Kashish Shrivastav
GitHub | @kashish2210
Slack | [Kashish Shrivastav](https://reltrans-workspace.slack.com/team/U0AGL7BC8A0)
Matrix | [@kashish2210:matrix.org](https://matrix.org/)
Time Zone | IST (GMT + 5:30)

---

## Open Source Contributions

### Contributions to the Reltrans Software (Current Open PRs)

1. **[Variable and Function Renaming](https://github.com/reltrans/reltrans/pull/93)** — A small PR that changes variable naming conventions.
   > *What did I learn?* Column 80 for punch-card nostalgia, thanks to `fjebaker`

2. **[Script for Postprocess](https://github.com/reltrans/reltrans/pull/83)** — Converts MCMC parameter chains from the `reltransDCP` model (uses `logxi`) to the `rtdist` model (uses distance) by calculating and appending distance values for each chain step.

3. **[Error Handler](https://github.com/reltrans/reltrans/pull/81)** — Moto is to build a centralised error handler system.

4. **[Refactor — radfuncs_dist](https://github.com/reltrans/reltrans/pull/114)** — Refactors `radfuncs_dist` to use grouped arguments through the derived types `config` and `model_args`, with `fcons` kept as a separate scalar input. The long positional argument list was removed from both the subroutine definition and its call site in `genreltrans`.

5. **[Refactor: radfunction_dens](https://github.com/reltrans/reltrans/pull/113)** — This change refactors `radfunctions_dens` to use the grouped derived-type arguments `config`, `model_args`, and `arrays` instead of the previous long positional argument list. The corresponding call in `genreltrans` was updated accordingly.

---

## My Background

I was selected for **Google Summer of Code 2025** with OpenAstronomy, contributing to **Stingray.jl** — a Julia port of the Stingray X-ray timing library.

My work spanned the full data pipeline:

- Implemented key pipeline components: NICER `.evt` file reading, lightcurve generation, filtering, and power spectrum analysis.
- Built Cross-Spectrum and Averaged Cross-Spectrum analysis with plotting recipes (~5,700 LOC).
- Worked with real FITS astronomical data and implemented GTI/BTI handling.

Beyond the code, I have a genuine passion for astrophysics. I've read deeply into topics like black holes, general relativity, and the theory of everything — so working on software that analyses real X-ray signals from accreting black holes isn't just a technical task for me, it connects directly to what I find most fascinating about the universe.

This background makes me well-suited for the Reltrans project: I already understand X-ray reverberation concepts, have hands-on experience with the kind of data Reltrans models use/contain, and know what it feels like to be a new user navigating a steep learning curve in a scientific codebase — which is exactly the problem this project aims to solve.

---

## Interest in Open Astronomy

Open Astronomy's mission to advance open-source tools for astrophysical research genuinely excites me — it sits right at the intersection of two things I care deeply about: science and software. There's something special about a community that believes good tooling is as important as the science itself, and I've seen that philosophy make a real difference.

I deeply value the commitment to open science, reproducibility, and collaboration that OpenAstronomy embodies. The Reltrans project feels like a natural fit for me — not just as a chance to apply my skills, but to learn from experts, grow as a developer, and contribute to tools that help astronomers interpret some of the most extreme phenomena in the universe. I'm genuinely excited about being part of a community that's pushing the boundaries of high-energy astrophysics forward.

---

## 1.0 Project Overview

Reltrans is a semi-analytical model for X-ray reverberation mapping of accreting black holes, used across both Active Galactic Nuclei and X-ray binaries. At its core is a Fortran engine that computes time-averaged spectra and energy-dependent, Fourier-domain cross spectra — making it an essential tool for interpreting the timing signatures of black hole systems. Python wrappers around this Fortran core have made the model more accessible, but **two critical gaps** remain that this project directly addresses.

**Gap 1 — Documentation:** Right now, there are no hands-on tutorials for new users. Getting started requires configuring HEASOFT, setting environment variables like `RELTRANS_TABLES`, `RMF_SET`, and `EMIN_REF`, understanding the `ReIm` flag system, and navigating multiple model flavours like `reltransDCp`, `reltransPL`, and `rtdist` — all without a single working example notebook to follow. Issue [#60](https://github.com/wilkinsdr/reltrans/issues/60) calls for exactly this: a structured series of Python notebooks that guide users from installation through to advanced scientific workflows like PyXSPEC fitting with real data.

**Gap 2 — Testing:** The existing test suite only checks final model outputs against snapshots, with no coverage of individual subroutines. PR [#94](https://github.com/wilkinsdr/reltrans/pull/94) made this concrete — a single refactor caused a numerical drift that sparked a team-wide debate about tolerances, with no principled framework to resolve it. Issue [#59](https://github.com/wilkinsdr/reltrans/issues/59) explicitly flags that lag-energy spectra, real/imaginary cross-spectrum outputs, and lag-frequency spectra are all untested. Building a proper `pytest`-based unit testing framework, informed by the ongoing structural refactor PRs [#68](https://github.com/wilkinsdr/reltrans/pull/68) and [#94](https://github.com/wilkinsdr/reltrans/pull/94), is the second major deliverable.

Together, these two contributions — tutorials and tests — form a safety net that the codebase currently lacks, lowering the barrier for new users while giving developers the confidence to keep refactoring and extending the code.

### System Architecture Overview

<img width="443" height="557" alt="image" src="https://github.com/user-attachments/assets/9a26de7a-d605-41fc-8631-67f26840b3b8" />

---

### Summary

Reltrans is a powerful but currently hard-to-use tool for X-ray reverberation mapping of accreting black holes. New users face a steep learning curve with no example notebooks to follow, and developers face a fragile codebase with no unit test safety net.

This project fixes both:
- Build a structured series of **interactive Python tutorial notebooks** guiding users from installation through to real scientific workflows. Additionally, a `setup.sh` script so developers can run `./setup.sh` to directly download normalised tables.
- Develop a robust **`pytest`-based unit testing framework** that validates model outputs and makes future development safer.

The result will be a more accessible, more maintainable Reltrans — one that the broader high-energy astrophysics community can adopt with confidence.

---

### Technical Implementation Roadmap

> **Note:** This is not the actual fixed week-by-week plan; it reflects current understanding and will be updated with mentor input.

#### Phase 1 — Familiarisation and Setup (Weeks 1–2)

Before writing a single tutorial or test, I need to deeply understand the codebase. This means:

- Compiling Reltrans, setting up the full environment (HEASOFT, XILLVER tables, environment variables)
- Running the existing test suite
- Tracing how `f2py_interface.py` calls into the Fortran core
- Studying the model flavours (`reltransDCp`, `reltransPL`, `rtdist`, `reltransDbl`) and the `ReIm` flag system
- Mapping out the existing subroutine structure with an eye on Issue [#13](https://github.com/wilkinsdr/reltrans/issues/13), since the ongoing reorganisation directly affects how unit tests should be scoped

#### Phase 2 — Tutorial Notebooks: Core Series (Weeks 3–6)

This is the first major deliverable, directly addressing Issue [#60](https://github.com/wilkinsdr/reltrans/issues/60). Notebooks will be built progressively:

- **Notebook 1 — Installation and Environment Setup:** A complete, reproducible setup guide covering HEASOFT installation, environment variable configuration, compilation, and verifying the model runs. This notebook is designed to eliminate the friction that keeps new users from getting started. For this part, a `setup.sh` script will be implemented.

- **Notebook 2 — Basic Python Interface:** Walking through `f2py_interface.py`, how to construct the energy array, set parameters, and call `reltransDCp`. Visualising the time-averaged spectrum and building intuition for what each parameter does.

- **Notebook 3 — Exploring Model Outputs:** Demonstrating all `ReIm` flag outputs — time-averaged spectrum, real/imaginary cross spectrum, modulus, lag-energy spectrum — with plots and physical interpretation for each. Showing how outputs change as key parameters like coronal height `h`, spin `a`, and inclination `inc` are varied.

- **Notebook 4 — Model Flavour Comparison:** Covering `reltransPL`, `rtdist`, and `reltransDbl`, explaining when and why each is used, and showing side-by-side comparisons of their outputs.

- **Notebook 5 — PyXSPEC Integration:** Demonstrating how to load Reltrans as a local model in XSPEC and use PyXSPEC to fit real data products — the most advanced and scientifically crucial tutorial.

> *Note: 5 notebooks is not a fixed number — there may be more.*

#### Phase 3 — Unit Testing Framework (Weeks 7–10)

This directly addresses Issue [#59](https://github.com/wilkinsdr/reltrans/issues/59). The strategy is to build tests in layers:

- **Layer 1 — Output Coverage Tests:** Extending the existing snapshot tests to cover all currently untested outputs — lag-energy spectra, real/imaginary cross spectra, and lag-frequency spectra. This creates an immediate safety net around the outputs that matter most scientifically.

- **Layer 2 — Python Wrapper Tests:** Testing the `f2py_interface.py` layer directly — verifying parameter passing between Python and Fortran is correct, that edge cases (parameter bounds, invalid inputs) are handled gracefully, and that environment variable configuration behaves as expected. This is informed by the tolerance discussion in PR [#94](https://github.com/wilkinsdr/reltrans/pull/94), where I already suggested combining `rtol` and `atol` checks.

- **Layer 3 — Subroutine-Level Tests:** As the refactor progresses (PRs [#68](https://github.com/wilkinsdr/reltrans/pull/68) and [#94](https://github.com/wilkinsdr/reltrans/pull/94), individual subroutines become testable in isolation. Unit tests for the components that have been cleanly separated — starting with `do_convolutions` and `config_frequency`, which were the first to be extracted in the refactor.

- **Layer 4 — Regression Tests and CI Integration:** Adding regression tests that catch numerical drift from future refactoring, and ensuring the full test suite runs cleanly in the existing CI pipeline.

#### Phase 4 — Documentation for Contributors and Final Polish (Weeks 11–12)

Creative additions such as GIFs and interactive images to show how Reltrans works. The animation below was generated using `matplotlib` with the current normalised tables data and calculations from Reltrans.
> Yeah! I have used Bit AI to make this smooth animation(frames).


https://github.com/user-attachments/assets/aa2302bb-1d00-43ad-b766-e10b8214f2ee

---

## 2.0 Milestones

### Coding Starts (Community Bonding)

Before writing any code: get Reltrans fully compiled and running, trace the flow from Python wrapper through to the Fortran core, and read through the existing test suite. Review active PRs ([#68](https://github.com/wilkinsdr/reltrans/pull/68), [#94](https://github.com/wilkinsdr/reltrans/pull/94)) and open issues ([#59](https://github.com/wilkinsdr/reltrans/issues/59), [#60](https://github.com/wilkinsdr/reltrans/issues/60), [#13](https://github.com/wilkinsdr/reltrans/issues/13) to map out how the refactor affects what can be tested. Also, understand the RTFAST emulator. Draft a concrete tutorial outline and agree on testing standards with mentors before writing a single line.

### 1st Evaluation (End of ~Week 6)

By midterm:
- **Tutorials:** Working notebook series covering installation and environment setup, basic Python interface usage, and core model workflows demonstrating the key `ReIm` outputs with plots and physical context.
- **Tests:** Unit tests for the most critical Python wrapper components, output coverage tests for the currently untested products (lag-energy, cross-spectrum real/imaginary, lag-frequency), integrated into the existing CI workflow to run automatically on every PR.

### Final Evaluation (End of ~Week 12)

- Complete tutorial series — including the advanced PyXSPEC fitting notebook and model flavour comparisons — fully reviewed and merged.
- Expanded test coverage, including edge cases, parameter boundary checks, and regression tests guarding against numerical drift seen in PR [#94](https://github.com/wilkinsdr/reltrans/pull/94).
- A contributor guide explaining how to add new tutorials and extend the test suite, written so that the work is maintainable long after GSoC ends.
- Final cleanup and preparation for long-term maintenance.

---

## 3.0 Deliverables

> **Note:** These are not fixed deliverables — the final output will likely include more.

**Deliverable 1 — Complete Tutorial Series:** Python notebooks living in the `reltransDocs` repository, covering the full user journey from first-time installation through to PyXSPEC fitting with real data. Each notebook will be self-contained, reproducible, and written with the kind of physical context that helps a new user understand not just what the code does but *why* — something that's currently completely absent. These notebooks will directly resolve Issue [#60](https://github.com/wilkinsdr/reltrans/issues/60).

**Deliverable 2 — `pytest`-based Unit Testing Framework:** Extending the existing snapshot tests to cover all untested model outputs, adding wrapper-level tests that validate Python-to-Fortran parameter passing, subroutine-level tests for the components cleaned up in the ongoing refactor, and regression tests that catch numerical drift from future changes. This directly resolves Issue [#59](https://github.com/wilkinsdr/reltrans/issues/59) and provides the safety net the team debated needing during PR [#94](https://github.com/wilkinsdr/reltrans/pull/94).

**Deliverable 3 — Contributor Guide:** Documentation on how to add new tutorials and extend the test suite, so neither deliverable becomes stale the moment GSoC ends. This is something the mentors themselves flagged as important — `@fjebaker` noted in PR [#68](https://github.com/wilkinsdr/reltrans/pull/68) that code structuring decisions need documentation, and `@bjricketts` raised the need for clear standards throughout the review thread.

---

## 4.0 Pre-Community Bonding and Current Working

I've successfully:
- Compiled Reltrans and set up the HEASOFT environment
- Run the existing test suite
- Traced how `f2py_interface.py` bridges Python to the Fortran core
- Explored the different model flavours and their parameter structures
- Read through PRs [#68](https://github.com/wilkinsdr/reltrans/pull/68) and [#94](https://github.com/wilkinsdr/reltrans/pull/94) carefully enough to follow the architectural thinking behind the refactor — the `t_model_arguments`, `t_config`, and `t_arrays` derived types, why `do_convolutions` was separated, and where the ongoing structural work is headed.

---

## 5.0 Availability

Since this is a **350-hour project**, I plan to dedicate around **3–4 hours each weekday** and **6–8 hours on weekends and holidays**. As a student, I'll make sure to manage my time well to maintain a healthy balance between academics and GSoC. I'm genuinely excited about the project and fully committed to giving it my best.

---

## 6.0 GSoC Timeline

| Week | Phase | Tasks |
|---|---|---|
| **Bonding** | Community Bonding | Deep dive into codebase, finalise tutorial outline, agree on testing standards and conventions with mentors, set up development environment fully |
| **Week 1** | Phase 1: Familiarisation | Trace full Python-to-Fortran call flow, map subroutine structure, study all model flavours and `ReIm` flag outputs, review Issues |
| **Week 2** | Phase 1: Familiarisation | Review existing test suite gaps, draft testing framework structure, finalise notebook outline with mentor feedback |
| **Week 3** | Phase 2: Tutorials | Installation and environment setup tutorials (HEASOFT, tables, env variables, compilation) |
| **Week 4** | Phase 2: Tutorials | Basic Python interface tutorials, energy array construction, calling the model, visualising the time-averaged spectrum |
| **Week 5** | Phase 2: Tutorials | All `ReIm` outputs with plots and physical interpretation, parameter sweep examples |
| **Week 6** | Phase 2: Tutorials + Tests | Model flavour comparison tutorials. Begin output coverage tests for untested products |
| **— 1st Evaluation** | | Core tutorials delivered, initial test suite integrated into CI |
| **Week 7** | Phase 3: Testing | Output coverage — lag-energy, cross-spectrum real/imaginary, lag-frequency snapshot tests |
| **Week 8** | Phase 3: Testing | Python wrapper tests, parameter boundary checks, and environment variable validation |
| **Week 9** | Phase 3: Testing | Subroutine-level unit tests for refactored components |
| **Week 10** | Phase 3: Testing | Regression tests, CI integration, tolerance framework informed by PR [#94](https://github.com/wilkinsdr/reltrans/pull/94) discussion |
| **Week 11** | Phase 4: Polish | Advanced tutorials — PyXSPEC integration and real data fitting workflows |
| **Week 12** | Phase 4: Polish | Contributor guide, final cleanup, mentor review addressing, documentation for long-term maintenance |
| **— Final Evaluation** | | All deliverables merged, contributor guide complete, project ready for long-term use |

> **Note:** Additional work will be added and dates will remain flexible. This timeline is provided for context only.

---

## 7.0 A Project Based on Reltrans

### The RELTRANS Dashboard *(for new users!)*

I have designed and built a dashboard on top of Reltrans.

It is a high-performance research suite designed for advanced X-ray reverberation mapping analysis (Ingram et al. 2019). It provides a seamless interface for sophisticated Kerr geometry calculations, supporting **Single LP**, **Double LP**, and **Distance-based lamp-post disk illumination models**.

**Key features:**
- Backend engine accurately computes Innermost Stable Circular Orbits (ISCO), relativistic g-factor profiles (*g_sd*, *g_do*), and time-delay transfer functions *Ψ(t)* in the strong-field regime.
- Convolves rest-frame XILLVER reflection tables with gravity-warped transfer functions to produce observed blurred reflection features and energy-dependent phase lags.
- Calculates radial ionisation *ξ(r)* profiles and Fe abundance dependencies while dynamically scaling timing delays by the black hole mass.
- A persistent **Plot History Book** allows researchers to navigate through past results using Previous and Next buttons, ensuring data isn't lost during parameter exploration.
- Toggle between **9 specialised research plots**, including emissivity profiles and cumulative flux distributions, within a professional dark theme.
- Every generated figure is available for instant download as a **high-resolution PNG** for publication or as **raw CSV data** for secondary analysis.
- Automates all XSPEC backend requirements by self-managing environment variables and table paths, ensuring a zero-config, interactive experience.

<img width="1222" height="553" alt="image" src="https://github.com/user-attachments/assets/c52a8650-e855-4e7d-9a7b-fd513d657bd2" />

<img width="1222" height="553" alt="image" src="https://github.com/user-attachments/assets/eebbbadb-d4c6-42ab-8ab7-b7773bad2435" />

<img width="1221" height="551" alt="image" src="https://github.com/user-attachments/assets/623eb11b-3b6d-4f7b-ae4e-2733988d8fa0" />

<img width="1223" height="543" alt="image" src="https://github.com/user-attachments/assets/d35c7a32-54a7-4030-b17f-85a5b076e850" />

<img width="1218" height="524" alt="image" src="https://github.com/user-attachments/assets/83a78111-0cd9-4670-90f3-3bcd0e2776a0" />

---

## 7.0 What I Am Studying and For What

| Resource | Topic |
|---|---|
| [tbabs — X-Ray Absorption in the ISM](https://pulsar.sternwarte.uni-erlangen.de/wilms/research/tbabs/) | X-Ray Absorption in the Interstellar Medium |
| [XSPEC Tutorial (YouTube)](https://www.youtube.com/watch?v=k1CD8f1LLhc) | XSPEC workflow and fitting |
| [Reltrans Official Docs](https://reltransdocs.readthedocs.io/en/latest/) | Reltrans architecture and model reference |

---

## 8.0 Appreciation

First of all, I would like to express my sincere appreciation for the OpenAstronomy community. It truly feels right to be here — welcoming, insightful, and inspiring. I've already learned a great deal just by engaging with the community for a month and exploring its resources for a year, and it has increased my interest more toward black holes. :)

---

## 9.0 GSoC

**Have you participated previously in GSoC? When? With which project?**
Yes — in 2025 with **Stingray.jl** (OpenAstronomy).

**Are you also applying to other projects?**
Nope!
