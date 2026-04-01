# Improving radiospectra's Functionality and Interoperability

## Contributor Information

- **Name:** Mohamed Ali BENBELGHITH
- **Time-zone:** UTC+1 (Tunisia)
- **Matrix/Slack/IRC Handle:** @medali\_\_:matrix.org
- **GitHub Username:** [MohamedAli1937](https://github.com/MohamedAli1937)
- **Blog:** [https://mohamedali1937.github.io/post01/](https://mohamedali1937.github.io/post01/)
- **Blog RSS feed:** N/A
- **PR Links:**
  - [#167: Adds slice method and associated tests (Open - In Review)](https://github.com/sunpy/radiospectra/pull/167)
  - [#181: Fixes time gap rendering in pcolormesh plots (Open - In Review)](https://github.com/sunpy/radiospectra/pull/181)
  - [#187: Fix metadata key mismatch in factory pipeline (Open - In Review)](https://github.com/sunpy/radiospectra/pull/187)
  - [#188: Adds peek() function for GenericSpectrogram (Open - In Review)](https://github.com/sunpy/radiospectra/pull/188)
  - [#189: Add equality and copy methods (Open - In Review)](https://github.com/sunpy/radiospectra/pull/189)

### Background

I am a 2nd-year undergraduate Software Engineering student at INSAT, Tunisia, with a deep passion for mathematics, physics, and intelligence. Combining these interests with software engineering, I work with OpenAstronomy to bridge the gap between legacy radio astronomy data formats and modern Python ecosystems. My focus is on developing robust, test-driven, and mathematically sound architectures.

### Interest in OpenAstronomy

I want to contribute to OpenAstronomy because I strongly believe in the power of unified, interoperable data ecosystems. While working on `radiospectra`, I noticed how fragmented certain array structures were compared to core SunPy standards. My goal for this GSoC project is to make handling `radiospectra` data identical to handling core SunPy objects—seamless, intuitive, and scientifically accurate. I am highly active in the community, checking Matrix channels daily, stress-testing the codebase with real instrument data, and employing test-driven development (TDD) to address deep architectural issues.

---

## Project Proposal

**Proposal Title:** Improving radiospectra's Functionality and Interoperability

**Organisation:** OpenAstronomy (SunPy)

### Summary

Radiospectra’s current design forces manual array indexing and error-prone coordinate handling, resulting in visualization artifacts and limiting scientific efficiency. This project modernizes Radiospectra into a highly interoperable, coordinate-aware framework.

By inheriting from `NDCube` and integrating Astropy’s World Coordinate System (WCS) mappings, this project unlocks:

- Physical unit slicing
- Seamless extra coordinate caching via `ExtraCoords`
- Deterministic visualization rendering

The new `Spectra` class naturally supports Python slicing (e.g., `spec[1:5, :-4]`), equality checks, and deep copying, achieving API parity with the SunPy ecosystem. Previous PRs (#167, #181, #187) have already laid a foundation for this work.

### Key Deliverables

1. **Coordinate-Aware Spectra:** A modernized `Spectra` class inheriting from `NDCube` with full WCS mapping, `ExtraCoords` integration, and axis-aware physical slicing.

2. **Background Subtraction Framework:** High-performance C-level vectorized rolling minimums (`scipy.ndimage.minimum_filter1d`) and polynomial fitting for instrument thermal drift removal.

3. **Gap-Aware Visualization:** Deterministic gap detection algorithm ($\Delta t > C \cdot (1+\epsilon)$) with arrays padded using `numpy.ma.masked_invalid()` to prevent misleading visual stretching, integrated with `@peek_show`.

4. **Documentation & Tutorial Gallery:** Narrative-style docs (`spectrogram.rst`) following `sunpy.map` conventions, legacy support with deprecation warnings, and beginner-to-advanced Jupyter notebook tutorials.

> _Note:_ Data extraction scrapers (e.g., eCALLISTO) will remain in `radiospectra` to avoid scope creep and external dependencies, as per mentor guidance.

---

### Implementation Timeline (350 Hours)

| Period            | Task & Deliverables                                                                                                       |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------ |
| Community Bonding | Map WCS requirements, establish PR strategies, and define API design constraints with mentors.                            |
| Week 1            | **Core Architecture:** Create `Spectra` class, constructor, and initial `pytest` suites.                                  |
| Week 2            | Integrate `ExtraCoords` for frequency sweeps; implement `freq_at_index()` and `time_at_index()` using Astropy `Quantity`. |
| Week 3            | Update PSP RFS and SOLO RPW readers to produce NDCube objects; validate with `pytest.mark.remote_data`.                   |
| Week 4            | Implement axis-aware physical slicing; achieve `sunpy.map.Map.submap()` parity.                                           |
| Week 5            | Build Background Subtraction API; implement vectorized `rolling_min` and polynomial fits.                                 |
| Week 6            | Finalize custom `_custom_bg(data, meta)` interface; complete NumpyDoc math documentation.                                 |
| Week 7            | Implement physical rebinning, `.flatten()`, and intensity normalization.                                                  |
| Week 8            | Develop deterministic gap detection ($\Delta t > C \cdot (1+\epsilon)$) as standalone function.                           |
| Week 9            | Overhaul Plotting Mixin with `masked_invalid()` edge handling; apply native `@peek_show` parity.                          |
| Week 10           | Apply `AstropyDeprecationWarning` to legacy `GenericSpectrogram`; draft migration guides.                                 |
| Week 11           | Build rich Jupyter notebook gallery with 3+ complex workflows.                                                            |
| Week 12           | Resolve pending reviews, ensure 100% test coverage, finalize documentation, and submit evaluations.                       |

> _Note:_ Every single week in the timeline includes writing and passing the corresponding `pytest` suites before merging. Special attention is given to:
>
> - Coordinate & Slicing Parity (Weeks 4–7)
> - Real Data Readers (Week 6)
> - Visualization Integrity (Weeks 11–12)

---

## GSoC Participation

- **Previous Participation:** None, this is my first GSoC.
- **Applying to Other Projects:** No, fully dedicated to this OpenAstronomy project.
- **Availability:** 350-hour project (~25–30 hours/week). University semester concludes before coding period, ensuring uninterrupted full-time focus. Eid al-Adha holiday (May 27--28) is the only planned exception.

---

## Additional Notes

### Test-Driven Development (TDD)

- Every weekly deliverable includes comprehensive `pytest` suites.
- High-risk areas will use `pytest.mark.remote_data` to validate real CDF files (catching issues like PR #187).
- Figure regression tests will ensure accurate visualization parity with SunPy standards.

### Communication Strategy

- **Daily:** Monitor Matrix/Element channels.
- **Weekly:** Live video check-ins with mentors.
- **Continuous:** GitHub PR review and issue tracking.
- **Timezone:** UTC+1 (flexible for mentors).

### Community Engagement

I am active and check the Element (Matrix) communications channel daily to stay closely synced with the core team. During the recent holy month of Ramadan, attending the 18:00 UTC weekly meetings was impossible as they coincided with Iftar. I successfully compensated by maintaining rigorous asynchronous communication with mentors via Matrix and GitHub PR reviews. I am now eager to transition to attending live meetings, as standard schedules have resumed.
