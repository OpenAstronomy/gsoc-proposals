# Improving radiospectra's Functionality and Interoperability

## Contributor Information
* **Name:** Kumar Amityush
* **Time-zone:** UTC +05:30 (India)
* **Matrix/slack/IRC Handle:** @kr_amityush:matrix.org (Matrix)
* **Github/forge username:** [Amityush-lgtm](https://github.com/Amityush-lgtm)
* **Blog:** [Modernizing radiospectra](https://sunpy-radiospectra.blogspot.com/)
* **Blog RSS feed:** https://sunpy-radiospectra.blogspot.com/feeds/posts/default?alt=rss
* **PR link(s):**
  * [radiospectra#142](https://github.com/sunpy/radiospectra/pull/142)
  * [radiospectra#144](https://github.com/sunpy/radiospectra/pull/144)
  * [radiospectra#151](https://github.com/sunpy/radiospectra/pull/151)
  * [radiospectra#158](https://github.com/sunpy/radiospectra/pull/158)
  * [radiospectra#163](https://github.com/sunpy/radiospectra/pull/163)

### Background
I have been fascinated by astronomy since I was 14, when I started a blog called [Star-gazers](https://stargazing53179.blogspot.com/) where I wrote articles on topics like stellar evolution, neutron stars and meteor showers. That early curiosity eventually led me to pursue a degree in Computer Science and Data Science, where I am now building a strong foundation in statistics, scientific computing and software engineering. Over the past year, I have become comfortable working with Python and its scientific ecosystem including NumPy, Matplotlib and related data analysis tools.

Recently, I started contributing to the radiospectra repository, where I worked on several issues and submitted multiple pull requests. These contributions which included restoring the WIND/WAVES Fido client and improving spectrogram plotting behavior which helped me understand the structure of the codebase and how radiospectra integrates with the broader SunPy ecosystem. Through this process, I gained practical experience in debugging, testing and maintaining scientific software.

Alongside open source contributions, I have built small Python projects involving API usage and data processing, which strengthened my understanding of writing clean and maintainable code. I am particularly interested in building reliable tools that are useful for real world scientific workflows and am motivated to continue learning while contributing meaningfully to the community.

### Interest in OpenAstronomy
I have long been interested in astronomy and scientific computing. During my recent contributions to radiospectra, I found the intersection of solar radio physics and modern scientific Python tooling particularly interesting. OpenAstronomy provides a collaborative environment where high quality scientific software is developed in the open source and contributing here allows me to combine my interests in astronomy, data analysis and software engineering. I would greatly appreciate the opportunity to contribute through GSoC and help strengthen radiospectra within the broader SunPy ecosystem.

## Project Proposal Application
**Proposal Title:** Modernizing radiospectra: A Coordinate-Aware and Interoperable Spectral Data Framework

**Organisation:** [SunPy](https://github.com/sunpy) (under OpenAstronomy)

**Mentors:** [@samaloney](https://github.com/samaloney), [@hayesla](https://github.com/hayesla)

**Project Size:** 350 hours (Large)

**Difficulty:** High

### Summary
The current `radiospectra` package stores spectral data as plain NumPy arrays with instrument specific metadata conventions. This means there is no unified way to map between physical coordinates like time, frequency and array indices, no built-in support for common analysis operations like background subtraction and no alignment with the coordinate-aware data models like NDCube and WCS that the rest of the SunPy ecosystem has adopted. As a result, users must write boilerplate code to perform even basic tasks and each instrument client re-implements its own conventions for axis ordering and metadata handling.

This project aims to replace that foundation with a new `Spectra` class built on top of NDCube and Astropy's WCS framework. The redesigned object will provide a single, coordinate-aware API where users can slice by time range or frequency band, perform background subtraction and plot spectrograms without needing to know the underlying array layout. The goal is to bring radiospectra to the same level of interoperability and usability that `sunpy.map` provides for solar image data.

I am drawn to this project because it sits at the intersection of software design and scientific usability and these are the two things I care about. After exploring the codebase and discussing the architecture with maintainers in [issue #143](https://github.com/sunpy/radiospectra/issues/143), I have a good understanding of what the current challenges are and what the redesigned API should look like. This is a project where I can make a lasting structural improvement rather than an incremental fix and thats what excites me most.

### Deliverables

**1. Redesigned, coordinate-aware Spectra object**
- A new `Spectra` class with a clear external API providing a WCS-like mapping from physical coordinates like time and frequency to array indices.
- Support for axis-aware operations: slicing, scaling, statistics, rebinning by index or by coordinate.
- Clean integration with SunPy, Astropy, NDCube and xarray data structures.
- Methods like `freq_at_index()`, `time_at_index()`, coordinate-based slicing and `in_interval()`.

**2. Background subtraction framework and analysis tools**
- Built-in background subtraction methods commonly used in solar radio analysis like `auto_const_bg`, `subtract_bg`, `randomized_auto_const_bg`.
- A clear, extensible interface that allows users to supply custom background subtraction functions.
- Additional operations: `rescale`, `flatten`, `join_many`, `check_linearity`.

**3. Improved visualisation for real-world data**
- Plotting tools that handle data gaps, missing channels, masked values and irregular time and frequency sampling.
- Sensible defaults with options for customizing visual output (colormaps, axis labels, normalization).

**4. Example gallery and documentation**
- Provide example notebooks and workflows.
- Update API documentation.
- Ensure comprehensive test coverage.

**Testing and Backward Compatibility**

All API modifications will preserve backward compatibility wherever necessary. If breaking changes are necessary they will follow a proper deprecation cycle with warnings and documentation updates. Each major feature will include unit tests, integration tests and regression tests to ensure stability and interoperability.

### Description/timeline

Breakdown of the work across the GSoC period:

#### Community Bonding

- Finalize the `Spectra` API design with mentors like NDCube wrapping strategy and coordinate behavior for out of bounds values.
- Study NDCube and WCS internals to understand how `sunpy.map` implements its WCS integration.
- Set up the development branch, CI and agree on the PR strategy (one PR per phase vs. per feature).
- Continue reviewing open issues and small fixes to stay active in the codebase.

#### Phase 1 : Core Data Structure & Coordinates (Weeks 1 – 4)

| Week | Description |
| ---- | ----------- |
| Week 1 | Establish the base `Spectra` class with NDCube integration. Open PRs for core class base. |
| Week 2 | Implement the transition layer converting old instrument metadata (e-Callisto, WAVES, etc.) into valid WCS formats. |
| Week 3 | Develop methods for coordinate lookup like `freq_at_index()`, `time_at_index()` and `in_interval()`. |
| Week 4 | Write unit and regression tests for core features. Finalize and merge Phase 1 PRs. |

#### Phase 2 : Analysis Framework & Background Subtraction (Weeks 5 – 8)

| Week | Description |
| ---- | ----------- |
| Week 5 | Port the base logic for `auto_const_bg` and `subtract_bg`. Ensure compatibility with masked arrays for handling data gaps. |
| Week 6 | Implement `randomized_auto_const_bg` and add an extensible `apply()` method allowing users to pass custom Python callables for background math. |
| Week 7 | Implement array operations: `rescale`, `flatten`, and `check_linearity`. |
| Week 8 | Complete test suite for analysis tools. Compare outputs against the old implementation to ensure mathematical accuracy. |

#### Phase 3 : Visualisation & Documentation (Weeks 9 – 12)

| Week | Description |
| ---- | ----------- |
| Week 9 | Build plotting functions using Matplotlib/NDCube plotters. Implement handling for missing channels and customized color normalizations. |
| Week 10 | Create deprecation warnings and migration paths for the old `Spectrogram` and `SpectrogramFactory` classes to ensure backward compatibility. |
| Week 11 | Write Sphinx API documentation and create 2–3 detailed Jupyter Notebooks for the SunPy Gallery like "Advanced analysis of e-callisto data" and "background subtraction and analysis of radio spectra". |
| Week 12 | Final integration testing, performance benchmarking and stabilization. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, I haven't participated before.

### Are you also applying to other projects?

No.

### Schedule availability

I am available throughout the GSoC period and committed wholeheartedly to the 350 hour project. I have no planned holidays or travel during the programme timeline.


## Other comments
- I will post weekly progress updates on my [blog](https://sunpy-radiospectra.blogspot.com/) throughout the GSoC coding period.
- I am comfortable communicating on Matrix and GitHub and can adjust my working hours to overlap with mentor availability if needed.
