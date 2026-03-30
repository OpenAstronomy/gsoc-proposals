
# Application Title

## Contributor Information

- **Name:** Abu Bakkar Siddique
- **Time-zone:** Asia/Karachi (UTC+05:00)
- **Matrix/Slack/IRC Handle:** @siddique (Astropy workspace)
- **GitHub username:** Abu-bakkar-siddique
- **Blog:** N/A
- **Blog RSS feed:** N/A
- **PR link(s):** https://github.com/astropy/astropy/pull/19470

---

## Background

I am a 3rd year Computer Science undergraduate from Lahore, Pakistan. I have been programming since 2023, and during this time I have developed a strong interest in open-source software and collaborative development.

Prior to my university studies, I completed my Intermediate in Computer Science (ICS), which gave me a solid foundation in programming fundamentals.


### Interest in OpenAstronomy


*Why do you want to work with us?*


My fascination with the universe goes back to childhood and has only grown over time. It has only been intensified by the moments like when I came across Cosmos: A Spacetime Odyssey (even without finishing it), and when a couple of years later, in 2019, I was gifted Brief Answers to the Big Questions by Stephen Hawking, which deepened my appreciation for the efforts that humanity is putting to understand the cosmos. These feelings reached a new peak when the James Webb Space Telescope released its first deep field image, which gave us a glimpse into the universe as it was billions of years ago(what a time to be alive).

I now find myself at an intersection of being a cosmophile, and a computer science student, willing to improve and contribute to the tools and software that help humanity understand the universe. 

Given all of this, contributing through code is the most meaningful way to contribute in this effort, and OpenAstronomy sits right at this intersection, which makes it an obvious natural choice for me.

---

## Project Proposal

**Proposal Title:** Hardening Astropy's Core Stability Through Direct Testing of Compiled Extensions

**Organisation:** OpenAstronomy (Astropy sub-org)

### Summary

Astropy's current test suites primarily validate behavior through the public Python API. As a result, the low-level compiled layer, covering C, Cython, and related extension modules, is often tested only indirectly. That usually works for user-facing functionality, but it leaves an important gap: low-level behavioral contracts are not always verified independently of the higher-level wrappers that call them.

This matters for two ongoing directions in the project:

1. the proposed decoupling of compiled extension modules into a standalone package, as discussed in the draft APE ([neutrinoceros/astropy-APEs#1](https://github.com/neutrinoceros/astropy-APEs/pull/1));
2. the continued Meson build migration effort ([astropy#17760](https://github.com/astropy/astropy/issues/17760)).

If extension modules are ever to be built, packaged, or evolved more independently, they need direct tests that verify their behavior at the extension boundary. Without such tests, it is difficult to determine whether a failure belongs to the compiled implementation itself, the Python wrapper layer, or the build system. Direct extension tests also create a stable behavioral baseline that makes it possible to verify correctness across build system changes or packaging separations, which is precisely the kind of confidence the APE work and Meson migration will require.

This project proposes to add focused `pytest` suites for Astropy's compiled extensions by importing and exercising the extension modules directly through their Python-visible interfaces. These tests will strengthen correctness guarantees, improve confidence during refactoring and build-system changes, and support future work on packaging separation.

---

## Why I Can Do This

I have already started contributing in this exact area of the codebase. My open pull request, [#19470](https://github.com/astropy/astropy/pull/19470), adds an independent test module for the `astropy.convolution._convolve` extension in the convolution subpackage. That work has given me direct experience with Astropy's workflow, the structure of its compiled modules, and the practical process of writing tests that exercise extension code directly rather than only through higher-level user-facing APIs.

Beyond this PR, my coursework and independent work have also given me relevant low-level background. I have worked with shared-memory parallelism in C using OpenMP, studied Python's C extension model including concepts such as `PyObject *`, `PyMethodDef`, and extension module compilation, and implemented numerical algorithms in both C and Python. This means I am not approaching compiled extensions as unfamiliar territory; I already have enough technical footing to read extension source code critically, understand what assumptions it makes, and reason about what its behavioral contracts actually are.

Most importantly, I have already begun learning how to approach this work in the right way: not by writing tests as quickly as possible, but by first understanding what each extension is responsible for and why it exists.

---

## Technical Approach

My approach begins with study before code. Before writing a single line of tests for any extension, I will first study the implementation carefully and discuss its core behavior with my mentor so I can clearly understand what purpose the extension serves, what assumptions it encodes, and how it can be tested most effectively. For a scientific project like Astropy, I believe this step is essential. The goal is not simply to increase coverage, but to ensure that the tests reflect the scientific and computational intent of the code.

For each extension module, I will identify its behavioral contracts, including accepted input types and dtypes, expected array shapes and memory-layout assumptions, numerical behavior and invariants, failure modes and raised exceptions, and boundary cases such as empty inputs, extreme values, or invalid combinations of arguments.

Once that understanding is in place, I will write focused `pytest` tests that import and exercise the compiled extension module directly through its Python-visible interface. Some modules expose direct functions, while others expose helper types, ufuncs, or lower-level classes, so the exact form of the tests will vary across modules. But the guiding principle will remain the same: test the compiled extension itself as directly as possible, independently of the higher-level public API.

---

## Deliverables

The core deliverable of this project is a direct `pytest` test module for each of Astropy's 16 compiled extension modules in this scope:

1. `astropy.convolution._convolve`
2. `astropy.cosmology._src.flrw.scalar_inv_efuncs`
3. `astropy.cosmology._src.signature_deprecations`
4. `astropy.io.ascii.cparser`
5. `astropy.io.fits._utils`
6. `astropy.io.fits.hdu.compressed._compression`
7. `astropy.io.votable.tablewriter`
8. `astropy.stats._fast_sigma_clip`
9. `astropy.stats._stats`
10. `astropy.table._column_mixins`
11. `astropy.table._np_utils`
12. `astropy.time._parse_times`
13. `astropy.timeseries.periodograms.bls._impl`
14. `astropy.timeseries.periodograms.lombscargle.implementations.cython_impl`
15. `astropy.utils.xml._iterparser`
16. `astropy.wcs._wcs`


---

## Description / Timeline

This is a proposed 12-week coding schedule, with community bonding beforehand.

| Period | Description |
|--------|-------------|
| **Community Bonding Period** | Set up development context, discuss project scope with mentor, review the compiled extensions in scope, and agree on testing priorities and expectations. |
| **Weeks 1–3** | Exploration and planning phase. Study all 16 extension modules carefully, document their behavior , interfaces, and likely edge cases, and review findings with my mentor. The output of this phase is a finalized, mentor-reviewed test plan that sequences the remaining work by complexity and dependency. I deliberately keep the per-week coding schedule flexible until this phase is complete, as the 16 extensions vary significantly in complexity, and I would rather let that study inform the schedule than impose an artificial structure before I have seen the code. |
| **Weeks 4–10** | Test writing phase, sequenced according to the plan produced in weeks 1–3. I will work through the extensions in an order agreed upon with my mentor, opening PRs incrementally and incorporating review feedback as I go. |
| **Week 11** | Address accumulated review feedback, revisit any extensions where initial tests were incomplete, and handle edge cases that emerged during review. |
| **Week 12** | Final cleanup, close any open review comments, and prepare the project report. |

---

## Communication

My primary communication channel is Slack, and I would prefer to use Slack for day-to-day discussion and coordination. For code-specific feedback, design questions, and review follow-up, I would use GitHub PR comments so that technical discussion remains attached to the relevant code changes.

I am also available for scheduled synchronous check-ins if needed. My usual response time is within 24 hours, and I will always communicate planned absences in advance.

---

## GSoC

**Have you participated previously in GSoC?** No, this is my first time applying.

**Are you also applying to other projects?** No, this is the only project I am planning to take on.

### Schedule Availability

My university semester final exams are scheduled around the second week of June (exact dates not yet confirmed) and will likely take about one week of concentrated preparation and exam time. I will communicate the exact schedule to my mentor as soon as it is confirmed.

I will also be observing Eid ul-Adha, during which I expect reduced availability for a few days. I plan to communicate this early and front-load work in advance so that project momentum is not affected.

Outside of those periods, I will be fully committed to the programme.

---

## Other Comments

I see this project not only as a chance to contribute tests, but as a chance to help strengthen Astropy's internal reliability in a way that supports future architectural work. I would be excited to work closely with my mentor on understanding these extensions properly and building direct tests that remain useful beyond the summer itself.