# astropy: Hardening Astropy's Core Stability (Dhruv Yadav)

## Contributor Information

* **Name:** Dhruv Yadav
* **Time-zone:** IST (UTC+5:30)
* **Matrix Handle:** @dhruv1955:matrix.org
* **Github username:** [dhruv1955](https://github.com/dhruv1955)
* **Blog:** https://dev.to/dhruv1955
* **Blog RSS feed:** https://dev.to/feed/dhruv1955
* **Proposal discussion link:** https://github.com/OpenAstronomy/gsoc-proposals/pull/4

### PR link(s): Pre-GSoC Contributions (14 PRs submitted, 10 merged)

**PR link(s):**

**Open (3)**
* [Add direct tests for ks_2samp Cython extension in stats._stats (#19378)](https://github.com/astropy/astropy/pull/19378)
* [Add direct tests for _sigma_clip_fast C extension (#19377)](https://github.com/astropy/astropy/pull/19377)
* [Add direct tests for _convolveNd_c Cython extension (#19455)](https://github.com/astropy/astropy/pull/19455)

**Merged (10)**
* [Add direct tests for _parse_times C extension in time (#19410)](https://github.com/astropy/astropy/pull/19410)
* [Fix missing dereference in use_mad_std allocation check in fast_sigma_clip.c (#19457)](https://github.com/astropy/astropy/pull/19457)
* [Add direct tests for scalar_inv_efuncs Cython extension in cosmology (#19407)](https://github.com/astropy/astropy/pull/19407)
* [Add examples for masked=False behavior in sigma_clip (#19376)](https://github.com/astropy/astropy/pull/19376)
* [Fix pickling of SigmaClip when Bottleneck is installed (#19372)](https://github.com/astropy/astropy/pull/19372)
* [Fix missing goto fail in create_parser after ValueError in parse_times.c (#19368)](https://github.com/astropy/astropy/pull/19368)
* [Fix numpy.ediff1d returning incorrect unit for magnitudes (#19360)](https://github.com/astropy/astropy/pull/19360) 
* [Fix AssertionError for numpy bool index in _handle_index_argument (#19358)](https://github.com/astropy/astropy/pull/19358)
* [Replace TODO-skipped get_formats tests with real coverage (#19357)](https://github.com/astropy/astropy/pull/19357) 
* [Fix return vs raise in ShapedLikeNDArray.take() when out is passed (#19351)](https://github.com/astropy/astropy/pull/19351)

**Closed without merge (1)**
* [Replace assert with RuntimeError in units/quantity_helper/function_helpers.py (#19359)](https://github.com/astropy/astropy/pull/19359)

---

## Background:

I'm a 3rd-year B.Tech student in Engineering Physics at NIT Agartala, with a strong foundation in C and C++. I also have industry experience building a safety management system at Tata Power Delhi Distribution Limited - my first experience writing code where a missed error path had real operational consequences.

That interest in what happens under the hood led me to Astropy. When I dug into the codebase, I found C and Cython extensions that were never tested directly - meaning bugs in the compiled layer could be masked by the Python API indefinitely. It's a real problem worth fixing, and it's what I've been working on since February 2026, with 10 of 14 submitted PRs already merged.

---

## Interest in OpenAstronomy:

I started contributing to Astropy because I wanted to understand what the compiled layer actually does, not just what it's supposed to do. The `goto fail` I found in `parse_times.c` wasn't something I went looking for - I was just reading the error path to see whether it worked, and it didn't. That's the kind of reading I want to keep doing.

What keeps me engaged with OpenAstronomy is that the bugs here aren't academic. A regression in `_parse_times.c` or `fast_sigma_clip.c` affects researchers processing real observational data, not a toy benchmark. That raises the stakes enough to make the work feel worth doing carefully.

The draft APE makes it clear that direct extension-level testing is a prerequisite for the compiled layer becoming independently modular. The Meson migration and Limited API work that maintainers are planning both depend on it. This means the work has a clear path forward beyond GSoC, which is exactly the kind of contribution I want to make.

---

## Project Proposal:

**Organisation:** OpenAstronomy (Astropy)

**Proposal Title:** Direct Extension-Level Testing for Astropy's Compiled Layer

**Project Size:** 175 hours (Medium)

**Mentors:** [@neutrinoceros](https://github.com/neutrinoceros), [@astrofrog](https://github.com/astrofrog), [@nstarman](https://github.com/nstarman)

### Summary:

Astropy relies on performance-critical compiled extensions for time parsing, cosmology, and statistics. These extensions are currently tested only indirectly through the high-level Python API, making it impossible to distinguish a regression in the compiled layer from one in the Python wrapper above it – and impossible to test extension boundaries in isolation.

This project adds direct, low-level tests for these critical C and Cython modules. Two of the four core deliverables are already merged – `_parse_times.c` tests (#19410) and `scalar_inv_efuncs.pyx` tests (#19407) – both passing CI on Linux, macOS, and Windows. The remaining two (`fast_sigma_clip.c` and `_stats.pyx`) are in open PRs awaiting review.

This is a stated requirement of the draft APE (`neutrinoceros/astropy-APEs#1`). The Meson build migration (#17760) and Limited API work (#19249) cannot move forward until the compiled layer is independently testable – this project removes that blocker.

Currently, a regression in `_parse_times.c` or `fast_sigma_clip.c` can remain undetected for months behind a passing Python API test. After this project, it cannot.

**Success criteria:** At least one extension-path test and one error-path test per target module, all passing on Linux, macOS, and Windows CI. The current baseline of directly tested exported symbols across the four target modules is zero.

**Key Constraint:** Tests must call the extension directly without touching the public Python API. Testing `_parse_times` required manually reconstructing the `dt_pars` and `dt_u1` structured dtypes from internal source – no documentation exists for this calling convention. That is what makes the work both hard and worth doing.

---

## Technical Challenges Already Encountered:

These aren't anticipated challenges - I've already hit them:

- **Missing `goto fail` in `_parse_times.c` (#19368, MERGED):**  
While reading the error paths in `parse_times.c`, I found that `create_parser` had an unhandled cleanup path after a `ValueError` – the parser object was not being released. I fixed this in #19368 before writing a single test. You cannot write a correct error-path test for a function that leaks memory on that path.

- **Undocumented ufunc calling convention:** To test `_parse_times` directly without touching the public `Time()` constructor, I had to deduce the calling convention from internal usage of `TimeString.get_jds_fast` — no documentation exists for this. I manually reconstructed the `dt_pars` and `dt_u1` structured dtypes from the C source. The resulting test is now merged (#19410) and serves as the template for all future modules.

```python
pars = np.zeros(7, dtype=_parse_times.dt_pars)

pars['delim'] = np.asarray(p['delims'], dtype=np.uint8).view('S1')
pars['start'] = p['starts']
pars['stop']  = p['stops']
pars['break_allowed'] = p['break_allowed']
```

- **Three Untested Code Paths in `fast_sigma_clip.c`:**  
Reading `fast_sigma_clip.c` directly (not through the API), I identified three paths with no direct test coverage:
  - The `mad_buffer` lazy allocation: it only allocates on the first outer-loop iteration where `use_mad_std=True` is active. A batched input where only later rows need `mad_std` has never been tested directly.
  - The `count=0` path: sets both bounds to `NPY_NAN`. Only reachable when every input element is pre-masked. The Python API tests this implicitly but not at the C boundary.
  - The `data_buffer` malloc failure path: sets `PyErr_NoMemory()` and returns, but no test verifies this propagates correctly to Python rather than silently returning garbage.

I also found and fixed a missing dereference in the `use_mad_std` allocation check in 19457 (MERGED).

- **Fused Types in `_stats.pyx`:** The `ks_2samp` function uses a Cython fused type covering 11 numeric specializations (`uint8` through `longdouble`) – the same function dispatches to different compiled paths depending on input dtype. Tests verify the fused dispatch works correctly across type boundaries, not just the default `float64` path, using analytical expected values (identical arrays → 0.0; non-overlapping arrays → 1.0) with no SciPy dependency.

- **`assert` vs `ValueError` convention:** A closed PR (#19359) taught me Astropy's distinction between internal invariants and user-facing errors – for example, the malloc failure path in `fast_sigma_clip.c` is guarded by an assert-style early return, so testing it as a `ValueError` would be testing the wrong boundary entirely. This directly shapes which paths belong in extension-level tests vs. API-level tests.

- **Cross-platform CI template:** The cosmology extension tests (#19407, already merged) cover all 6 scalar inverse E(z) functions with analytical values and pass CI on Linux, macOS, and Windows – this PR now serves as the template for the remaining modules.

---

## Deliverables:

**Deliverable 1: `astropy/time/_parse_times.c` – Already Merged (#19410):**

- `create_parser` ufunc with structured dtype inputs (`dt_pars`, `dt_u1`)
- ISO, ISOT, and yday format parsing correctness
- Error path: wrong-size parameter array raises `ValueError` with correct message
- Returns a numpy ufunc instance (not a callable, not a Python function)

**Deliverable 2: `astropy/stats/src/fast_sigma_clip.c`**

The ufunc signature is `(n),(n),(),(),(),(),()->(),()`. Seven inputs, two outputs. Tests call the extension directly by importing `astropy.stats._fast_sigma_clip`, not through `SigmaClip()`.

- Flag combinations: `use_median=True/False` × `use_mad_std=True/False` (4 combinations)
- Lazy `mad_buffer` allocation: batched input where only later rows trigger `use_mad_std`, verifying buffer is allocated once and reused
- The `count=0` path: fully pre-masked input must return `(NaN, NaN)` for both bounds
- Edge cases: `max_iter=0` (no iteration), single-element array, asymmetric sigma (`sigma_low != sigma_high`)
- Error propagation: `PyErr_NoMemory()` on `data_buffer` malloc failure

**Deliverable 3: `astropy/stats/_stats.pyx`**

`ks_2samp` uses a Cython fused type with 11 numeric specializations. Tests use analytical expected values with no SciPy dependency.

- `float64`: identical arrays → 0.0; non-overlapping arrays → 1.0; partial overlap → known analytical value
- `int64` and `float32`: fused type dispatch verification across type boundaries
- Different array sizes (`n1 ≠ n2`): verifies the asymmetric step logic
- Empty-array edge case: `n=0` input

**Deliverable 4: `astropy/cosmology/_src/flrw/scalar_inv_efuncs.pyx` – Already Merged (#19407)**

- All 6 scalar inverse E(z) functions: `lcdm_inv_efunc_norel`, `lcdm_inv_efunc_nomnu`, `flcdm_inv_efunc_norel`, `flcdm_inv_efunc_nomnu`, `wcdm_inv_efunc_norel`, `fwcdm_inv_efunc_norel`
- Flat universe at z=0: E(0)=1, so `inv_efunc=1.0`
- Matter-dominated limit: `inv_efunc(z) = (1+z)^{−1.5}` analytically
- wCDM with `w0=−1` reduces to LambdaCDM
- Passing CI on Linux, macOS, and Windows

**Deliverable 5: Contributor Guide**

This guide documents what does not currently exist anywhere in Astropy's codebase:

- `dt_pars` / `dt_u1` reconstruction: How to reconstruct structured dtypes from `fast_parser_pars` without using the public `Time()` constructor — as demonstrated in `test_parse_times_extension.py`
- ufunc calling convention: The gufunc signature `(n),(n),(),()->(),()` used in `_fast_sigma_clip`, with a worked example
- `assert` vs `ValueError`: The PR #19359 distinction governing which paths belong in extension tests vs. API tests, with examples from `fast_sigma_clip.c`
- Fused type dispatch testing: How to write tests that verify Cython fused types dispatch correctly across multiple numeric specializations
- Analytical expected values: How to write self-contained tests without depending on external libraries like SciPy

---

## Stretch Goals – Tier A:

1. `astropy/table/_column_mixins.pyx`: `__getitem__` overrides, structured array field access, multi-dimensional slicing.
2. `astropy/table/_np_utils.pyx`: `join_inner` covering inner, left, right, and outer joins; masks; empty tables; duplicate keys.
3. `astropy/convolution/_convolve.pyx`: Numerical accuracy vs. a pure Python reference, boundary conditions, padding modes.
4. `astropy/io/ascii/cparser.pyx`: Format correctness, malformed input, encoding errors.
5. `astropy/timeseries/periodograms/lombscargle/cython_impl.pyx` and `bls/_impl.pyx`: Numerical accuracy, sparse data edge cases.

## Stretch Goals – Tier B (begins after 80% of Tier A is merged)

1. `astropy/wcs` C extensions (wcslib): direct tests for the wcslib C extension interface – specifically `wcsp2s`/`wcss2p` round-trip accuracy
2. `astropy/coordinates` C/Cython extensions: coordinate transformation compiled extensions – specifically erfa ufunc round-trip accuracy for `s2p`/`p2s`
3. `astropy/utils/xml/iterparse.c` and `astropy/io/votable/src/tablewriter.c`: XML iteration and VOTable writing at the C interface – specifically round-trip write/parse correctness for integer and float columns

Two infrastructure items – Meson build migration (#17760) and Limited API compatibility (#19249) – are related but out of scope. I've noted them only because they depend on this work being done first.

---

## Timeline:

| Period | Description |
|---|---|
| **Community Bonding (May 1 - May 25)** | ➢ Get CI green on all three platforms (Linux, macOS, Windows) before writing a single new test<br>➢ Map all untested exported symbols across target subpackages<br>➢ Walk through findings with mentors<br>➢ Leave with a shared written definition of "done" for each module – no ambiguity when coding starts |
| **Week 1 (May 26 - Jun 1)** | ➢ `_parse_times.c` tests already merged (#19410) – start on stats immediately<br>➢ Draft PR for `fast_sigma_clip.c` opens on day one, following structure of #19407<br>➢ Initial tests cover finite bounds, `use_median` vs `use_mean`, and pre-masked input<br>➢ Ufunc signature `(n),(n),(),(),(),(),()->(),()` already understood from reading module init code |
| **Week 2 (Jun 2 - Jun 8)** | ➢ Expand `fast_sigma_clip.c` coverage: `use_mad_std` flag and lazy `mad_buffer` allocation (batched-input path)<br>➢ Cover `max_iter=0`, single element, and all-masked input returning NaN<br>➢ Prioritize flag combinations most likely to regress silently |
| **Week 3 (Jun 9 - Jun 15)** | ➢ Begin `_stats.pyx` – `ks_2samp` tests with analytical expected values: identical arrays (0.0), non-overlapping (1.0), partial overlap, different sizes<br>➢ Cover `float64`, `int64`, and `float32` for fused type dispatch verification<br>➢ No SciPy dependency<br>➢ Continue iterating on reviewer feedback for `fast_sigma_clip` PR in parallel |
| **Week 4 (Jun 16 - Jun 22)** | ➢ Wrap up both stats PRs – goal is merged, not just open<br>➢ Write internal note on calling conventions and dtype patterns found across `_parse_times`, `fast_sigma_clip`, and `_stats`<br>➢ This note becomes the reference document for all remaining subpackages |
| **Week 5 (Jun 23 - Jun 29)** | ➢ Begin contributor guide (Deliverable 5) while patterns are fresh<br>➢ Document `dt_pars` reconstruction, ufunc/gufunc calling convention, `assert` vs `ValueError` distinction, and fused type dispatch testing patterns |
| **Week 6 (Midterm) (Jun 30 - Jul 6)** | ➢ Buffer week for stats PRs – both should be merged or in active review with passing CI<br>➢ Submit midterm report<br>➢ If anything is blocked, escalate with mentors this week – not later<br>➢ Deliverables 1–4 all merged or in active review = on track |
| **Week 7 (Jul 7 - Jul 13)** | ➢ Survey remaining Cython in `astropy/cosmology` beyond `scalar_inv_efuncs`<br>➢ Write numerical accuracy tests against known analytic integrals<br>➢ Scope strictly limited to cosmology – coordinates is Tier B and will not bleed into this week |
| **Week 8 (Jul 14 - Jul 20)** | ➢ Semester resumes ~July 26 – reduce to 15–18 hours/week from this point<br>➢ Focus entirely on getting open PRs reviewed and merged<br>➢ No new modules started unless all core deliverables are in active review<br>➢ Begin `astropy/table/_column_mixins.pyx`: `__getitem__` overrides, structured array field access, multi-dimensional slicing |
| **Week 9 (Jul 21 - Jul 27)** | ➢ Complete `_np_utils.pyx`: `join_inner` for all 4 join types (0=inner, 1=outer, 2=left, 3=right), masking behavior, empty tables, duplicate key edge cases<br>➢ Begin `convolution/_convolve.pyx`: numerical accuracy against pure Python reference, boundary conditions, padding modes |
| **Week 10 (Jul 28 - Aug 3)** | ➢ Complete `_convolve.pyx`<br>➢ Begin `io/ascii/cparser.pyx`: format correctness, malformed input, encoding error paths<br>➢ If Tier A is on track, open draft for `lombscargle/cython_impl.pyx` and `bls/_impl.pyx` – stretch only, will not come at cost of core deliverables |
| **Week 11 (Aug 4 - Aug 10)** | ➢ Code freeze – no new features, no scope expansion<br>➢ Full CI audit on every open PR across Linux, macOS, and Windows<br>➢ Platform-specific failures fixed before anything else |
| **Week 12 (Aug 11 - Aug 17)** | ➢ Final report: which extensions were tested, what worked, what remains uncovered, roadmap for whoever continues<br>➢ Final evaluation<br>➢ Real buffer – if Week 11 uncovered something broken, this is where it gets fixed |

**Weekly exit criteria:** Each week must include at least one extension-path test and one error-path test in the active PR. Linux CI must pass before the week ends. if macOS or Windows runs are delayed by queue issues, they must complete within 48 hours. I keep no more than 2 PRs active at any time - opening a third means something earlier is not getting the review attention it needs.

---

## Expected Impact:

- **Silent regression prevention:** Right now, a regression in `_parse_times.c` or `fast_sigma_clip.c` can hide for months behind a passing Python API test. After this project, it cannot.
- **Contributor documentation:** The calling conventions, dtype construction patterns, and error path strategies I'm documenting don't exist anywhere in Astropy's codebase today. Future contributors adding performance-critical code will have a reference instead of reverse-engineering everything from scratch like I had to.
- **APE unblocked:** The draft APE can't move forward until the compiled layer is independently testable. This project removes that blocker - which means the Meson migration and Limited API work have a cleaner path forward after GSoC ends.

**Contingency:** If an unexpected blocker occurs, Tier A stretch goals are the first thing to drop - not the core deliverables. As long as deliverables 1-3 are merged or in active review with passing CI by midterm, the project is on track. Week 11 stays as code freeze regardless.

**Commitments:** My semester ends before May 10, and my summer break runs through July 26 - I have no competing commitments for the entire GSoC coding period and can start contributing during community bonding without waiting for exams to finish.

---

## Communication Plan:

- **Weekly updates:** Zulip is my primary channel. I'll post 2-3 updates a week - what's done, what's blocked, what's next. The goal is to catch wrong directions early before they cost a week.
- **Weekly sync:** I'll have a weekly 30-60 minute sync with mentors and share a short written agenda the day before.
- **Draft PRs from day one:** Every module will have a draft PR opened on day one, not when it feels "ready," so feedback comes before I go too far in the wrong direction.
- **48-hour escalation rule:** If I'm blocked for more than 48 hours - due to CI issues, unclear interfaces, or stalled reviews - I'll escalate on Zulip immediately instead of waiting.
- **Evaluation reports:** Midterm and final reports will cover completed work, CI results across Linux/macOS/Windows, any timeline deviations, and remaining work.

---

## Personal Statement:

### Past Experience:

Since February 2026 I've submitted 14 PRs to Astropy, with 10 merged. The work spans bug fixes in C and Python, documentation, and direct extension-level tests. One closed PR (#19359) was particularly instructive - the review clarified Astropy's convention of using `assert` for internal invariants and `ValueError`/`TypeError` for user-facing errors, which directly shapes how I write extension-level tests.

I'm a 3rd-year Engineering Physics student at NIT Agartala with a strong foundation in C and numerical programming. At Tata Power Delhi Distribution Limited I built a safety management system as a full stack developer - my first experience writing code where silent failures have real consequences, which is exactly the failure mode this project is trying to prevent in Astropy's compiled layer.

I placed All India Rank 128 in Engineers' Ring of Honor 2025. I also coordinate internships for 60+ students across 10+ companies at NIT Agartala - less relevant technically, but it's taught me how to manage multiple parallel work streams without dropping any of them, which matters for a project where several PRs will be in review simultaneously.

### Motivation: Why This Project?

The last few months, I've been reading through the compiled layer of Astropy, not as a user, but as someone who wants to know what could go wrong and where. The `goto fail` I found in `parse_times.c` is a concrete example of what happens when error paths go unread – that is the kind of reading I want to keep doing. This kind of work, trying to find the difference between what code should do and what it actually does at the edges, is what I'd like to spend my time on.

The reason this project is important outside of GSoC is that the compiled layer will not be able to become independently modular until it is independently testable. This is made clear in the draft APE - the way to a cleaner build system and ultimately Limited API compatibility is through testing that does not depend on the Python API for the C layer. I understand this not only through the proposal, but because I've run into it myself - when I was trying to test `_parse_times` without relying on the Python API for the `Time` constructor, I had to reverse-engineer the call convention from internal usage. This is precisely what this project removes for every contributor that comes after.

I'm not applying because I'm a good fit for this. I'm applying because I've already started and I want to complete it well.

### Why Me?

The quick summary is that I've already done this. I developed the `_parse_times` calling convention from internal usage of `TimeString.get_jds_fast` with no documentation available to me, fixed an actual error path bug in doing so (#19368), and got the cosmology extension tests merged and passing for all three platforms (#19407). That PR is now live as a working example for all future modules.

I've also worked directly with the mentors through PR review, not just seen their name on the project page. I'm familiar with what they push back on (the difference between `assert` and `ValueError`, for example, from #19359), and I've adjusted my approach to writing tests accordingly. This process is already underway. The groundwork that typically consumes the first month – understanding calling conventions, fixing latent bugs, getting CI green – is already done.

So what I'm proposing is not a plan for doing something, it is a plan for finishing something that has already been started, and I have proof that this approach works.

### Availability:

My semester ends before May 10, and after that, I have a complete summer break until July 26. Therefore, I can dedicate 20 to 25 hours per week to the project without any conflicting commitments, allowing me to focus on the implementation-heavy work during the early phase of the project. Once my semester starts after July 30, I will continue to contribute 15 to 18 hours per week, focusing on reviewing, refining, and merging open pull requests rather than starting any new modules unless all the core deliverables are already in active review status. I do not have any examinations, internships, or any leave during the coding period, and any conflicting issues will be communicated to the mentors in advance through Zulip.

---

## GSoC:

**Have you participated in GSoC before?** No.

**Are you applying to other projects?** No.

**Schedule availability:** I'll flag any planned absences to mentors via Zulip in advance. Nothing is currently planned during the coding period.

---

## Other comments :

- I will post weekly progress updates on my blog throughout the GSoC coding period.
- I am comfortable communicating on Zulip and GitHub and can adjust my working hours to overlap with mentor availability if needed.