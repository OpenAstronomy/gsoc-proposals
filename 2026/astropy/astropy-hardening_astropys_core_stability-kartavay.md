# Hardening Astropy's Core Stability

## Contributor Information
* **Name:** Kartavay
* **Time-zone:** IST (UTC+5:30)
* **Matrix/slack/IRC Handle:** @kartavay:matrix.org
* **Github/forge username:** https://github.com/kartavayv
* **Blog:** https://kartavayv.github.io/
* **Blog RSS feed:** https://kartavayv.github.io/feed.xml
* **PR link(s):**
  * https://github.com/astropy/astropy/pull/19214 (merged)
  * https://github.com/astropy/astropy/pull/19110 (open)

### Background
I am a college student pursuing Bachelors in Computer Science. I am currently in my 2nd year. Programming and Astronomy have been my deep interests since I was 14 (pre GPT era). Building things yourself was the only way. That same instinct - understanding things at the lowest level - eventually led me to Astropy.

I started studying Astropy code around last year's November with the aim of Adding a [LaTeX reader](https://github.com/astropy/astropy/issues/14837). It took me around a month to guide myself through the complexity of Astropy codebase, understand it, and ultimately make my first [PR](https://github.com/astropy/astropy/pull/19110) for it. While working on it I repeatedly encountered modules functioning at very low level. These modules sit behind almost every functionality making visible changes. Seeing these functions again and again gave me a sense of what low level looks like.

One bottleneck that I encountered while building this reader was the parsing of 1D vector like strings into a Quantity object. Initially I tried to solve it by using a rather patchy workaround, but nstarman suggested me to make another [PR](https://github.com/astropy/astropy/pull/19214) which allows this parsing. Working on this PR taught me how to "actually" work in a scientific codebase while aligning myself with the library design. Another major learning of this was on how to communicate ideas properly such that real progress can be made with the minimum amount of friction possible. Interaction and feedback from maintainers (nstarman and mhvk) sharpened both my programming skills and taught me what production code looks like in scientific library.

### Interest in OpenAstronomy
I have an interest in Astronomy since childhood. I used to read a lot about it. OpenAstronomy allows me to contribute to real scientific progress using the skills I have. It seems to me like an opportunity that I shouldn't miss. This was also a motivation when I first started to work on the [LaTeX reader](https://github.com/astropy/astropy/issues/14837) PR in November, and it seems that it has kept me going up until now.


## Project Proposal Application
**Proposal Title:** Hardening Astropy's Core Stability

**Organisation:** Astropy / OpenAstronomy

### **Summary:**
If a bug is introduced in `join_inner` inside `_np_utils.pyx`, the only way it surfaces today is through a failure in `Table.join()` - a public API call several layers up. The extension itself has no direct test. During preparation for this proposal, I found a similar silent failure in `ks_2samp` (`_stats.pyx`): unsorted input returns `0.5` instead of the correct `0.25` with no warning raised.

Astropy's codebase can be mainly divided into two layers. The primary layer is written in Python and is more specifically a Public API, the second is a low-level layer written in Cython, C and C++. As described in the project description, most of the development occurs in the user facing API written in Python and the development in low-level layer, which is not intended as "user-facing", are much more slow paced. Its code almost never changes.

But this poses a problem, which I have faced myself, many times while making a PR, whenever a PR is made CI spends a lot of time (about 20-50%) in recompiling the code of this slowly evolving low-level layer, which primarily stays the same. This re-compilation of these extension modules across all the CI jobs wastes a lot of CPU and developer time, which isn't cured by running tests locally either. A solution is proposed to this very problem in this [APE](https://github.com/neutrinoceros/astropy-APEs/pull/1/changes), which proposes pushing all the extension modules (written in C, C++ and Cython) up the stack into a separate package. 

Naturally this package would require tests. However, we cannot just simply "relocate" high level API tests to this new package. It is so because these tests are sharply aligned with the Public API and therefore solely concerned with making checks on user facing behavior. They rarely intersect with the low-level layer. This poses a serious need of writing tests for this layer and this project addresses that gap by designing tests for this low-level layer which can then support the architectural separation as described in the APE.

### Deliverables

**1.** A dedicated test suite for Cython extension modules in `astropy/table/` (`_column_mixins.pyx`, `_np_utils.pyx`), covering edge cases and boundary conditions at the extension-function level. I will start by iterating on [#19492](https://github.com/astropy/astropy/pull/19492).

**2.** Tests for `scalar_inv_efuncs.pyx` in `astropy/cosmology/`, covering all 10 cosmology model families across three neutrino regimes. Tests will include numerical precision validation against known analytical limits, memory layout coverage (C-contiguous and Fortran-contiguous inputs), and regression tests ensuring consistency with the high-level cosmology API — motivated in part by a pre-existing silent bug discovered during PR [#19110](https://github.com/astropy/astropy/pull/19110), where `m_nu` parameters were silently dropped in `FlatLambdaCDM`.

**3.** Tests for `astropy/io/votable/src/tablewriter.c`, Astropy's C-level VOTable TABLEDATA writer. It is untested at the C extension level. Tests will cover fully and partially masked rows, `supports_empty_values` edge cases, empty string converter output, buffer growth under large inputs, and `indent`/`buf_size` clamping boundaries.

**4.** Tests for `astropy/time/src/parse_times.c`, covering: parsing failure modes (delimiter mismatch, non-digit input,truncated components), boundary-condition behaviour in index-based parsing, day-of-year to calendar date conversion across leap-year edge cases, and gufunc-level behaviour including vectorized inputs and mixed-validity arrays.

**5.** A contributor-facing document (added to Astropy's dev docs) mapping coverage gaps across all Cython/C extension modules, with recommendations for future testing targets so this work has a clear continuation path.

### Stretch Goals
**1.** Extend isolated testing coverage to `astropy/stats/` covering Cython modules (`_stats.pyx`) and C modules (`compute_bounds.c`, `fast_sigma_clip.c`, `wirth_select.c`) including dtype boundary conditions and behavior on pathological inputs (NaN, inf, zero-length arrays).
**2** Tests for `_convolve.c`: correctness of boundary modes (fill, wrap, extend, interpolate), NaN propagation and `nan_interpolate` behavior, including kernel-weight normalization when valid inputs are sparse, boundary-condition testing for index calculations, minimal-size and degenerate input validation, and consistency checks across 1D, 2D, and 3D convolution paths.

### Description/timeline

| Period | Description |
|--------|-------------|
| Community Bonding (May 1 – May 24) | Set up development and coverage environment. Study existing Astropy test patterns and CI behavior. Identify high-risk areas (dtype handling, memory layout, error propagation) across target modules. Finalize testing strategy and priorities with mentors. |
| Week 1 (May 25 – May 30) | Finish direct tests for `_column_mixins.pyx` by iterating on [#19492](https://github.com/astropy/astropy/pull/19492). I will begin by adding remaining edge cases and making improvements based upon the maintainer feedback. Then write direct tests for `_np_utils.pyx`. These tests will cover all four join types (inner, outer, left, right), masking logic, cartesian product correctness, and boundary conditions like empty tables, single-key groups,and many-to-many matches. |
| Week 2 (Jun 2 – Jun 6) | Write direct tests for `astropy/cosmology/_src/flrw/scalar_inv_efuncs.pyx` covering all 10 cosmology model families across its `_norel` and `_nomnu` variants. Tests will include z=0 normalization, matter-dominated analytical limits, and model reduction checks (e.g. w=-1 reduces wCDM to LambdaCDM). |
| Week 3 (Jun 9 – Jun 13) | Complete cosmology tests covering all 10 full massive neutrino variants, high-z behavior (z=10, z=1000) and negative redshift inputs. Add regression test for the `m_nu` silent drop bug which I discovered while working on PR [#19110](https://github.com/astropy/astropy/pull/19110). Carry-over from week 2 if needed. |
| Week 4–5 (Jun 16 – Jun 27) | Write tests for `astropy/io/votable/src/tablewriter.c`, focusing on extension-level behaviour of the TABLEDATA writer. I will write tests include handling of masked rows, behaviour of `supports_empty_values`, and validation of output formatting. Additional tests will explore boundary conditions in `indent` and `buf_size`. |
| Week 6 (Jun 30 – Jul 4) | Begin tests for `astropy/time/src/parse_times.c`, focusing on core parsing paths (valid ISO-like inputs) and common failure modes (e.g. delimiter mismatch, non-digit input, truncated components). Establish testing scaffolding for gufunc-based parsing. More complex edge cases will continue into the buffer week if needed. |
| Week 7 (Jul 7 – Jul 10) | Buffer and midterm preparation. Continue tests for `parse_times.c` (including day-of-year conversion and vectorized behaviour where applicable), address review feedback across open PRs, and fix regressions. Submit midterm evaluation. |
| Week 8–9 (Jul 13 – Jul 25) | Develop a contributor-facing coverage audit for Astropy’s Cython and C extension modules. This will involve mapping modules to current test coverage, identifying gaps, and outlining potential testing targets based on observed risk areas (e.g. dtype handling, memory layout, error propagation). |
| Week 10–11 (Jul 28 – Aug 15) | Work on stretch goals, including tests for `astropy/convolution/_convolve.c` and extended coverage for `astropy/stats/`. Focus will be on key behaviours such as boundary conditions, NaN handling, and consistency across different input configurations. Continue refining earlier test suites and addressing feedback. |
| Week 12 (Aug 17 – Aug 24) | Final PR cleanup, address remaining review feedback, ensure CI passes across all platforms. Submit final work product by Aug 24 deadline. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time applying to GSoC.

### Are you also applying to other projects?
No, this is the only project I am applying to this year.

### Schedule availability
I have college holidays from May through mid-June, covering the Community Bonding period and the start of coding. College resumes around mid-June with no exams or major commitments during the coding period. I am available full-time for this project.