# Improving radiospectra's Functionality and Interoperability

## Contributor Information
* **Name:** Kumar Amityush
* **Time-zone:** UTC +05:30 (India)
* **Matrix/slack/IRC Handle:** @kr_amityush:matrix.org (Matrix)
* **Github/forge username:** [Amityush-lgtm](https://github.com/Amityush-lgtm)
* **Blog:** [Modernizing radiospectra](https://sunpy-radiospectra.blogspot.com/)
* **Blog RSS feed:** https://sunpy-radiospectra.blogspot.com/feeds/posts/default?alt=rss
* **PR link(s):**
  * [Migrate network clients to new unified Scraper format (#142)](https://github.com/sunpy/radiospectra/pull/142) (Merged)
  * [Ensure spectrogram plotting correctly handles non-UTC time scales (#144)](https://github.com/sunpy/radiospectra/pull/144) (Merged)
  * [Fix mixed frequency unit plotting on shared axes (#151)](https://github.com/sunpy/radiospectra/pull/151) (Merged)
  * [Restore WAVES Fido client for SPDF data sources (#158)](https://github.com/sunpy/radiospectra/pull/158) (Merged)
  * [Improve default time axis formatting using ConciseDateFormatter (#163)](https://github.com/sunpy/radiospectra/pull/163) (Waiting for review)

### Background
I have had a fascination in astronomy since I was 14, when I started a blog called [Star-gazers](https://stargazing53179.blogspot.com/) where I wrote articles on topics like stellar evolution, neutron stars and meteor showers. That early curiosity and technical eagerness eventually led me to pursue a degree in Computer Science and Data Science, where I am now learning about statistics, scientific computing and software engineering. Over the past year, I have become comfortable working with Python and its scientific ecosystem including NumPy, Matplotlib and related data analysis tools.

Recently, I started contributing to the radiospectra repository, where I worked on several issues and submitted multiple pull requests. These contributions which included restoring the WIND/WAVES Fido client and improving spectrogram plotting behavior which helped me understand the structure of the codebase and how radiospectra integrates with the broader SunPy ecosystem. Through this process I gained practical experience in debugging, testing and maintaining scientific software.

Alongside open source contributions, I have built Python projects involving APIs and data processing, which helped me get better at writing code thats easier to read and work with. I am particularly interested in building reliable tools that are useful for real world scientific workflows and I will continue learning and improving while making meaningful contributions to the community.

### Interest in OpenAstronomy
I have long been interested in astronomy and scientific computing. During my recent contributions to radiospectra, I found the intersection of solar radio physics and modern scientific Python tooling particularly interesting. OpenAstronomy is a place where serious scientific software is built out in the open and contributing here allows me to combine my interests in astronomy, data analysis and software engineering. I have been attending SunPy's weekly meetings regularly which gave me a much better sense of how the community operates and SunPy ecosystem works. Everyone has been very welcoming and I look forward to contributing to the community.

## Project Proposal Application
**Proposal Title:** Modernizing radiospectra: A Coordinate-Aware and Interoperable Spectral Data Framework

**Organisation:** [SunPy](https://github.com/sunpy) (under OpenAstronomy)

**Mentors:** [@samaloney](https://github.com/samaloney), [@hayesla](https://github.com/hayesla)

**Project Size:** 350 hours (Large)

**Difficulty:** High

### Summary
The current `radiospectra` stores spectral data as plain NumPy arrays and every instrument client has its own way of handling metadata, axis ordering and coordinate conversions. There is no built-in way to go from physical coordinates like time and frequency to array indices, no background subtraction tools and it doesn't really fit in with the coordinate-aware models like NDCube and WCS that the rest of SunPy uses. So users end up writing a lot of boilerplate just to do basic things.

This project aims to fix that by building a new `Spectra` class that gives users a better way to slice by time or frequency, do background subtraction and plot spectrograms without having to worry about the underlying array layout. NDCube is a strong option for this since `sunpy.map` and `sunkit-spex` already use it, but part of the project will be to properly explore how best to use it and whether other approaches like xarray or a custom WCS layer might work better for radiospectra's specific needs. The goal is to bring radiospectra closer to the kind of usability that `sunpy.map` already provides for image data.

I am drawn to this project because it involves both software design and real scientific usability and these are the two things I care about. After exploring the codebase and discussing the architecture with mentors in [issue #143](https://github.com/sunpy/radiospectra/issues/143), I have a good understanding of what the current challenges are and what the redesigned API should look like. This is a project where I can make a lasting structural improvement rather than an incremental fix and thats what excites me most.

### Deliverables

#### Design Approach Evaluation

Before building the `Spectra` class I want to breakdown all the possible design approaches during community bonding by building small prototypes and comparing them. The table below shows my initial analysis based on my research:

| Factor | NDCube (subclass) | NDCube (wrapper) | xarray | Plain NumPy + WCS |
|---|---|---|---|---|
| **SunPy Integration** | **Pros:** `sunpy.map` and `sunkit-spex` use it<br>**Cons:** Restricted to NDCube ecosystem | **Pros:** Native to SunPy<br>**Cons:** Wrapper adds extra layer to maintain | **Pros:** Popular in data science<br>**Cons:** Pulls in major external dependency | **Pros:** Minimum dependencies<br>**Cons:** Gains nothing from SunPy ecosystem |
| **WCS Support** | **Pros:** Built-in coordinate handling<br>**Cons:** None | **Pros:** Built-in via wrapper<br>**Cons:** Wrapper can get complex | **Pros:** Good for labeled data<br>**Cons:** Needs manual mapping to Astropy WCS | **Pros:** Full control<br>**Cons:** have to build WCS mapping from scratch |
| **Implementation Effort** | **Pros:** Direct inheritance<br>**Cons:** Must strictly follow NDCube design | **Pros:** Flexible API design<br>**Cons:** Needs more effort to connect components | **Pros:** Quick to start<br>**Cons:** Harder to maintain over time | **Pros:** Simple initially<br>**Cons:** Have to write all coordinate logic manually |
| **API Simplicity** | **Pros:** Familiar to SunPy users<br>**Cons:** Tied to NDCube's structure | **Pros:** Clean, custom API<br>**Cons:** Users must learn specific wrapper | **Pros:** Familiar pandas like syntax<br>**Cons:** Unfamiliar to some | **Pros:** Very simple<br>**Cons:** Very limited functionality |
| **Axis-Aware Slicing** | **Pros:** Native support by coordinate<br>**Cons:** None | **Pros:** Possible via wrapper<br>**Cons:** Slower and more complex code | **Pros:** Native support<br>**Cons:** Uses xarray syntax, not Astropy | **Pros:** Fully custom behavior<br>**Cons:** Must build from scratch |
| **Plotting** | **Pros:** Reuses NDCube plotters<br>**Cons:** Limited to their plot styles | **Pros:** Can use NDCube or custom<br>**Cons:** Needs extra code to connect things | **Pros:** has own plot tools<br>**Cons:** Needs custom code for solar WCS data | **Pros:** Exact plotting required<br>**Cons:** Must build everything from scratch |
| **Maintenance Burden** | **Pros:** Handled by NDCube team<br>**Cons:** If NDCube breaks, then there can be a problem | **Pros:** Handled by NDCube team<br>**Cons:** Must maintain wrapper layer | **Pros:** Handled by xarray team<br>**Cons:** High risk of upstream API changes | **Pros:** No external breakages<br>**Cons:** All maintenance falls on radiospectra team |

The community bonding phase will be used to build prototypes and pick the best one with mentors input before coding starts.

#### 1. New coordinate-aware Spectra object
- A new `Spectra` class with a clear external API providing a WCS-like mapping from physical coordinates like time and frequency to array indices.
- Support for axis-aware operations like slicing, scaling, statistics, rebinning by index or by coordinate.
- Clean integration with SunPy and Astropy built on whichever data model i.e. NDCube, xarray or other works best after evaluation.
- Methods like `freq_at_index()`, `time_at_index()`, `in_interval()` and coordinate slicing.

#### 2. Tools for background subtraction and analysis
- Common background subtraction methods like `auto_const_bg`, `subtract_bg`, `randomized_auto_const_bg` that are used in solar radio analysis.
- An interface so that users can add their own custom methods for background subtraction if needed.
- Other operations like `rescale`, `flatten`, `join_many`, `check_linearity`.

#### 3. Better visualisation tools
- Plotting tools that can  handle the real observational features like data gaps, missing channels, masked values and irregular sampling.
- Proper defaults but also options to customize things like colormaps, axis labels and normalization.

#### 4. Examples and Tests
- As examples are important, I will add example notebooks or scripts demonstrating coordinate slicing and workflows, background subtraction use cases and improved plotting of gappy spectral data.
- Proper test coverage to support all new functionality.

### Description/timeline

Breakdown of my plan across the GSoC period:

#### Note: 
Tests and Documentation i.e. API docstrings, narrative docs and user guides will be written continuously alongside development rather than as in a separate phase to maintain consistency.

#### Community Bonding (May 1 - May 24)

- Get more familiar with the SunPy organisation and go through the radiospectra codebase properly.
- Look at existing radiospectra features and open issues.
- Build small prototypes using different approaches like subclassing NDCube or wrapping it, using xarray or custom WCS layer to understand what works best for radiospectra.
- Compare the prototypes with mentors and agree on a final approach for the Spectra data model.
- Set up the development branch and agree on a PR strategy.

#### Phase 1 : Core Data Structure & Coordinates (Weeks 1 - 6, May 25 - Jul 5)

| Week | Dates | Description |
| ---- | ----- | ----------- |
| Week 1 | May 25 - May 31 | Start building the base `Spectra` class using the approach agreed on during community bonding. Open PRs for the core class structure.|
| Week 2 | Jun 1 - Jun 7 | Add the WCS-like interface that maps physical coordinates like time and frequency to array indices.|
| Week 3 | Jun 8 - Jun 14 | Build the transition layer that converts old instrument metadata like e-Callisto, WAVES etc. into valid WCS formats.|
| Week 4 | Jun 15 - Jun 21 | Write methods for coordinate lookup like `freq_at_index()`, `time_at_index()` and `in_interval()`. Also support slicing by both indices and coordinates.|
| Week 5 | Jun 22 - Jun 28 | Start working on background subtraction. Implement atleast one common method i.e. `auto_const_bg` or something else. Define the interface for user supplied background subtraction functions.|
| Week 6 | Jun 29 - Jul 5 | Set up the core test infrastructure for new data model. Start improving visualisation for irregular or gapped data. Finalise and merge Phase 1 PRs.|

#### Midterm Evaluation (Jul 6 - Jul 10)

#### Phase 2 : Analysis, Visualisation & Gallery (Weeks 7 - 12, Jul 6 - Aug 16)

| Week | Dates | Description |
| ---- | ----- | ----------- |
| Week 7 | Jul 6 - Jul 12 | Implement the remaining operations on `Spectra` like scaling, reductions rebinning. |
| Week 8 | Jul 13 - Jul 19 | Finish background subtraction framework with `subtract_bg`, `randomized_auto_const_bg` and additional built-in methods. Also implement `rescale`, `flatten`, `check_linearity`. |
| Week 9 | Jul 20 - Jul 26 | Build the plotting functions using Matplotlib or NDCube plotters. Handle missing channels and add customized color normalizations. |
| Week 10 | Jul 27 - Aug 2 | Finish up visualisation improvements for data with gaps, masked values and irregular sampling. Add depreceation warnings and migration paths for old `Spectrogram`/`SpectrogramFactory` classes. |
| Week 11 | Aug 3 - Aug 9 | Make example gallery demonstrating coordinate-based slicing, background subtraction workflows and improved plotting of gappy spectral data. |
| Week 12 | Aug 10 - Aug 16 | Improve test coverage, fix remaining issues and complete final refactoring. Merge all completed work upstream. |

#### Final Evaluation (Aug 17 - Aug 24)

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, I haven't participated before.

### Are you also applying to other projects?

No.

### Schedule availability

I am available throughout the GSoC period as my vacations will start from May and committed fully to the 350 hour project. I have no planned holidays or travel during the programme timeline.


## Other comments
- I will post weekly progress updates on my [blog](https://sunpy-radiospectra.blogspot.com/) throughout the GSoC coding period.
- I am comfortable communicating on Matrix and GitHub and can adjust my working hours to overlap with mentor availability if needed.
