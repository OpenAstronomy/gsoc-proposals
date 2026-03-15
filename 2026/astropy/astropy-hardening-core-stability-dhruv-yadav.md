# astropy: Hardening Astropy's Core Stability (Dhruv Yadav)

## Contributor Information

* **Name:** Dhruv Yadav
* **Time-zone:** IST (UTC+5:30)
* **Matrix Handle:** @dhruv1955:matrix.org
* **Github username:** dhruv1955
* **Blog:** https://dev.to/dhruv1955
* **Blog RSS feed:** https://dev.to/feed/dhruv1955
* **Proposal discussion link:** https://github.com/OpenAstronomy/gsoc-proposals/pull/4

### PR link(s): Pre-GSoC Contributions (12 PRs submitted, 8 merged)

**Open (3)**
* [#19410 TST: add direct tests for _parse_times C extension in time](https://github.com/astropy/astropy/pull/19410)
* [#19378 TST: Add direct tests for ks_2samp Cython extension in stats._stats](https://github.com/astropy/astropy/pull/19378)
* [#19377 TST: Add direct tests for _sigma_clip_fast C extension](https://github.com/astropy/astropy/pull/19377)

**Merged (8)**
* [#19407 TST: add direct tests for scalar_inv_efuncs Cython extension in cosmology](https://github.com/astropy/astropy/pull/19407)
* [#19376 DOC: add examples for masked=False behavior in sigma_clip](https://github.com/astropy/astropy/pull/19376)
* [#19372 BUG: fix pickling of SigmaClip when Bottleneck is installed](https://github.com/astropy/astropy/pull/19372)
* [#19368 BUG: Fix missing goto fail in create_parser after ValueError in parse_times.c](https://github.com/astropy/astropy/pull/19368)
* [#19360 BUG: Fix numpy.ediff1d returning incorrect unit for magnitudes](https://github.com/astropy/astropy/pull/19360)
* [#19358 BUG: Fix AssertionError for numpy bool index in _handle_index_argument](https://github.com/astropy/astropy/pull/19358)
* [#19357 tests(io.registry): replace TODO-skipped get_formats tests with real coverage](https://github.com/astropy/astropy/pull/19357)
* [#19351 BUG: Fix return vs raise in ShapedLikeNDArray.take() when out is passed](https://github.com/astropy/astropy/pull/19351)

**Closed without merge (1)**
* [#19359 BUG: Replace assert with RuntimeError in units/quantity_helper/function_helpers.py](https://github.com/astropy/astropy/pull/19359)

---

## Background

I've always been curious about what's happening under the hood. Studying Engineering Physics at NIT Agartala meant spending a lot of time with spectral analysis, coordinate transforms, and numerical methods, and naturally that led me to Astropy. When I looked deeper into the codebase, I found real C and Cython extensions that had never been tested directly. A bug in the compiled layer could sit undetected behind the Python API for a long time. That felt like something worth fixing, and I've been working on it since February 2026.

I'm a 3rd-year B.Tech student in Engineering Physics at NIT Agartala. My coursework gave me solid C and C++ fundamentals. Beyond academics, I worked as a Full Stack Developer at Tata Power Delhi Distribution Limited, building a production safety management system. That experience taught me what it means to write code that actually has to work reliably. Three years of scientific Python and 12 Astropy contributions have given me the codebase context I need for this project.

---

## Interest in OpenAstronomy

Astropy is infrastructure that astronomers around the world depend on quietly every day. I'm drawn to the kind of work that doesn't always get noticed but makes everything else more reliable. Testing compiled extensions that have never had direct coverage, finding the memory safety bug that no public API test would catch, documenting calling conventions so the next contributor doesn't have to reverse-engineer them from scratch. OpenAstronomy's approach of building open, community-maintained scientific tools is exactly the kind of software development I want to be part of long-term. This project is where I can contribute most concretely right now, and the proposed APE makes the long-term direction clear: a reliably independent, testable compiled layer that the whole ecosystem can build on.

---

## Project Proposal

**Organisation:** OpenAstronomy (Astropy)

### Summary

Right now, if a bug is introduced in `create_parser` inside `_parse_times.c`, the only way it surfaces is through a failure in `Time('2020-01-01').jd`, a public API call several layers up. The extension itself has no direct test.

Astropy is a mixed-language codebase. Most of it is pure Python, but many hotpaths are written in C, C++, and Cython. These compiled extensions handle performance-critical operations like time string parsing, cosmological distance integrals, and statistical functions. They are currently tested only indirectly through the public Python API, which means a regression in the compiled layer can be masked by the Python layer above it. The compiled layer also contributes significantly to CI runtime because it requires recompilation even when the underlying code rarely changes.

This project builds a dedicated low-level test suite that exercises each extension directly, independently of the public API. That is a necessary first step for the proposed APE ([neutrinoceros/astropy-APEs#1](https://github.com/neutrinoceros/astropy-APEs/pull/1)), which proposes splitting Astropy's compiled extensions into a separate package. That split is only safe if each extension has tests that do not depend on the rest of Astropy. The work also lays groundwork for the Meson migration ([astropy#17760](https://github.com/astropy/astropy/issues/17760)) and Limited API compatibility ([astropy#19249](https://github.com/astropy/astropy/issues/19249)).

---

### Technical Challenges Already Encountered

Working through three subpackages of this problem before applying has surfaced the hard parts early. In `time/_parse_times.c`, I read the source carefully enough to find a missing `goto fail` that would leak memory on a parse error. That became [PR #19368](https://github.com/astropy/astropy/pull/19368). Testing the `create_parser` ufunc directly required figuring out its calling convention by studying how `TimeString.get_jds_fast` invokes it, then constructing the right `dt_ymdhms` structured dtype by hand rather than through the public `Time` constructor. For `stats/_stats.pyx`, I computed analytical expected values for the KS statistic rather than cross-checking against scipy, which means the test is sensitive to regression even if scipy changes. None of this was documented. It required reading the code.

[PR #19359](https://github.com/astropy/astropy/pull/19359) was closed without merge after review. It taught me that Astropy distinguishes between invariant violations (`assert`) and recoverable errors (`ValueError`) at the API boundary, a distinction that matters when deciding what to test at the extension level.

---

### Deliverables

**Minimum deliverables:**

1. Direct tests for `astropy/time/_parse_times.c`: `create_parser` ufunc, structured dtype inputs, parser correctness for ISO/ISOT/yday formats, and error path coverage
2. Direct tests for `astropy/stats/src/fast_sigma_clip.c`: all flag combinations, empty input, all-masked input, and edge cases
3. Direct tests for `astropy/stats/_stats.pyx`: `ks_2samp` with analytical expected values and no scipy dependency
4. Direct tests for `astropy/cosmology/_src/flrw/scalar_inv_efuncs.pyx`: all 6 scalar inverse E(z) functions (already merged: [#19407](https://github.com/astropy/astropy/pull/19407))
5. A written final report covering which extensions were tested, what strategies worked, what was difficult, and what remains with a concrete path forward

**Stretch goals:**

6. Direct tests for `astropy/table` C extensions (column shim)
7. Direct tests for `astropy/wcs` C extensions (wcslib)
8. Direct tests for `astropy/coordinates` C/Cython extensions
9. Exploratory Meson build system migration ([astropy#17760](https://github.com/astropy/astropy/issues/17760))
10. Exploratory PEP 803/809 Limited API compatibility ([astropy#19249](https://github.com/astropy/astropy/issues/19249))

---

### Timeline

| Period | Description |
|---|---|
| **Community Bonding** | Set up dev environment. Validate CI on Linux, macOS, Windows. Map all untested C/Cython extensions: exported symbols, ufunc signatures, dtype requirements. Discuss testing strategy and priority order with mentors. Write a shared plan before coding starts. |
| **Week 1** | Start `time/_parse_times.c`. Open draft PR on day one. Write tests for `create_parser` with ISO, yday, and ISOT parsing. Confirm it returns `numpy.ufunc`. Validate `dt_pars`, `dt_u1`, and `dt_ymdhms` structured dtypes work correctly. |
| **Week 2** | Expand `_parse_times.c` coverage. Add parser output validation: parse known ISO strings and assert year, month, day, hour, minute, second values. Cover error paths: wrong size arrays, malformed input, and boundary date values. |
| **Week 3** | Audit full `astropy/time` subpackage for any remaining compiled code not yet covered. Write tests for any additional C or Cython extensions found. Iterate on review feedback for the Week 1 to 2 PR. |
| **Week 4** | Buffer for time subpackage. Finalise and merge the time PR. Write a short internal note on calling conventions and dtype patterns found. This will serve as a reference for all remaining subpackages. |
| **Week 5** | Begin `astropy/stats`. Write direct tests for `fast_sigma_clip.c`: basic finite bounds, use_median vs use_mean, pre-masked input, use_mad_std flag, max_iter=0, single element, and all-masked input returning NaN. |
| **Week 6 (Midterm)** | Complete `astropy/stats/_stats.pyx`. Write `ks_2samp` tests with analytical expected values for identical arrays (0.0), non-overlapping (1.0), partial overlap, and different sizes. No scipy dependency. Submit midterm report. |
| **Week 7** | Begin `astropy/cosmology` beyond `scalar_inv_efuncs`. Survey remaining Cython functions. Write numerical accuracy tests against known analytic integrals where applicable. |
| **Week 8** | Begin `astropy/coordinates` survey. Map erfa integration points and identify directly testable symbols. Write initial tests for the most isolated entry points. |
| **Week 9** | Begin `astropy/table` C extensions (column shim). Study how the shim interacts with numpy arrays. Write tests for direct array operations that bypass the Python Table API. |
| **Week 10** | Begin `astropy/wcs` C extensions. Focus on isolated symbol-level tests rather than full pipeline coverage. Start stretch goals if ahead of schedule. |
| **Week 11** | Code freeze. Audit all PRs for CI pass on Linux, macOS, and Windows. Fix any platform-specific issues. No new features, only stabilisation. |
| **Week 12** | Write final report: which extensions were tested, what approach worked, what remains uncovered, and a roadmap for whoever continues this work. Submit final evaluation. |

**Hours per week:** 30 hours

**Commitments:** My semester ends before May 10th and my summer break runs through mid-July, giving me a clean availability window that covers the entire GSoC coding period. I'll be fully available from day one.

---

### Communication Plan

- **Weekly sync with mentors** (30 to 60 min video call) to review progress, discuss blockers, and adjust priorities if needed
- **Async updates on Astropy's Zulip** a few times per week. Zulip is Astropy's primary community channel where mentors are active daily, so questions get answered in context and discussions are visible to the broader team
- **GitHub** for all code-related discussion. One draft PR per extension module, opened early so mentors can guide the approach before the work is complete rather than reviewing a finished product
- **Email** for anything that needs a longer or more structured discussion outside of a scheduled call
- **Midterm and final written reports** summarising what was tested, what worked, what didn't, and what remains

---

## Personal Statement

### Past Experience

I've been programming in Python and C++ for about 3 years, with a focus on scientific computing and systems-level problem solving. My Engineering Physics coursework gave me strong C fundamentals. Outside academics, I worked as a Full Stack Developer at Tata Power Delhi Distribution Limited, building a production safety management system using the MERN stack. That experience taught me what it means to write code that actually has to work reliably under real-world conditions.

I ranked AIR 128 in the Engineers' Ring of Honor 2025 out of thousands of participants, and I currently serve as Internship Coordinator at NIT Agartala, managing placements for 60+ students across 10+ companies. I mention these not to pad a list but because they reflect how I work: I follow through, I communicate clearly, and I take responsibility seriously.

Since February 2026 I've submitted 12 PRs to Astropy, with 8 already merged, covering bug fixes in C and Python, documentation, and low-level extension tests. Not every PR made it in. [#19359](https://github.com/astropy/astropy/pull/19359) was closed after review, which sharpened my understanding of how the codebase reasons
about internal vs. external error handling.

### Motivation: Why This Project?

What draws me to this project is that it sits right at the intersection of two things I genuinely enjoy: low-level systems reasoning and long-term software design. The question of how you test a compiled ufunc directly, without going through the Python wrapper, is interesting to work through. And the answer matters. Every extension I add tests for becomes something that can be safely refactored, migrated to a new build system, or split into a separate package without fear.

I've read the draft APE and the Meson migration discussion. I understand where this work fits in the bigger picture, and I find that bigger picture compelling: a more modular, independently testable Astropy that's easier to maintain for everyone who depends on it.

### Why Me?

I've already done a version of this work across three subpackages. In `time/_parse_times.c`, figuring out how to call `create_parser` directly meant reading `TimeString.get_jds_fast` carefully enough to understand its calling convention. None of that was documented. Along the way I found a missing `goto fail` that would leak memory on a parse error and fixed it in [PR #19368](https://github.com/astropy/astropy/pull/19368). For `stats/_stats.pyx`, I computed analytical expected values for the KS statistic from scratch rather than just cross-checking against scipy, which means the test catches regressions even if scipy changes.

I don't need to figure out the approach from scratch. I need to apply what I've already learned to the remaining subpackages, handle whatever new complexity each introduces, and document the patterns clearly enough that the next contributor doesn't have to reverse-engineer them. I've worked directly with neutrinoceros, astrofrog, and nstarman through PR review, so I know what they expect in terms of code quality and test design.

### Availability

My semester ends before May 10th and my summer break runs through mid-July, giving me a clean availability window that covers the entire GSoC coding period. No competing commitments. I can put in 30 hours a week consistently.

---

## GSoC

**Have you participated previously in GSoC?** No, this is my first time applying.

**Are you also applying to other projects?** No.

**Schedule Availability:** My semester ends before May 10th with a summer break through mid-July. No other commitments during the GSoC coding period. I will communicate any short unavailability to mentors in advance via Zulip or email.
