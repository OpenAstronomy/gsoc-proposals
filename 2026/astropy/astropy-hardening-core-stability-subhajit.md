[astropy] Hardening astropy’s core stability - Subhajit Bhattacharjee
Contributor Information
•	Name: Subhajit Bhattacharjee
•	Time-zone: IST (UTC +5:30)
•	Matrix/slack/IRC Handle: subhajit_b (Slack) / Thenewbiecoder80221 (GitHub)
•	Github/forge username: Thenewbiecoder80221
•	Blog: [Link to your blog/portfolio if you have one, else leave blank]
•	PR link(s):
o	PR #19424: TST: Move test data for test_comp_image_quantize_level from scipy to local.
o	PR #19412: Fix AttributeError when printing deepcopy of masked Table.
o	PR #19480: Fix pandas parquet string dtype compatibility (U3 vs object).
o	PR #19454: Fix longitude latitude addition logic.
________________________________________
Background
I am an undergraduate student at NIT Agartala specializing in Electronics and Instrumentation Engineering. My technical interests lie at the intersection of high-level abstractions and low-level performance. I am comfortable working across Python and C++, with a specific focus on how data structures are managed across the C-API boundary.
Beyond coding, I have experience navigating complex CI/CD environments. My recent contributions to Astropy have focused on "plumbing" tasks—fixing regression bugs, managing internal data paths, and ensuring cross-library compatibility—which have prepared me for the architectural challenges of this project.
Interest in OpenAstronomy
I want to work with OpenAstronomy because it manages the most critical infrastructure in modern astrophysics. While many developers focus on adding new scientific features, I am drawn to the sustainability and reliability of the codebase itself. I believe that for Astropy to serve the community for another decade, its core must be decoupled and hardened against the "Monolith Tax." Working here allows me to contribute to foundational software that impacts thousands of researchers globally.
________________________________________
Project Proposal Application
Proposal Title: Hardening and Decoupling Astropy’s Core Low-Level Layer
Organisation: Astropy
Summary
Astropy is a mixed-language codebase where performance-critical "hotpaths" are written in C, C++, and Cython. Currently, these extensions are tightly coupled with the Python API, causing CI overhead (20–50% of runtime spent on recompilation) and a significant testing gap: the low-level layer is rarely tested in isolation.
This project is attractive to me because it is a "surgical" architectural task. My goal is to build an independent testing framework that targets these extensions directly. My prior work on PR #19424 (decoupling test data) and PR #19480 (handling strict memory layouts/dtypes) gives me the exact background needed to navigate Astropy's internal build logic and ensure the integrity of its compiled components.
Deliverables
1.	Isolated Test Environment: A standalone pytest configuration capable of loading and exercising Cython/C extensions without importing the entire high-level Astropy package.
2.	Hardened Extension Suites: Direct unit tests and Property-Based tests (using Hypothesis) for astropy.convolution and astropy.stats.
3.	Modernization Prototype: A feasibility report and prototype meson.build for a decoupled subpackage to benchmark build-speed improvements.
________________________________________
Description/Timeline
Period	Description
Community Bonding	Audit existing setup_package.py files across core subpackages. Finalize the "Hot-Path" module list with mentors and establish a dedicated testing directory (e.g., astropy/tests/low_level).
Week 1-2	Isolation Layer: Design a mechanism to expose internal C-functions via Cython .pxd files for direct testing. Establish a CI job that triggers only on extension changes.
Week 3-4	The Convolution Pilot: Implement direct tests for C-kernels in astropy.convolution. Focus on boundary conditions and memory alignment.
Week 5-6	Fuzzing & Invariants: Integrate Hypothesis to generate edge-case inputs (NaN, Inf) to stress-test the compiled C-logic against segmentation faults.
Week 7-9	Expansion: Port the testing strategy to astropy.stats (fast-histogram logic) and investigate the complexity of astropy.wcs (wcslib) bindings.
Week 10-11	The Meson Prototype: Implement a meson.build prototype for the decoupled convolution module. Compare build times and developer experience against the current setuptools setup.
Week 12	Documentation & APE: Draft a report on the feasibility of a full core split and provide documentation for future contributors to add low-level tests.
________________________________________
GSoC
•	Have you participated previously in GSoC? No, this is my first time applying.
•	Are you also applying to other projects? No, I am focusing my efforts entirely on this Astropy proposal as it perfectly matches my technical interests.
Schedule availability
I can commit 35 hours per week to the project. I have no major holiday plans during the program. My university semester ends in May, allowing me to focus fully on the coding phases from June onwards.
Other comments
I am already active on the Astropy Slack and GitHub. I prefer a "fail fast" approach—opening draft PRs early to get mentor feedback on architectural decisions before committing to a final implementation.


