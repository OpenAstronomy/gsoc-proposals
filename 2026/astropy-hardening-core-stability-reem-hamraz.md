
# Hardening Astropy's Core Stability

## Contributor Information
* **Name:** Reem Hamraz
* **Time-zone:** IST (UTC +5:30)
* **Slack Handle:** _reem_
* **Github username:** [ReemHamraz](https://github.com/ReemHamraz) 
* **Blog:** https://dev.to/reemhamraz
* **PR link(s):**
  * Open (4)- 
    * 1. [#19458 — TST: Add direct tests for join_inner in table/_np_utils](https://github.com/astropy/astropy/pull/19458)
    * 2. [#19106 — Fix unit extraction in structured_to_unstructured for StructuredQuantity](https://github.com/astropy/astropy/pull/19106)
    * 3. [#19107 — Align Masked Helpers with NumPy 2.4 signatures; fix in1d namespace crash](https://github.com/astropy/astropy/pull/19107)
    * 4. [#19096 — Update masked function signatures for NumPy 2.4](https://github.com/astropy/astropy/pull/19096)
  * Merged (3)-
    * 1. [#19103 — Fix redundant as_array helpers for NumPy 2.0+ signatures](https://github.com/astropy/astropy/pull/19103)
    * 2. [#19104 — Update matrix_rank helper for NumPy 2.0+ tolerance parameters](https://github.com/astropy/astropy/pull/19104)
    * 3. [#19468 — Add docstring explaining jointype integer mapping in _np_utils.pyx](https://github.com/astropy/astropy/pull/19468)

---

### Background
I love coding. And the fact that my dad bought me my first laptop when I was 10 didn't quite help with this either. 
Hi! My name is Reem Hamraz and I'm a 3rd year CS major at Integral University, specializing in Data Science and AI. My new year's resolution was to contribute to open source. While looking for organizations that used Python and C — which is squarely where my experience sits (mainly because of experience with an internship and my relevant coursework) — I found Astropy. It was specifically [issue #19093](https://github.com/astropy/astropy/issues/19093) that pulled me into the codebase. What started as a quick look turned into a proper deep-dive, and here I am, writing my GSoC proposal for this very organization. I've already contributed to Astropy and my first ever merged PR was with this organization, as well. I am fully intent on keeping my streak of meaningful contributions going.

---

### Interest in OpenAstronomy
Astropy isn't just a popular library, it's genuinely well-engineered. The mixed-language architecture, the build complexity, the fact that a community of volunteers maintains compiled extensions across Python versions and platforms: that stuff is pretty tough but at the same time it's also interesting.
The specific problem this project is tackling, that a significant chunk of the codebase is almost entirely untested in isolation, is the kind of gap that doesn't show up on a surface read but matters a lot once you understand the dependency graph.
I am motivated to work on a infrastructure that makes the rest of the project more trustworthy, more specifically building a testable compiled layer that the whole ecosystem can rely on. It's not very glamorous, but it's useful in a way that compounds.

---

## Project Proposal Application
**Proposal Title: Hardening Astropy's Core Stability**

**Organisation: Open Astronomy (Astropy)**

**Project Size:** 175 hours (Medium)

**Mentors:** [@neutrinoceros](https://github.com/neutrinoceros), [@astrofrog](https://github.com/astrofrog), [@nstarman](https://github.com/nstarman)

---

### **Summary:**
Astropy’s performance relies on a complex, mixed-language architecture. While the Python-facing API is robust and well-tested, the underlying compiled layer (C, C++, and Cython), which handles the library's most performance-critical "hot-paths" remains a significant testing blind spot, and it accounts for a disproportionate share of build complexity and CI time. Before anyone can responsibly split the low-level layer into a separate package (as outlined in the draft APE), that layer needs its own tests and right now, it doesn't have any. Worse, currently, the compiled extensions that handle performance-critical operations are only validated indirectly through high-level Python calls. This creates a risky abstraction: a regression deep within the compiled code can be masked by the Python layer sitting above it, often remaining invisible, you wouldn't know something broke until it was already downstream.
Hence, this project aims to build a dedicated, de novo test suite that exercises each compiled extension module directly, bypassing the public API entirely, that too from scratch. This work is the essential prerequisite for three major Astropy milestones:

* **The APE Split:** Ensuring each extension is stable enough to live in a separate package ([neutrinoceros/astropy-APEs#1](https://github.com/neutrinoceros/astropy-APEs/pull/1)).

* **Meson Migration:** Streamlining our build process and reducing CI friction ([astropy#17760](https://github.com/astropy/astropy/issues/17760)).

* **Limited API Compatibility:** Future-proofing the codebase across Python versions compatibility ([astropy#19249](https://github.com/astropy/astropy/issues/19249)).

Personally, I think that the hardest part of this project is not writing the tests, it's figuring out *how* to write them and *how* to probe these modules in isolation, which all the more drives me to strive harder, so that I can bridge this gap.
And, I think I can do this because I've already started. I've made some contributions, traced the extension module inventory, studied the `astropy/table` subpackage (which the mentor flagged as a good starting point), and looked at the `unit_list_proxy.c` problem in detail. 

---

### Deliverables
**1.** Full inventory of all compiled extension modules (.so files) across all subpackages, with each module name, subpackage, and Python-callable symbols listed. Committed as EXTENSION_INVENTORY.md.

**2.** A pytest-based test suite that directly imports and exercises compiled extension modules from astropy/table, astropy/time, astropy/timeseries/periodograms, and astropy/utils/xml — without going through the public Python API.

**3.** A documented approach to the astropy/wcs/src/unit_list_proxy.c circular dependency problem: the module depends at runtime on astropy.units.UnitBase, a Python class that doesn't exist at compile time. The test suite either resolves this with a minimal stub, or documents why it can't be resolved independently and proposes a path forward.

**4.** Direct pytest tests for astropy/wcs compiled modules, with documentation of symbols that cannot be tested in isolation due to the import cycle.

**5.** Direct pytest tests for astropy/erfa (_erfa.so), cross-referenced against the upstream ERFA C library documentation.

**6.** Direct pytest tests for at least one additional subpackage (astropy.coordinates or astropy.time).

**7.** TESTING_EXTENSIONS.rst: a documentation page added to Astropy's docs explaining the direct extension test layer, its purpose, and how to run it independently.

**8.** Final written report: total coverage added, honest inventory of what remains untested, and a step-by-step guide for future contributors to extend the suite to new subpackages.

---

### Stretch Goals: 
**9.** Exploratory PR for Meson build system migration (astropy #17760), following patterns from SciPy's completed migration (scipy/scipy#14480).

**10.** Exploratory PEP 809 / Limited API compatibility (astropy #19249),  allowing a single compiled binary to run across multiple Python versions without recompilation.

---

### Description/timeline
I've started with `astropy/table`, mostly because it's simpler in comparison to the other sub packages and will help me get into the groove of things.
The harder case is `astropy/wcs/src/unit_list_proxy.c`. This C extension uses `PyObject*` that handles to a `UnitBase`-derived class from `astropy.units`. This is a class that doesn't exist at C compile time and is only available once Python has fully initialized `astropy.units`. Now, here is what creates a cycle in the dependency graph: testing WCS extensions in isolation would require either mocking `astropy.units` at the Python level with a minimal stub that satisfies the C module's runtime lookups, or restructuring the C code to defer those lookups more cleanly. My current research leads to the solution that a Python-level stub is the most tractable approach without requiring a refactor of the C code itself, but this is up for suggestions.

---

# Astropy Project Timeline

---

| Period | Description |
| :--- | :--- |
| **Community Bonding** (May 8 – Jun 1) | Set up the dev environment with the Meson build system. Re-read the draft APE in full. Finalize the list of extension modules to cover and the order of attack. Understand the existing test infrastructure well enough to build alongside it rather than against it. Spend time on `unit_list_proxy.c` specifically — prototype the stub approach and see if it holds. |
| **Week 1** (Jun 2–8) | Write tests for `astropy/table/_np_utils.pyx` — this exposes utility functions for NumPy array operations used by `astropy.table`. Direct import of the `.so`, basic input/output checks, edge cases. |
| **Week 2** (Jun 9–15) | Write tests for `astropy/table/_column_mixins.pyx`. This is the mixin that provides the array-like interface on Column objects. Tests here will cover the C-level slot implementations. |
| **Week 3** (Jun 16–22) | Begin `astropy/time/src/parse_times.c`. This parses time strings into structured data. It's the largest of the simpler modules (~500 LOC) but the mentor noted it's heavily boilerplate. Map the public interface exposed by the compiled module and draft tests. |
| **Week 4** (Jun 23–29) | Complete `parse_times.c` test coverage. Write the mid-project report: what's working, what the testing patterns look like, any issues discovered. |
| **Week 5** (Jun 30–Jul 6) | **First evaluation.** Address any feedback. Start on `astropy/utils/xml/src/iterparse.c` — this wraps libexpat for iterative XML parsing. |
| **Week 6** (Jul 7–13) | Complete `iterparse.c` tests. The module has a defined C API surface; tests will cover the parser's behavior across well-formed XML, malformed input, and encoding edge cases. |
| **Week 7** (Jul 14–20) | `astropy/timeseries/periodograms/bls/_impl.pyx` and `bls.c` — the Box Least Squares periodogram implementation. This is a numerically intensive module; tests will use known synthetic light curves with planted transit signals and check the output against expected period/depth/duration values. |
| **Week 8** (Jul 21–27) | `astropy/timeseries/periodograms/lombscargle/implementations/cython_impl.pyx` — the Lomb-Scargle periodogram Cython implementation. Same approach: synthetic time series, known periods, verify the power spectrum output. |
| **Week 9** (Jul 28–Aug 3) | Begin the `unit_list_proxy.c` work. Implement the stub approach developed during bonding (or document why it doesn't work and what the alternative is). |
| **Week 10** (Aug 4–10) | **Buffer week.** Backfill any gaps in coverage from weeks 1–8, improve test quality, add documentation. |
| **Week 11** (Aug 11–17) | Final evaluation prep. Review all tests, ensure they run cleanly in CI with the Meson build. Write the final report. |
| **Week 12** (Aug 18–25) | **Submit final report.** Document what was covered, what wasn't, and what would be needed to finish the remaining modules. |

---

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time participating in GSoC.

### Are you also applying to other projects?
Yes, I'd mailed my proposal to Data for the Common Good and a mentor advised me to submit it on the official GSoC site.

### Schedule availability
My end-semester exams run May 5th to May 26th, 2026, which overlaps with the community bonding period. During those three weeks, I can commit 8–10 hours per week rather than the full 16. I have structured the proposal to absorb this: community bonding is intentionally lighter (codebase reading, environment setup, mentor alignment rather than deliverable-heavy work), and I will make up the deficit in Weeks 3 through 5 of the coding period. I will tell my mentor at least one week before exams start and share a revised week-by-week schedule.
Outside the exam window, I have no other competing commitments. No internship, no part-time work. This is my primary summer project. I am in IST (UTC+5:30) and typically have focused coding blocks from 6:00 PM to 10:00 PM IST.

### Weekly Schedule
* Monday–Friday: 2–3 focused hours per day on coding and testing.
* Saturday: documentation, PR cleanup, and written mentor update.
* Sunday: buffer for anything that ran over during the week.

### Progress Reporting
* Weekly: a short written update (Slack Huddles or direct email) covering what was done, what is blocked, and what is next.
* Biweekly: a 30-minute video call with the mentor for code review and direction-setting.
* Per milestone: a short post with a demo or output example.
* Continuous: all work in a public fork with a draft PR open from Day 1.

---

## Personal Statement

### Biographical Information
#### Education
B.Tech in Computer Science Engineering (Data Science & AI) — Integral University, Lucknow, India. CGPA: 8.50 / 10. Expected graduation: November 2027.

Senior Secondary: Loreto Convent Intermediate College, 80.2% (2023). Secondary: Delhi Public School Jankipuram, 96.6% (2021).

#### Technical Experience
**Undergraduate Research — Virginia Commonwealth University (May 2024 – present, remote)**
Clinical data analysis on 562 liver transplant cases in R: 25+ statistical models, univariate and multivariate regression, and co-authorship on a clinical research paper studying post-transplant weight trajectory patterns using generative AI analytics. 

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
| PR #12454 — Archive migrations by year and enforce 6-month safety gap | Submitty — In Review |
| PR #12 — Build document ingestion pipeline for GA4GH policy compliance checks | GA4GH — In Review |

Eight PRs, six in active review, two merged. The pattern across them: I read the codebase carefully enough to find something real to fix, and I submit work that meets the project's standards. That is what I plan to do here.

### Achievements
*  Published co-author: “Defining Weight Trajectory After Liver Transplantation Using Generative AI” (VCU, 2024–present).
*  Winner, CEPT Urban Innovation Hackathon 2025.
*  Multiple Best and Outstanding Delegate accolades, International Model United Nations.
*  Certifications: Python (IBM), Data Analytics in Python (IIT BHU), Software Engineering (JP Morgan), GNSS Overview (ISRO).
*  University Tech Club Chief Coordinator. TEDxIUL Lead Organiser and Licensee.

---

## Comments

### Why This Proposal Is Stronger Than "Just Adding More Tests"
It would be super easy to just propose adding more Python-level tests to Astropy. What makes this project different is that it operates one layer below the public API, at the very boundary between Python and compiled code, which is technically harder and more consequential for the project's long-term architecture. 

### Connection to the Broader Astropy Roadmap
This proposal is a direct prerequisite for the draft APE (astropy-APEs#1) that proposes splitting the compiled layer into a separate package. You cannot safely split a layer that has no standalone tests, because you would have no way to verify it still works independently. The test coverage built during GSoC is not incidental to that APE; it is the evidence base that makes the split feasible.

### Stretch Goals: Meson and Limited API
If time permits, two stretch goals are available: 
* 1. an exploratory PR for the Meson build system migration (issue #17760), following patterns from SciPy's completed migration (scipy/scipy#14480); and 
* 2. exploring PEP 809 / Limited API compatibility (issue #19249), which would allow a single compiled binary to run across multiple Python versions without recompilation. 
Both are downstream opportunities that the test-suite work directly enables.

### Post-GSoC Plans
* Months 1–3: Maintain the test files as Astropy's compiled extensions change. When a new version of ERFA is vendored or a Cython extension is updated, the direct tests will need updating, this is essentially an ongoing maintenance work.
* Months 3–6: Explore contributing a short note or follow-up issue about the Meson migration, especially if the exploratory PR receives positive mentor feedback
