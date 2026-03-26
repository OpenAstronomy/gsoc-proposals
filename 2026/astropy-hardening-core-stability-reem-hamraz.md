
# Hardening Astropy's Core Stability

## Contributor Information
* **Name:** Reem Hamraz
* **Time-zone:** IST (UTC +5:30)
* **Slack Handle:** `_reem_`
* **Github username:** [ReemHamraz](https://github.com/ReemHamraz) 
* **Blog:** https://dev.to/reemhamraz
* **Blog RSS Feed:** https://dev.to/feed/reemhamraz
* **Proposal Discussion Link:** https://github.com/OpenAstronomy/gsoc-proposals/pull/24 

### PR link(s): Pre-GSoC Contributions

**Open (4)-** 
    1. [#19458 — TST: Add direct tests for join_inner in table/_np_utils](https://github.com/astropy/astropy/pull/19458)
    2. [#19106 — Fix unit extraction in structured_to_unstructured for StructuredQuantity](https://github.com/astropy/astropy/pull/19106)
    3. [#19107 — Align Masked Helpers with NumPy 2.4 signatures; fix in1d namespace crash](https://github.com/astropy/astropy/pull/19107)
    4. [#19096 — Update masked function signatures for NumPy 2.4](https://github.com/astropy/astropy/pull/19096)

**Merged (3)-**
    1. [#19103 — Fix redundant as_array helpers for NumPy 2.0+ signatures](https://github.com/astropy/astropy/pull/19103)
    2. [#19104 — Update matrix_rank helper for NumPy 2.0+ tolerance parameters](https://github.com/astropy/astropy/pull/19104)
    3. [#19468 — Add docstring explaining jointype integer mapping in _np_utils.pyx](https://github.com/astropy/astropy/pull/19468)

---

### Background
I love coding. And the fact that my dad bought me my first laptop when I was 10 certainly didn't cure my obsession. 

Hi! My name is Reem Hamraz and I'm a 3rd year CS major at Integral University, specializing in Data Science and AI. My new year's resolution was to contribute to open source (while people make resolutions to be healthier, mine was to sit at my desk and code). So naturally, I started looking for organizations that used Python and C — which is squarely where my experience sits (mainly because of my recent internship and my relevant coursework) — I found Astropy. It was specifically [issue #19093](https://github.com/astropy/astropy/issues/19093) that pulled me into the codebase. What started as a quick look, turned into a proper deep-dive, and here I am, writing my GSoC proposal for this very organization. I've already contributed to Astropy and my first ever merged PR was with this organization as well. That being said, I am fully intent on keeping my streak of meaningful contributions going.

---

### Interest in OpenAstronomy
Astropy isn't just a popular library, it's genuinely well-engineered. The mixed-language architecture, the build complexity, the fact that a community of volunteers maintains compiled extensions across Python versions and platforms: that level of complexity is daunting, but it’s exactly the kind of robust, low-level engineering problem that piques my interest.

The specific problem this project is tackling, is that a significant chunk of the codebase is almost entirely untested in isolation, and this is the kind of gap that doesn't show up on a surface read but matters a lot once you understand the dependency graph.

I am motivated to work on an infrastructure that makes the rest of the project more trustworthy, specifically building a testable compiled layer that the whole ecosystem can rely on. It's not very glamorous, but, it's useful in a way that compounds.

---

## Project Proposal Application
**Proposal Title: Hardening Astropy's Core Stability**

**Organisation: Open Astronomy (Astropy)**

**Project Size: 175 hours (Medium)**

**Mentors:** [@neutrinoceros](https://github.com/neutrinoceros), [@astrofrog](https://github.com/astrofrog), [@nstarman](https://github.com/nstarman)

---

### Summary
Astropy’s performance relies on a complex, mixed-language architecture. While the Python-facing API is robust and well-tested, the underlying compiled layer (C, C++, and Cython), which handles the library's most performance-critical "hot-paths" remains a significant testing blind spot, and it accounts for a disproportionate share of build complexity and CI time. Before anyone can responsibly split the low-level layer into a separate package (as outlined in the draft APE), that layer needs its own tests and right now, it doesn't have nearly enough to stand on its own. Worse, currently, the compiled extensions that handle performance-critical operations are only validated indirectly through high-level Python calls. This creates a risky abstraction: a regression deep within the compiled code can be masked by the Python layer sitting above it, often remaining invisible; you wouldn't know something broke until it was already downstream.

Hence, this project aims to build a dedicated, de novo test suite that exercises each compiled extension module directly, bypassing the public API. While this is an essential prerequisite for the proposed APE split, it also significantly de-risks the other major Astropy milestones:

* **The APE Split:** Proving each extension is stable enough to live in a separate package ([neutrinoceros/astropy-APEs#1](https://github.com/neutrinoceros/astropy-APEs/pull/1)).
* **Meson Migration:** Providing a safety net to catch silent compilation regressions during the build system transition ([astropy#17760](https://github.com/astropy/astropy/issues/17760)).
* **Python 3.15 Limited API:** Safely testing the internal C-extension refactors required for upcoming free-threaded builds ([astropy#19249](https://github.com/astropy/astropy/issues/19249)).

Personally, I think that the hardest part of this project is not writing the tests, it's figuring out *how* to write them and *how* to probe these modules in isolation, which all the more drives me to strive harder, so that I can bridge this gap.

And, I think I can do this because I've already started. I've made some contributions, traced the extension module inventory, studied the `astropy/table` subpackage (which the mentors flagged as a good starting point), and I am currently looking into the `unit_list_proxy.c` problem in detail. 

---

### Deliverables
**1.** Audit of the testable C-API surface for compiled extension modules, cross-referencing against `MANIFEST.in` to ensure all active modules are mapped for the testing roadmap (replacing the need for a redundant inventory file).

**2.** A pytest-based test suite that directly imports and exercises compiled extension modules from `astropy/table`, `astropy/time`, `astropy/timeseries/periodograms`, and `astropy/utils/xml` — without going through the public Python API.

**3.** A resolution to the `astropy/wcs/src/unit_list_proxy.c` circular dependency problem (its runtime reliance on `astropy.units.UnitBase`). The primary approach will be exploring a structural refactor of the C code to break the dependency cycle entirely. If a refactor proves unfeasible, the fallback will be a documented Python-level minimal stub to allow isolated testing.

**4.** Direct pytest tests for astropy/wcs compiled modules, with documentation of symbols that cannot be tested in isolation due to the import cycle.

**5.** Direct pytest tests for astropy/erfa (_erfa.so), cross-referenced against the upstream ERFA C library documentation.

**6.** Direct pytest tests for at least one additional subpackage (astropy.coordinates or astropy.time).

**7.** TESTING_EXTENSIONS.rst: a documentation page added to Astropy's docs explaining the direct extension test layer, its purpose, and how to run it independently.

**8.** Final written report: total coverage added, honest inventory of what remains untested, and a step-by-step guide for future contributors to extend the suite to new subpackages.

---

### Stretch Goals: 
**9.** Exploratory PR for Meson build system migration ([astropy #17760](https://github.com/astropy/astropy/issues/17760)), following patterns from SciPy's completed migration (scipy/scipy#14480).

**10.** Exploratory PEP 803/809 / Limited API compatibility ([astropy #19249](https://github.com/astropy/astropy/issues/19249)), allowing a single compiled binary to run across multiple Python versions without recompilation.

---

### Description/timeline
I've started with `astropy/table` mostly because it's simpler in comparison to the other sub packages and will help me get into the groove of things.

The harder case is `astropy/wcs/src/unit_list_proxy.c`. This C extension uses `PyObject*` that handles to a `UnitBase`-derived class from `astropy.units`. This is a class that doesn't exist at C compile time and is only available once Python has fully initialized `astropy.units`. That's the cycle: you can't test the WCS extension in isolation without first breaking a runtime dependency the C code never had to think about at compile time. My initial instinct was to mock `astropy.units` with a minimal Python stub. After feedback from the mentors, I understand that the cleaner path is to refactor the dependency out of the C code directly. That's what I'm going to try first. The stub stays in my back pocket if the refactor proves out of scope for the summer.

---


# Project Timeline

| Period | Description |
| :--- | :--- |
| **Community Bonding** (May 1 – May 24) | Attend the monthly Astropy Dev Telecons (aiming to join the calls on April 1st, May 6th, and the June 1st call marking the end of the bonding period). Re-read the draft APE in full. Finalize the list of extension modules to cover and the order of attack. Understand the existing test infrastructure well enough to build alongside it rather than against it. Spend time on `unit_list_proxy.c` specifically — prototype the stub approach and see if it holds. |
| **Week 1** (May 25 – May 31) | Write tests for `astropy/table/_np_utils.pyx` — this exposes utility functions for NumPy array operations used by `astropy.table`. Direct import of the `.so`, basic input/output checks, edge cases. |
| **Week 2** (Jun 1 – Jun 7) | Write tests for `astropy/table/_column_mixins.pyx`. This Cython extension provides the underlying array-like behavior and internal operations for `Column` objects. Tests here will directly cover the C-level slot implementations. |
| **Week 3** (Jun 8 – Jun 14) | Begin `astropy/time/src/parse_times.c`. This parses time strings into structured data. It's the largest of the simpler modules (~500 LOC) but the mentors noted it's heavily boilerplate. Map the public interface exposed by the compiled module and draft tests. |
| **Week 4** (Jun 15 – Jun 21) | Complete `parse_times.c` test coverage. Write the mid-project report: what's working, what the testing patterns look like, any issues/technical difficulties discovered. |
| **Week 5** (Jun 22 – Jun 28) | Start on `astropy/utils/xml/src/iterparse.c` — this wraps libexpat for iterative XML parsing. |
| **Week 6** (Jun 29 – Jul 5) | Complete `iterparse.c` tests. The module has a defined C API surface; tests will cover the parser's behavior across well-formed XML, malformed input, and encoding edge cases. |
| **Week 7** (Jul 6 – Jul 12) | **Midterm Evaluation (July 6–10).** Address any feedback. `astropy/timeseries/periodograms/bls/_impl.pyx` and `bls.c` — the Box Least Squares periodogram implementation. This is a numerically intensive module; tests will use known synthetic light curves with planted transit signals and check the output against expected period/depth/duration values. |
| **Week 8** (Jul 13 – Jul 19) | `astropy/timeseries/periodograms/lombscargle/
implementations/cython_impl.pyx` — the Lomb-Scargle periodogram Cython implementation. Same approach: synthetic time series, known periods, verify the power spectrum output. |
| **Week 9** (Jul 20 – Jul 26) | Begin the `unit_list_proxy.c` work. Implement the stub approach developed during bonding (or document why it doesn't work and what the alternative is). |
| **Week 10** (Jul 27 – Aug 2) | **Buffer week.** Backfill any gaps in coverage from weeks 1–8, improve test quality, add documentation. |
| **Week 11** (Aug 3 – Aug 9) | Final evaluation prep. Review all tests, ensure they run cleanly in CI. Write the final report draft. |
| **Week 12** (Aug 10 – Aug 16) | Finish documentation and ensure all remaining code is pushed and passing checks. |
| **Final Evaluation** (Aug 17 – Aug 31) | **Submit final report.** Document what was covered, what wasn't, and what would be needed to finish the remaining modules (for future contributors). |


## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time participating in GSoC.

### Are you also applying to other projects?
Yes, I'd mailed my proposal to Data for the Common Good and a mentor advised me to submit it on the official GSoC site.

### Schedule availability
My end-semester exams run May 5th to May 26th, 2026, which overlaps with the community bonding period and the first two days of the official coding phase. During those three weeks, I can commit 8–10 hours per week rather than the full 16. I have structured the proposal to absorb this: community bonding is intentionally lighter (codebase reading, environment setup, mentor alignment rather than deliverable-heavy work); for the first two days of the coding period (May 25 and 26), I will focus on lightweight setup tasks and immediately ramp up to my full schedule on May 27th, making up the slight deficit during Weeks 3 through 5. I will tell my mentors at least one week before exams start and will share a revised schedule (if need be).

Outside the exam window, I have no other competing commitments. No internship, no part-time work. This will be my primary summer project. I am in IST (UTC+5:30) and typically have focused coding blocks from 6:00 PM to 10:00 PM IST.

### Weekly Schedule
* Monday–Friday: 2–3 focused hours per day on coding and testing.
* Saturday: documentation, PR cleanup, and written updates to the mentors.
* Sunday: buffer for anything that ran over during the week.

### Progress Reporting
* Weekly: a short written update (Slack Huddles or direct email) covering what was done, what is blocked, and what is next.
* Biweekly: a 30-minute video call with the mentors for code review and direction-setting.
* Per milestone: a short post with a demo or output example.
* Continuous: all work in a public fork with a draft PR open from Day 1.

---

## Personal Statement

### Education
B.Tech in Computer Science Engineering (Data Science & AI) — Integral University, Lucknow, India. CGPA: 8.50 / 10. Expected graduation: November 2027.

Senior Secondary: Loreto Convent Intermediate College, 80.2% (2023). Secondary: Delhi Public School Jankipuram, 96.6% (2021).

### Technical Experience
**Undergraduate Research — Virginia Commonwealth University (May 2024 – present, remote)**
Clinical data analysis on 562 liver transplant cases in R: 25+ statistical models, univariate and multivariate regression, and co-authorship on a clinical research paper studying post liver transplant weight trajectory patterns using generative AI analytics. 

**AI Internship — Signature IT Software Designers Pvt. Ltd (May – July 2025)**
Built a multi-model classification system (SVM, KNN, Decision Tree) with 92% accuracy using GridSearchCV and ROC-AUC optimization. Developed a Python data pipeline and real-time visualization dashboard using matplotlib, pandas, and seaborn. Automated an ML pipeline reducing prediction error by 18%. The main thing this work taught me: pipelines that are not reproducible and testable are not worth building. 

**ReMembr — AI-Assisted Cognitive Recognition Application (December 2025)**
Built a dementia support application using Python, OpenCV, and TensorFlow/PyTorch. The system uses CNN-based inference to identify familiar faces and objects in real time to aid memory recall.
The main engineering constraint here was false positives: in an assistive context, a wrong answer has real consequences. That reliability bar is not different from what a patient-facing health data backend needs.

**Heart Disease Prediction System (November 2025)**
ML system on a 70,000-record clinical dataset. Full preprocessing, feature engineering (BMI, MAP, Pulse Pressure), and 7 ML algorithms benchmarked. Optimized a Random Forest model to 72.5% accuracy and 0.73 precision, deployed as a serialized pipeline for real-time health-risk screening.

---

### Open Source Contributions 

| Pull Request | Organisation — Status |
| :--- | :--- |
| PR #19096 — Update masked function signatures for NumPy 2.4 | Astropy — In Review |
| PR #19103 — Fix redundant as_array helpers for NumPy 2.0+ signatures | Astropy — Merged |
| PR #19104 — Update matrix_rank helper for NumPy 2.0+ tolerance parameters | Astropy — Merged |
| PR #19106 — Fix unit extraction in structured_to_unstructured for StructuredQuantity | Astropy — In Review |
| PR #19107 — Align Masked Helpers with NumPy 2.4 signatures; fix in1d namespace crash | Astropy — In Review |
| PR #19458 — Add direct tests for join_inner in table/_np_utils Cython extension | Astropy — In Review |
| PR #19468 — Add docstring explaining jointype integer mapping in _np_utils.pyx | Astropy — Merged |
| PR #12454 — Archive migrations by year and enforce 6-month safety gap | Submitty — In Review |
| PR #12 — Build document ingestion pipeline for GA4GH policy compliance checks | GA4GH — In Review |

Getting involved with Astropy has been an incredible learning experience (I started contributing back in December 2025). So far, I've submitted nine pull requests — three are already merged, and six are currently in active review. Each of these PRs represents the countless hours spent reading the codebase to truly understand its inner workings before writing a single line of code. My goal is to always deliver thoughtful, production-ready work , and I am fully committed to channeling that same dedication into this summer's project. While my code goes through rigorous review and iterative corrections, I genuinely thrive on that feedback. It has been the absolute best way for me to deeply learn the codebase, align with the community standards, and grow as a contributor.

---

### Achievements
*  Published co-author: “Defining Weight Trajectory After Liver Transplantation Using Generative AI” (VCU, 2024–present).
*  Winner, CEPT Urban Innovation Hackathon 2025 and WebCraft 2026.
*  Multiple Best and Outstanding Delegate accolades, International Model United Nations.
*  Certifications: Python (IBM), Data Analytics in Python (IIT BHU), Software Engineering (JP Morgan), GNSS Overview (ISRO).
*  University Tech Club Chief Coordinator. TEDxIUL Lead Organiser and Licensee.

---

## Comments

### Why This Proposal Is Stronger Than "Just Adding More Tests"
While Astropy's public API is already extensively tested, its underlying compiled building blocks currently lack isolated testing. What makes this project different is that it operates one layer below the public API, at the very boundary between Python and compiled code, which is technically harder and more consequential for the project's long-term architecture. 


### Connection to the Broader Astropy Roadmap
While establishing direct testing for Cython extensions significantly improves the codebase, this project aligns more powerfully with the Astropy Project’s roadmap goals for Community Building and Sustainability.

The official roadmap explicitly prioritizes initiatives to "increase the learning and mentoring opportunities for people interested in becoming contributors and helping to develop existing contributors into maintainers," as well as to "increase inclusion, diversity, and empowerment efforts within the Astropy Project and NumFOCUS communities."    

My participation in this GSoC project is a direct realization of these core objectives. As a woman in tech from an under-represented region in the global open-source community, this internship will provide me a vital, hands-on mentoring environment. By tackling the C-extension testing framework, I am not only filling a critical technical gap, but I am also actively developing the deep architectural knowledge required to transition from a first-time contributor into a long-term maintainer. Ultimately, this opportunity empowers me to bring a diverse perspective to the Astropy ecosystem and helps pave the way for broader inclusion within the NumFOCUS community. That's the real valuable aspect for me, and I think it's worth saying plainly rather than dressing it up in roadmap language.


### Post-GSoC Plans
* **Months 1–3:** Maintain the test files as Astropy's compiled extensions change. When a new version of ERFA is vendored or a Cython extension is updated, the direct tests will need updating, this is essentially an ongoing maintenance work.
* **Months 3–6:** Explore contributing a short note or follow-up issue about the Meson migration, especially if the exploratory PR receives positive mentor feedback
