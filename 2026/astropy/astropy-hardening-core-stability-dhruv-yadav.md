# astropy: Hardening Astropy's Core Stability (Dhruv Yadav)

## Contributor Information

* **Name:** Dhruv Yadav
* **Time-zone:** IST (UTC+5:30)
* **Matrix Handle:** @dhruv1955:matrix.org
* **Github username:** dhruv1955
* **Blog:** https://dev.to/dhruv1955
* **Blog RSS feed:** https://dev.to/feed/dhruv1955
* **Proposal discussion link:** https://github.com/OpenAstronomy/gsoc-proposals/pull/4

### PR link(s): Pre-GSoC Contributions (12 PRs submitted, 9 merged)

**Open (2)**

* [#19378 TST: Add direct tests for ks_2samp Cython extension in stats._stats](https://github.com/astropy/astropy/pull/19378)
* [#19377 TST: Add direct tests for _sigma_clip_fast C extension](https://github.com/astropy/astropy/pull/19377)

**Merged (9)**

* [#19410 TST: add direct tests for _parse_times C extension in time](https://github.com/astropy/astropy/pull/19410)
* [#19407 TST: add direct tests for scalar_inv_efuncs Cython extension in cosmology](https://github.com/astropy/astropy/pull/19407)
* [#19376 DOC: add examples for masked=False behavior in sigma_clip](https://github.com/astropy/astropy/pull/19376)
* [#19372 BUG: fix pickling of SigmaClip when Bottleneck is installed](https://github.com/astropy/astropy/pull/19372)
* [#19368 BUG: Fix missing goto fail in create_parser after ValueError in parse_times.c](https://github.com/astropy/astropy/pull/19368)
* [#19360 BUG: Fix numpy.ediff1d returning incorrect unit for magnitudes](https://github.com/astropy/astropy/pull/19360)
* [#19358 BUG: Fix AssertionError for numpy bool index in _handle_index_argument](https://github.com/astropy/astropy/pull/19358)
* [#19357 TST: replace TODO-skipped get_formats tests with real coverage](https://github.com/astropy/astropy/pull/19357)
* [#19351 BUG: Fix return vs raise in ShapedLikeNDArray.take() when out is passed](https://github.com/astropy/astropy/pull/19351)

**Closed without merge (1)**

* [#19359 BUG: Replace assert with RuntimeError in units/quantity_helper/function_helpers.py](https://github.com/astropy/astropy/pull/19359)

---

## Background

I'm a 3rd-year B.Tech student in Engineering Physics at NIT Agartala, with a strong foundation in C and C++. I also have industry experience building a safety management system at Tata Power Delhi Distribution Limited - my first taste of what it means to write code that cannot fail silently.

That interest in what happens under the hood led me to Astropy. When I dug into the codebase, I found C and Cython extensions that were never tested directly - meaning bugs in the compiled layer could be masked by the Python API indefinitely. It's a real problem worth fixing, and it's what I've been working on since February 2026, with 9 of 12 submitted PRs already merged.

---

## Interest in OpenAstronomy

I'm drawn to infrastructure work that users never see but always depend on. Astropy's compiled layer is exactly that - C and Cython extensions that sit beneath the Python API and handle the performance-critical paths that astronomers rely on daily. When those extensions go untested, bugs can hide there indefinitely. That's the kind of problem I want to fix.

What keeps me invested in OpenAstronomy specifically is the scale of real-world use. A bug I catch in `_parse_times.c` or `fast_sigma_clip.c` isn't an academic exercise - it affects researchers processing actual observational data. That connection between low-level engineering and scientific work is what makes this community worth contributing to long-term.

The draft Astropy Proposal for Enhancement ([neutrinoceros/astropy-APEs#1](https://github.com/neutrinoceros/astropy-APEs/pull/1)) also makes clear that direct extension-level testing isn't just cleanup work - it's a prerequisite for the compiled layer becoming independently modular and maintainable. This project fits directly into that roadmap, which means the work has durability beyond GSoC.

---

## Project Proposal

**Organisation:** OpenAstronomy (Astropy)

**Proposal Title:** Direct Extension-Level Testing for Astropy's Compiled Layer

**Project Size:** 175 hours (Medium)

**Mentors:** [@neutrinoceros](https://github.com/neutrinoceros), [@astrofrog](https://github.com/astrofrog), [@nstarman](https://github.com/nstarman)

---

### Summary

Astropy relies on performance-critical compiled extensions for time parsing, cosmology, and statistics. These extensions are currently tested only indirectly through the high-level Python API, which makes it hard to distinguish regressions in the compiled layer from regressions in the Python layer above it - and makes it impossible to test extension boundaries in isolation.

This project adds direct, low-level tests for critical C and Cython modules, ensuring compiled code is tested at the extension interface and not just through the API's observable behavior. This is a stated requirement of the proposed Astropy Enhancement Proposal ([neutrinoceros/astropy-APEs#1](https://github.com/neutrinoceros/astropy-APEs/pull/1)), which targets a more independently maintainable compiled layer. The work also lays groundwork for future infrastructure changes such as the Meson build migration ([astropy#17760](https://github.com/astropy/astropy/issues/17760)).

By making extension behavior directly testable, this work increases maintainer confidence during refactors and reduces the risk of silent regressions across release cycles.

**Success criteria:**
At least one extension-path test and one error-path test per target module, all passing on Linux, macOS, and Windows CI. Success is also measured by the number of previously untested exported symbols that gain direct coverage - the current baseline across the four target modules is zero.

---

### Technical Challenges Already Encountered

These aren't anticipated challenges - I've already hit them:

- In the time parser, I found a missing `goto fail` in the error cleanup path and fixed it in [#19368](https://github.com/astropy/astropy/pull/19368)
- To test `_parse_times` directly, I had to deduce the ufunc calling convention from internal usage of `TimeString.get_jds_fast` and manually construct the `dt_pars` and `dt_u1` structured dtypes - without touching the public `Time` constructor
- For the KS test in `_stats.pyx`, I used analytical expected values rather than cross-checking against SciPy, keeping the tests self-contained
- A closed PR ([#19359](https://github.com/astropy/astropy/pull/19359)) taught me Astropy's distinction between internal invariants (`assert`) and user-facing errors (`ValueError`, `TypeError`) - this directly shapes what belongs in extension-level tests
- The cosmology extension tests ([#19407](https://github.com/astropy/astropy/pull/19407), already merged) cover all 6 scalar inverse E(z) functions with analytical values and pass CI on Linux, macOS, and Windows - this PR now serves as the template for the remaining modules

---

### Deliverables

1. Direct tests for `astropy/time/_parse_times.c` - `create_parser` ufunc, structured dtype inputs, ISO/ISOT/yday parsing correctness, error path coverage — **already merged** ([#19410](https://github.com/astropy/astropy/pull/19410))
2. Direct tests for `astropy/stats/src/fast_sigma_clip.c` - all flag combinations, empty and all-masked inputs, edge cases
3. Direct tests for `astropy/stats/_stats.pyx` - `ks_2samp` tested against analytical expected values, no scipy dependency
4. Direct tests for `astropy/cosmology/_src/flrw/scalar_inv_efuncs.pyx` - all 6 scalar inverse E(z) functions, **already merged** ([#19407](https://github.com/astropy/astropy/pull/19407))
5. A short contributor guide on testing compiled extensions - covering how to construct structured dtypes, call ufuncs directly, and write error path tests, so future contributors don't have to rediscover this from scratch

**Stretch goals — Tier A:**

6. Direct tests for `astropy/table/_column_mixins.pyx` - `__getitem__` overrides, structured array field access, multi-dimensional slicing
7. Direct tests for `astropy/table/_np_utils.pyx` - `join_inner` covering inner, left, right, and outer joins; masks; empty tables; duplicate keys
8. Direct tests for `astropy/convolution/_convolve.pyx` - numerical accuracy vs. a pure Python reference, boundary conditions, padding modes
9. Direct tests for `astropy/io/ascii/cparser.pyx` - format correctness, malformed input, encoding errors
10. Direct tests for `astropy/timeseries/periodograms/lombscargle/cython_impl.pyx` and `bls/_impl.pyx` - numerical accuracy, sparse data edge cases

Tier A begins once deliverables 1-3 are merged or approved.

**Stretch goals — Tier B:**

11. Direct tests for `astropy/wcs` C extensions (wcslib)
12. Direct tests for `astropy/coordinates` C/Cython extensions
13. Direct tests for `astropy/utils/xml/iterparse.c` and `astropy/io/votable/src/tablewriter.c`

Tier B begins once 80% of Tier A is merged. Two infrastructure items - Meson build migration ([#17760](https://github.com/astropy/astropy/issues/17760)) and Limited API compatibility ([#19249](https://github.com/astropy/astropy/issues/19249)) - are related but out of scope; I've noted them only because they depend on this work being done first.

---

### Timeline

| Period | Description |
| --- | --- |
| **Community Bonding** | First priority is getting CI green on all three platforms - Linux, macOS, Windows - before a single test is written. I'll map all untested C/Cython extensions across the target subpackages (exported symbols, ufunc signatures, dtype requirements) and walk through my findings with mentors. The goal is to leave this period with a shared, written definition of "done" for each module so there's no ambiguity when coding starts. |
| **Week 1** | `_parse_times.c` tests are already merged ([#19410](https://github.com/astropy/astropy/pull/19410)), so this week starts on `astropy/stats` immediately. Draft PR for `fast_sigma_clip.c` opens on day one. Initial tests cover finite bounds, `use_median` vs. `use_mean`, and pre-masked input. |
| **Week 2** | Expand `fast_sigma_clip.c` coverage: `use_mad_std` flag, `max_iter=0`, single element, and all-masked input returning NaN. The flag combinations here are combinatorially large - I'll prioritise the paths most likely to regress silently. |
| **Week 3** | Begin `astropy/stats/_stats.pyx`. `ks_2samp` tests use analytical expected values - identical arrays (0.0), non-overlapping (1.0), partial overlap, different sizes - with no scipy dependency. Continue iterating on reviewer feedback for the `fast_sigma_clip` PR in parallel. |
| **Week 4** | Wrap up both stats PRs. My goal is merged, not just open. I'll also write a short internal note on the calling conventions and dtype patterns found across `_parse_times`, `fast_sigma_clip`, and `_stats` - this becomes the reference document for all remaining subpackages so I'm not re-deriving the same things from scratch each time. |
| **Week 5** | With stats PRs in review, begin the contributor guide (deliverable 5) - documenting the calling conventions, dtype construction patterns, and error path strategies found across `_parse_times`, `fast_sigma_clip`, and `_stats`. Writing this now while the patterns are fresh is better than leaving it to Week 12. |
| **Week 6 (Midterm)** | Buffer week for stats. Both stats PRs should be merged or in active review with passing CI by now. Midterm report submitted. If anything is still blocked, this is the week I escalate with mentors rather than quietly falling behind. |
| **Week 7** | Survey remaining Cython in `astropy/cosmology` beyond `scalar_inv_efuncs` and write numerical accuracy tests against known analytic integrals. I'm keeping this week scoped to cosmology only - coordinates is Tier B and I won't let it bleed in here just because the cosmology work finishes early. |
| **Week 8** | Begin `astropy/table`. Direct tests for `_column_mixins.pyx`: `__getitem__` overrides, structured array field access, multi-dimensional column slicing. I'll open the `_np_utils.pyx` PR in draft if the `_column_mixins` review is already moving, but I won't actively split attention between two unreviewed PRs. |
| **Week 9** | Complete `_np_utils.pyx`: `join_inner` for inner, left, right, and outer joins, masking behavior, empty tables, duplicate key edge cases. Begin `astropy/convolution/_convolve.pyx`: numerical accuracy against a pure Python reference implementation, boundary conditions, padding modes. |
| **Week 10** | Complete `_convolve.pyx`. Begin `astropy/io/ascii/cparser.pyx`: format correctness, malformed input, encoding error paths. If Tier A is on track I'll open a draft for `lombscargle/cython_impl.pyx` and `bls/_impl.pyx`, but these are stretch - I won't sacrifice polish on the core deliverables to hit them. |
| **Week 11** | Code freeze. Every open PR gets a full CI audit on Linux, macOS, and Windows. Platform-specific failures get fixed before anything else. No new features, no scope expansion - just making sure everything that's open is actually ready to merge. |
| **Week 12** | Final report: which extensions were tested, what approach worked, what remains uncovered, and a concrete roadmap for whoever picks this up next. Then final evaluation. This week also exists as a real buffer - if Week 11 uncovered something broken, Week 12 is where it gets fixed. |

**Weekly exit criteria:** Each week must include at least one extension-path test and one error-path test in the active PR. Linux CI must pass before the week ends; if macOS or Windows runs are delayed by queue issues, they must complete within 48 hours. I keep no more than 2 PRs active at any time - opening a third means something earlier isn't getting the review attention it needs.

## Expected Impact

- Right now, a regression in `_parse_times.c` or `fast_sigma_clip.c` can hide for months behind a passing Python API test. After this project, it won't - the compiled layer has its own tests that fail at the source.

- The calling conventions, dtype construction patterns, and error path strategies I'm documenting don't exist anywhere in Astropy's codebase today. Future contributors adding performance-critical code will have a reference instead of reverse-engineering everything from scratch like I had to.

- The draft APE can't move forward until the compiled layer is independently testable. This project removes that blocker - which means the Meson migration and Limited API work have a cleaner path forward after GSoC ends.

**Contingency:** If an unexpected blocker occurs, Tier A stretch goals are the first thing to drop - not the core deliverables. As long as deliverables 1-3 are merged or in active review with passing CI by midterm, the project is on track. Week 11 stays as code freeze regardless.

**Commitments:** My semester ends before May 10, and my summer break runs through July 15 - I have no competing commitments for the entire GSoC coding period and can start contributing during community bonding without waiting for exams to finish.

---

## Communication Plan

- My primary channel is Zulip, where I'll post 2-3 async updates per week covering what's done, what's blocked, and what's next. These aren't status reports for the sake of it - they're early-warning signals so mentors can redirect me before I lose a week going the wrong direction.

- I'll request a 30–60 minute weekly sync with mentors, with a written agenda shared the day before. This is where scope questions, timeline slippage, and design decisions get resolved - not via back-and-forth comment threads on a PR.

- All work happens through GitHub. Every module gets a draft PR on day one, not when I think it's ready. Mentors can see direction early and I get feedback before I've written 200 lines in the wrong direction. I keep no more than 2 PRs active at any time.

- If I'm blocked for more than 48 hours - whether by a CI issue, an unclear extension interface, or a stalled review - I escalate in Zulip immediately and flag it for the next weekly sync. I won't silently lose days.

- At midterm and final evaluation I'll submit a written report covering: which modules are complete, CI results across Linux/macOS/Windows, any timeline deviations and how I addressed them, and what remains for whoever continues this work.

---

## Personal Statement

### Past Experience

Since February 2026 I've submitted 12 PRs to Astropy, with 9 merged. The work spans bug fixes in C and Python, documentation, and direct extension-level tests. One closed PR ([#19359](https://github.com/astropy/astropy/pull/19359)) was particularly instructive — the review clarified Astropy's convention of using `assert` for internal invariants and `ValueError`/`TypeError` for user-facing errors, which directly shapes how I write extension-level tests.

I'm a 3rd-year Engineering Physics student at NIT Agartala with a strong foundation in C and numerical programming. At Tata Power Delhi Distribution Limited I built a safety management system as a full stack developer - my first experience writing code where silent failures have real consequences, which is exactly the failure mode this project is trying to prevent in Astropy's compiled layer.

I placed All India Rank 128 in Engineers' Ring of Honor 2025. I also coordinate internships for 60+ students across 10+ companies at NIT Agartala - less relevant technically, but it's taught me how to manage multiple parallel workstreams without dropping any of them, which matters for a project where several PRs will be in review simultaneously.

### Motivation: Why This Project?

I've spent the last few months reading Astropy's compiled layer not as a user but as someone trying to understand what could go wrong and where. That's what led me to the missing `goto fail` in `parse_times.c` - not a code audit, just genuine curiosity about what the error path actually does. That kind of work, finding the gap between what code is supposed to do and what it actually does at the boundary, is what I want to spend my time on.

The reason this project matters beyond GSoC is that the compiled layer can't become independently modular until it's independently testable. The draft APE makes this explicit - the path to a cleaner build system and eventual Limited API compatibility runs directly through having tests that don't depend on the Python API to exercise the C layer. I understand this not just from reading the proposal but from hitting the problem directly: when I tried to test `_parse_times` without touching the `Time` constructor, I had to reverse-engineer the calling convention from internal usage. That friction is exactly what this project removes for every contributor who comes after.

I'm not applying because this fits my skills. I'm applying because I've already started and I want to finish it properly.

---

### Why Me?

The short answer is that I've already done this work. I derived the `_parse_times` calling convention from internal usage of `TimeString.get_jds_fast` without any documentation to guide me, found and fixed a real error-path bug in the process ([#19368](https://github.com/astropy/astropy/pull/19368)), and got the cosmology extension tests merged and passing on all three platforms ([#19407](https://github.com/astropy/astropy/pull/19407)). That PR now exists as a working template for every module that follows.

I've also worked directly with the mentors through PR review - not just read their names on the project page. I know what they push back on (the `assert` vs `ValueError` distinction from [#19359](https://github.com/astropy/astropy/pull/19359) is a good example), and I've adjusted how I write tests accordingly. That feedback loop is already running. A new contributor would spend the first month of GSoC building what I've already built.

What I'm proposing isn't a plan to do something - it's a plan to finish something that's already started, with evidence that the approach works on real code, real reviews, and real CI.

### Availability

My semester ends before May 10 and my summer break runs through July 15, covering the entire GSoC coding period without interruption. I can contribute 20-25 hours per week and more during the early weeks when the hardest module (`_parse_times`) is the focus. I have no exams, internships, or other commitments during this window.

---

## GSoC

- **Have you participated in GSoC before?** No.

- **Are you applying to other projects?** No.

- **Schedule availability:** I'll flag any planned absences to mentors via Zulip in advance. Nothing is currently planned during the coding period.

