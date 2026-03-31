Google Summer of Code 2026
Application Proposal
Interactive Tutorials & Automated Testing for the Reltrans Codebase
OpenAstronomy  —  Sub-org: Reltrans (X-ray Reverberation Mapping)

1. CONTRIBUTOR INFORMATION
Name	Naaysha Jain
Email	naayshajain0112@gmail.com
Time-Zone	IST (UTC+5:30)
Matrix / IRC	naayshajain  (OpenAstronomy Zulip & GitHub Discussions)
GitHub	https://github.com/naayshajain0112
PR Link(s)	reltrans/reltrans #120 — Docstrings & readability in f2py_interface.py (open, in final review) (https://github.com/reltrans/reltrans/pull/120)
University	Vellore Institute Of Technology (Vellore) 
Availability	~40 hrs/week | No internships, exams, or travel during the coding period

2. BACKGROUND
About Me
I am a second-year Computer Science undergraduate whose work sits at the intersection of scientific computing and software reliability. My daily tools are Python, NumPy, SciPy, Astropy, Jupyter, and pytest — the exact stack that powers the Reltrans ecosystem. Beyond coursework, I spend significant time reading and contributing to open-source scientific codebases, because I believe the quality of research software directly determines the quality of research.
Interest in OpenAstronomy
OpenAstronomy stands apart from other umbrella organisations because it treats scientific software as a first-class engineering artefact. That means not just writing code that produces numbers, but writing code that is correct, tested, documented, and maintainable by distributed teams of researchers who are primarily domain scientists, not software engineers. The GSoC projects that have had lasting impact in this umbrella — the SunPy download pipeline, the Astropy testing infrastructure, the sunkit-image tutorial series — share a common thread: they invested in foundations rather than features. That is what this proposal does for Reltrans.
Why This Project, and Why Me
Reltrans produces scientifically critical outputs. Researchers use it to fit real X-ray data from NICER, XMM-Newton, and NuSTAR. A silent regression in the Python wrappers, or a new graduate student who cannot figure out how to run the model, translates directly into wasted observing time and incorrect published results. Those stakes are what make this project genuinely important — not just useful.
I am not approaching Reltrans from the outside. Through PR #120 I have already gone deep into f2py_interface.py, written Google-style docstrings for all four core wrapper functions, and iterated through two rounds of review with maintainers @mgullik and @bjricketts. That work gave me something no other applicant can claim on day one: a working build environment, a verified understanding of the Python–Fortran interface, and an established relationship with the people who will mentor this project. The gap between where PR #120 ends and where this proposal begins is large, but the prerequisite step — understanding the codebase well enough to contribute meaningfully — is already done.

3. PROJECT PROPOSAL
Title: Interactive Tutorials & Automated Testing for the Reltrans Code
Organisation: OpenAstronomy — Sub-org: Reltrans

Summary
Reltrans is a semi-analytical model for X-ray reverberation mapping of accreting black holes, with a Fortran computational core wrapped in Python via f2py. Despite its scientific value, the codebase has two compounding problems: no interactive tutorials to onboard new users, and no automated test suite to catch regressions before they reach researchers. This 12-week project addresses both with equal weight. The first deliverable is six Jupyter notebook tutorials covering the full user journey from environment setup to advanced PyXspec integration. The second is a layered pytest framework — unit tests, regression golden-output comparisons, and edge-case tests — achieving more than 80% branch coverage of the Python wrappers and integrated into GitHub Actions CI on every pull request.
Why This Project is Attractive — and Why I Can Execute It
The appeal of this project is not that it adds a visible new capability. It is that it makes the existing capability trustworthy. Every tutorial I write is a researcher I onboard who would otherwise have abandoned the codebase after an afternoon. Every test I add is a regression I prevent from reaching a published analysis. That is the kind of work that experienced open-source contributors value most — and the kind that generic applicants typically avoid because it is harder to make sound impressive.
I can execute it because I have already demonstrated the prerequisite skills in the Reltrans codebase itself. PR #120 is not a token contribution made to qualify for GSoC. It required reading the Fortran source files, understanding how f2py generates Python bindings, and writing documentation accurate enough to survive two rounds of expert review. That is exactly the level of understanding needed to design meaningful tutorials and tests — not the surface-level familiarity of someone who cloned the repository yesterday.

4. DELIVERABLES
1.	Six Jupyter Notebook Tutorials. Tutorial 01: Installation & environment verification. Tutorial 02: Python interface basics and f2py loading. Tutorial 03: time-averaged spectra with parameter sensitivity analysis. Tutorial 04: Fourier-domain cross spectra and lag-frequency plots. Tutorial 05: advanced workflows — parameter grid search, output caching, PyXspec integration. Tutorial 06: contributor guide — how to extend tutorials and tests. Each notebook is independently executable, narrative-driven, and seeds all parameters explicitly for reproducibility.
2.	pytest Unit & Regression Test Suite. Unit tests for all four public wrapper functions (reltransPL, reltransDCp, reltransDbl, reltransx) covering output shape, dtype, and reference numerical values for known inputs. Regression golden-output files committed to the repository with configurable numerical tolerance. Edge-case tests for boundary inputs (zero flux, maximum spin, extreme energy ranges). One end-to-end integration smoke test covering the full compile-to-output pipeline. Target: more than 80% branch coverage of the Python wrapper code.
3.	GitHub Actions CI Workflow. A .github/workflows/tests.yml file running unit and regression tests on every pull request, caching the Fortran build artefact, and reporting coverage. Tests tagged by pytest marker (unit, regression, slow) so fast checks run on every PR and heavier regression checks run on a nightly schedule. Pre-commit hooks enforcing notebook output clearing, Black, and flake8.
4.	Contributor Documentation & Final Report. Docstrings for all test fixtures and conftest.py helpers. A CONTRIBUTING.md section explaining how to add a tutorial notebook and how to add a test, with worked examples. A written GSoC final report summarising design decisions, coverage metrics, and recommendations for future contributors.

5. DETAILED WEEKLY TIMELINE
The timeline below is written at weekly granularity because a vague phase-level plan is not a plan — it is a wish list. Each week has a specific set of outputs, a success criterion, and an explicit contingency for what happens if the week runs long. Evaluation checkpoints are marked.
A note on how this was constructed: I read through the accepted proposals from past OpenAstronomy GSoC cycles, including those for SunPy, Astropy, and sunkit-image, and noted that the proposals which survived rejection shared one feature — the timeline was specific enough that a mentor could hold the contributor accountable to it. That is the standard I am applying here.

Period	Specific Outputs, Success Criteria & Contingencies
Community Bonding May 8 – 26 (Pre-coding)	Goal: enter Day 1 of coding with zero unknowns.
–	Read the full Fortran source — reltrans.f90 and companion files — not just the Python wrappers. Map every function that feeds into f2py_interface.py.
–	Finalise PR #120. Resolve any remaining reviewer comments and get the docstring PR merged before Week 1 starts, or formally close it with a clean summary.
–	Agree with mentor(s) on: notebook naming convention, parameter choices for reference numerical tests, golden-output tolerance thresholds, and CI caching strategy.
–	Set up the development environment on the machine I will use for the full summer. Confirm that the Fortran compiler, f2py, and all Python dependencies install cleanly.
–	Draft outlines for all six tutorials — one paragraph per notebook describing scope, inputs, and expected outputs — and share with mentor for early feedback before any code is written.
–	Write the GitHub Actions YAML skeleton: an empty workflow file that triggers on pull requests and runs a placeholder test. Getting CI green with a placeholder is faster than building it mid-project.
✓ Milestone: PR #120 resolved. CI skeleton live. All six notebook outlines approved by mentor. Zero surprises on Day 1.
Week 1 May 27 – Jun 2	Tutorial 01 — Installation & Environment Verification
–	Write the notebook section by section: Fortran compiler installation (gfortran), f2py compilation steps, Python package installation via pip/conda, and a smoke-test cell that imports the compiled extension and prints a version string.
–	Test the notebook on a clean virtual environment to confirm it runs end-to-end without pre-existing setup.
–	Write the first two unit tests: one that imports reltransPL and checks its callable signature, one that calls it with a minimal valid input and asserts the output is a numpy array of the correct shape and dtype.
–	Open a draft PR for Tutorial 01 and the two tests so the mentor can see direction before the week ends.
✓ Milestone: Tutorial 01 notebook passes nbmake in CI. Two unit tests pass. Draft PR open.
▶ If delayed: If environment setup is more complex than expected, Tutorial 01 carries into Week 2 and Tutorial 02 shifts by one week. The total project scope does not change — only the week 2 scope narrows.
Week 2 Jun 3 – 9	Tutorial 02 — Python Interface Basics & reltransPL Tests
–	Write Tutorial 02: loading the f2py extension, understanding the call signature of reltransPL, running a power-law continuum spectrum, visualising and interpreting the output array.
–	Expand unit tests for reltransPL: (a) output shape check, (b) output dtype check, (c) reference numerical value for a specific input — assert_allclose against a value computed from the known-good build during community bonding.
–	Add the reltransPL tests to the GitHub Actions workflow so they run on every PR push.
–	Write conftest.py with a shared fixture that compiles and loads the Fortran extension once per test session, avoiding repeated compilation overhead in CI.
✓ Milestone: Tutorial 02 complete. reltransPL has 3 passing tests covering shape, dtype, and numerical reference. CI runs tests on every push.
▶ If delayed: If the reference value comparison fails due to platform-dependent Fortran behaviour, consult with mentor and set a looser tolerance or skip the numerical test on platforms where it is not reproducible.
Week 3 Jun 10 – 14	reltransDCp Tests & CI Coverage Reporting
–	Write unit tests for reltransDCp: output shape, dtype, and one reference value check for a known Fourier-domain cross spectrum input.
–	Add pytest-cov to the CI workflow. Generate the first coverage report and commit it as a CI artefact. Record the baseline coverage percentage.
–	Review and polish Tutorials 01 and 02 based on any mentor feedback received. Clear all notebook outputs before final commit.
✓ Milestone: reltransDCp has 3 passing tests. Coverage report live in CI. pytest markers configured. Tutorials 01 and 02 polished and merged.
Weeks 4 – 5 Jun 15 – 28	Tutorials 03 & 04 — Spectra and Fourier Analysis
–	Week 4: Tutorial 03 — Time-averaged spectra: setting up a full spectral model call, exploring iron line energy and disk ionisation sensitivity, comparing output against a reference spectrum plot. This notebook explicitly shows what happens when parameters are at their physical limits — a common source of confusion for new users.
–	Week 4: Write the regression golden-output file for reltransPL and reltransDCp. Run both functions with a fixed seed input on the known-good build and commit the output arrays as .npy files. Write the regression tests that load these files and use assert_allclose with the agreed tolerance.
–	Week 5: Tutorial 04 — Fourier-domain cross spectra: introducing reverberation lags, running reltransDCp and reltransDbl, producing energy-dependent lag-frequency plots with matplotlib. This notebook builds directly on Tutorial 03 without requiring Tutorial 03 to have been run first.
–	Week 5: Write unit tests for reltransDbl: shape, dtype, and reference value. Add the corresponding regression golden-output file.
✓ Milestone: Tutorials 03 and 04 complete. Regression golden-output files committed for reltransPL, reltransDCp, and reltransDbl. Coverage at or above 60%.
▶ If delayed: If Tutorial 04 runs long, the reltransDbl regression golden-output file moves to Week 6. The Week 6 scope is already the lightest in the project, so it absorbs this without affecting the 1st Evaluation deliverable.
Week 6 Jun 29 – Jul 5 ★ 1st Evaluation	First Evaluation Submission — All Phase 1 Work Reviewed and Merged
–	Consolidate all open PRs from Weeks 1–5 into a clean, reviewed state. Every PR must have passed CI and received at least one mentor comment before being merged.
–	Write the mid-project blog post: what was built, what was harder than expected, what was easier, what the coverage number is, and what comes next.
–	Write unit tests for reltransx: shape, dtype, and one reference value. Add the reltransx golden-output regression file. This completes test coverage of all four public wrapper functions.
–	Run a full coverage report across all tests. Document the number in the PR description. Target: more than 60%.
–	Prepare the 1st Evaluation submission: a written summary of completed work with links to all merged PRs, coverage report screenshot, and a one-paragraph reflection on the project.
✓ Milestone: All four wrapper functions have unit tests and regression golden-output files. Coverage above 60%. Two tutorials merged. 1st Evaluation submitted.
Weeks 7 – 8 Jul 6 – 19	Tutorial 05 — Advanced Workflows & Edge-Case Tests
–	Week 7: Tutorial 05 — Advanced workflows: parameter grid search over spin and inclination using numpy meshgrid, output caching strategy using joblib.Memory or a simple .npy cache, and a worked example showing how to load Reltrans model outputs into PyXspec/Xspec for spectral fitting. This is the notebook most requested by graduate student users and the one most likely to be referenced in papers.
–	Week 7: Write edge-case tests for reltransPL: zero photon flux input, maximum spin parameter (a* = 0.998), minimum and maximum energy array bounds. Document the expected behaviour (error, warning, or graceful fallback) for each case.
–	Week 8: Write edge-case tests for reltransDCp, reltransDbl, and reltransx following the same pattern. For any case where the Fortran core behaves unexpectedly, open an issue flagging it for the maintainers rather than silently suppressing it in the test.
–	Week 8: Tag all edge-case tests with @pytest.mark.regression so they can be run separately from the fast unit suite.
✓ Milestone: Tutorial 05 complete. Edge-case tests written for all four wrapper functions. Any unexpected Fortran behaviour logged as open issues with maintainer notification.
Week 9 Jul 20 – 26	Integration Smoke Test & Internal Documentation
–	Write the end-to-end integration smoke test: starting from source, compile the Fortran extension with f2py, import the result in a fresh Python process, call all four wrapper functions with minimal inputs, and assert that each returns a result of the correct type. Tag with @pytest.mark.slow and configure CI to run this test nightly rather than on every PR.
–	Write docstrings for every test fixture in conftest.py and every helper function in the test directory. A future contributor should be able to understand the test design by reading the fixture documentation alone.
–	Review all open issues flagged during edge-case testing. For each: either fix it (if within project scope), link it to the relevant golden-output test, or write a comment explaining why the behaviour is expected.
–	Run the full test suite locally — unit, regression, edge-case, smoke — and record the wall-clock time. This is the number that will go into the nightly CI timeout setting.
✓ Milestone: Integration smoke test live and running in nightly CI. All fixtures documented. Open issues triaged.
▶ If delayed: If the smoke test reveals a build reproducibility issue on the CI runner, work with mentor to add the Fortran build artefact to the CI cache and rerun. This is the most likely Week 9 blocker and has a known solution.
Week 10 Jul 27 – Aug 2 (4-day break Aug 1–4)	Tutorial 06 — Contributor Guide
This is the most important tutorial for the long-term health of the project. It does not teach users how to use Reltrans; it teaches contributors how to extend the work done in this project.
–	Section 1 — How to add a tutorial notebook: naming convention, parameter standards, how to run nbmake locally, how to clear outputs before committing.
–	Section 2 — How to write a unit test: fixture usage, naming conventions, how to generate a new golden-output file, how to set tolerance thresholds.
–	Section 3 — How to read the CI report: what each badge means, how to interpret coverage gaps, what to do if a regression test fails after a Fortran core change.
–	Section 4 — Project architecture overview: a written map of how f2py_interface.py connects to the Fortran core, which functions are tested, and which tutorials cover each function.
✓ Milestone: Tutorial 06 complete and merged. The 4-day break (Aug 1–4) is absorbed by the reduced scope of this week — Tutorial 06 is writing-heavy and can be drafted in 3 focused days.
Week 11 Aug 5 – 11	Full Test Suite Audit — Target 80% Coverage
–	Run coverage analysis on the complete test suite. Identify every uncovered branch in the Python wrapper code. For each gap: write a test if the gap represents real user behaviour, or add a comment explaining why the branch is intentionally untested.
–	Fix any flaky tests identified during Weeks 7–10. A flaky test is worse than no test — it trains contributors to ignore CI failures.
–	Enforce notebook output clearing via pre-commit hook. Test that the hook fires correctly on a deliberate violation before committing it.
–	Run the full pre-commit hook suite (Black, flake8, notebook output clearing) across all files touched during the project and fix any remaining violations.
–	Update CONTRIBUTING.md with the final coverage number and a note on how to run the hooks locally.
✓ Milestone: Coverage above 80% confirmed by CI report. No flaky tests. Pre-commit hooks passing on all project files.
▶ If delayed: If coverage is stuck below 80% because a Fortran-level code path is not reachable through the Python wrappers, document the gap explicitly and explain why it is a Fortran-layer responsibility rather than a wrapper-layer responsibility.
Week 12 Aug 12 – 16 ★ Final Evaluation	Final Polish, Report & Evaluation Submission
–	Final pass through all six tutorial notebooks: fix any broken links, ensure all output cells are cleared, verify that each notebook runs to completion in a clean virtual environment.
–	Write the changelog entry for the repository covering all work done during the project.
–	Write PR descriptions for any PRs still in review, providing enough context that a maintainer can merge them after the GSoC period ends if needed.
–	Write the GSoC final report: a summary of completed work, coverage metrics, design decisions, lessons learned, and a specific list of follow-on work for future contributors.
–	Post the final blog entry summarising the project for the broader OpenAstronomy community.
–	Submit the final evaluation.
✓ Milestone: All six tutorials merged. Test suite at 80%+ coverage. GitHub Actions CI running on every PR and nightly. Final report and blog post published. Final evaluation submitted on time.

Timeline Risk Summary
The three most plausible project risks and their mitigations are listed below. Identifying risks in the proposal is not a sign of weakness — it is evidence that the timeline was designed by someone who has thought past the optimistic case.

Risk	Likelihood	Mitigation
Platform-dependent Fortran compilation breaks reference numerical tests in CI.	Medium — already observed in PR #120 review discussions.	Cache the Fortran build artefact. Set platform-specific tolerances agreed with mentor during community bonding.
Tutorial 05 (PyXspec integration) is more complex than estimated.	Low-Medium — PyXspec is well-documented but environment setup can be opaque.	Set up the PyXspec environment during community bonding. If integration proves intractable, replace with a standalone fitting example using scipy.optimize, which is equally instructive.
Mentor is unavailable for a review period, blocking a PR merge.	Low — standard in open-source projects.	Continue working on the next week's deliverable while the PR waits. No week depends on the previous week's PR being merged — only on it being open and in review.

6. GSOC HISTORY & OTHER APPLICATIONS
Have you participated previously in GSoC? No. This is my first application.
Are you applying to other projects? No. I am applying exclusively to OpenAstronomy — Reltrans. This is a deliberate choice. Distributing preparation across multiple organisations would have produced a shallower understanding of every project and a weaker proposal for each. Instead, I spent the pre-application period going deep into one codebase — deep enough to open a PR that survived two rounds of expert review. That depth is not replicable across five simultaneous applications.

7. SCHEDULE & AVAILABILITY
I am available for the full coding period without interruption from employment, coursework, or exams. My university semester concludes before May 20.
–	Commitment: approximately 40 hours per week across five to six days.
–	Response time: available on GitHub, OpenAstronomy Zulip, and email within one business day.
–	Sync calls: willing to join a 30-minute check-in with mentor(s) at the start of each phase — four calls across the summer.
–	Public updates: weekly progress post on the OpenAstronomy community forum every Friday, covering what was done, any blockers, and the plan for the following week.
–	Holiday: a 4-day personal break is planned August 1–4, during Week 10. It is disclosed here, not mid-project. It is absorbed by reducing Week 10 to Tutorial 06, which is writing-heavy and completable in three focused days.

8. PRE-GSOC CONTRIBUTIONS & TECHNICAL BACKGROUND
Pull Request #120 — Docstrings and Readability in f2py_interface.py
Repository: reltrans/reltrans  |  Status: Open, in final review with @mgullik and @bjricketts.
–	What it does: Google-style docstrings for all four core wrapper functions — reltransPL, reltransDCp, reltransDbl, and reltransx — covering Args, Returns, and Notes with accurate parameter descriptions drawn from the Fortran source.
–	Review evidence: Two complete rounds of maintainer feedback incorporated, including formatting requirements, parameter ordering conventions, and the correct reflionx model version for reltransx documentation.
–	Why it matters here: The docstrings I wrote had to be accurate at the Fortran-interface level. That is the same level of understanding required to write a tutorial that does not mislead users, and a test that does not assert the wrong thing.

Scientific Python Background
–	NumPy — array operations, broadcasting, and np.testing.assert_allclose for numerical regression comparisons.
–	SciPy — numerical integration and signal processing, directly relevant to Fourier-domain cross spectra workflows in Tutorial 04.
–	Astropy — FITS-format spectral data, used where real observational data appears in the tutorial notebooks.
–	Jupyter / nbformat — built and documented Jupyter notebooks for university coursework; familiar with nbmake for automated notebook testing and nbformat for output clearing.
–	pytest — written unit and integration tests in personal projects; familiar with fixtures, parametrisation, markers, and pytest-cov.
–	GitHub Actions — set up CI/CD pipelines for personal repositories; understand workflow YAML, dependency caching, and artefact retention.

9. LONG-TERM COMMITMENT
Software infrastructure depreciates without maintenance. Tests go stale when model defaults change. Tutorials break when APIs are updated. I intend to prevent that by continuing to contribute after the programme ends.
–	Maintain the test suite as the codebase evolves — updating golden-output files when model defaults change and adding tests for new wrapper functions.
–	Keep tutorials current with API changes, and contribute additional notebooks as the community develops new use cases.
–	Review pull requests that touch the tested or documented code, providing continuity of knowledge so that future contributors start from a stronger foundation than I did.

Every deliverable in this project is designed to be extended by others, not to require my continued involvement. That is the correct design philosophy for open-source scientific software, and I will hold myself to it.

Thank you for considering this proposal.
Naaysha Jain  —  naayshajain0112@gmail.com  —  https://github.com/naayshajain0112
