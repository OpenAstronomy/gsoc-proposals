# JuliaAstro: Spectra.jl across the electromagnetic spectrum

## Basic Information

**Name:** Mahmoud Mohamed Fathy  
**From:** Alexandria, Egypt  
**University:** Al Alamein International University (AIU), Faculty of Computer Science – AI Science (2023 - Present)  
**Time Zone:** GMT+2  
**Email:** mahmoudmoh23101475@gmail.com  
**Email:** Mahmoud.fathy.2024@aiu.edu.eg  
**Zulip:** https://julialang.zulipchat.com/#user/1060802  
**GitHub:** https://github.com/Mahmoudmoh-cse  
**LinkedIn:** https://linkedin.com/in/mahmoud-mohamed-4b0a07178  
**Matrix:** @mahmoud_mohamed_f:matrix.org  
**Phone:** +20 106 292 7729  
**Blog:** https://dev.to/mahmoud_mohamed_8561cb6b0/my-gsoc-2026-journey-spectrajl-across-the-electromagnetic-spectrum-2h40  

**Overview:** My name is Mahmoud Mohamed Fathy, I am a third-year computer science student majoring in AI Science at Al Alamein International University. I always try to explore the depths of technology and its applications. I enjoy applying what I learn on real projects and love making projects that help people. I have a strong background in data engineering, machine learning, and computer vision.

---

## Me and GSoC

- I plan to dedicate approximately 12 hours per week before the start of GSoC to familiarize myself with the codebase and documentation of Spectra.jl and SpectralFitting.jl.
- During the coding phase of GSoC, I am prepared to invest a minimum of 24 to 30 hours per week into the project. Furthermore, I am open to continuing my involvement with the JuliaAstro project beyond the GSoC period. If there is additional work to be done.

---

## Why Me?

- Over the past weeks, I have spent a great deal of time inspecting the codebases of Spectra.jl and SpectralFitting.jl and trying to understand the design.
- I took a deep dive into the Spectra.jl documentation and the OGIP data format specification, and became familiar with the JuliaAstro ecosystem and many of its conventions.
- I joined the JuliaAstro community channels and became familiar with the project's community and mentors. I have been asking questions about the project and the codebase, commenting on issue #47 (GSoC start here) and issue #41 (the main design discussion).

---

## My Experience

**Contributions to JuliaAstro / Open Source:**
- Commented on Spectra.jl issue #47 (GSoC start here) – introduced myself and engaged with the mentors about the project direction.
- Commented on Spectra.jl issue #41 – asked a technical question about the OGIP migration interface design, demonstrating understanding of the three-file OGIP structure (PHA, ARF, RMF).
- PR #52: docs: improve citation section with BibTeX entry — https://github.com/JuliaAstro/Spectra.jl/pull/52

**Personal Projects:**
- **Image Enhancement Web Application (2025):** A web application for image enhancement using computer vision and signal processing techniques, directly relevant to spectral data manipulation pipelines.
- **Pneumonia Detection – CNN (2025):** Built a complete scientific data pipeline for medical image classification, demonstrating experience with structured file I/O and numerical data workflows.
- **Customer Churn Prediction / Health Condition Prediction / Kidney Disease Prediction (2025):** End-to-end ML projects involving data ingestion, transformation, and modeling — closely mirroring the data loading and rebinning workflows central to Spectra.jl.
- **Treasure Hunt Game – Python A\* Search (2024):** Algorithmic problem solving using graph search, demonstrating strong fundamentals in algorithm design.
- **Student Performance Dashboard with Ticketing System (2025):** Full-stack data engineering graduation project at DEPI, involving structured data pipelines and dashboarding.

**Internships & Industry Experience:**
- Data Engineering Intern – DEPI (Digital Egypt Pioneers Initiative, June–Dec 2025): Built data pipelines and worked with structured databases, directly applicable to file format parsing in this project.
- Mastering OOP using Java – ITI (July–Oct 2025): Strengthened software design skills and built a Cafeteria Management System.
- Computer Vision Team Member – Vortex Robotics (2025–Present): Applied AI-based vision for underwater ROV in a research-driven, team-based environment.

I am also a competitive programmer and ICPC participant (Summer 2024), active in algorithmic problem solving.

---

## Spectra.jl across the Electromagnetic Spectrum

### Introduction

**Problem Definition:** When we observe a star or any astrophysical source, we can disperse its light through a prism or off a grating to obtain the intensity of radiation across a range of wavelengths — a spectrum. Spectra are measured across many different frequency ranges (radio, infrared, optical, ultraviolet, X-ray, gamma-ray) and come in many different formats. Each frequency range has its own instrument types and exploits different physics, meaning data files from various observatories must be handled differently and carefully in order to gain meaningful scientific insights.

Spectra.jl currently lacks a unified, well-tested interface for loading and manipulating these diverse datasets. The OGIP parser — the standard format for X-ray spectra — lives in SpectralFitting.jl rather than Spectra.jl, common formats for optical, infrared, and radio data are not yet supported, and basic manipulation routines such as rebinning, unit conversion, and wavelength/energy shifting are missing.

**Solution:** The solution for this problem could be done with the following:
- Migrate and integrate the OGIP parser from SpectralFitting.jl into Spectra.jl, ensuring compatibility with the spectral types Spectra.jl defines.
- Implement data loaders for at least three different spectral file formats beyond OGIP.
- Provide common utility routines such as rebinning, wavelength/energy shifting, and unit conversion — all guided by Test Driven Development.

---

## Project Deliverables

This project will be built upon the groundwork laid by existing work in SpectralFitting.jl and Spectra.jl, providing a solid foundation. Leveraging this existing work will streamline the project's scope.

And because a lot of groundwork has already been done, this project will focus on the following tasks:

- Migrate and integrate the OGIP parser from SpectralFitting.jl into Spectra.jl with all existing tests passing.
- Implement data loaders for at least three spectral file formats (SDSS optical spectra, JWST/Spitzer infrared FITS, and a radio FITS format).
- Implement common spectral manipulation routines: rebinning (minimum flux, S/N, count rate), wavelength/energy shift correction, and unit conversion.
- Add documentation strings for each function.
- Unit tests and Continuous Integration (CI).

In addition to the above tasks, other crucial aspects must be integrated into the implementation process:
- Clean, reliable, and maintainable code.
- Blog posts containing a log of everything that has been done and what's next.

---

## Implementation

### Milestone 1 – OGIP Migration

The first task is to migrate the OGIP parser from SpectralFitting.jl into Spectra.jl. OGIP (Office of Guest Investigator Programs) is the standard format for X-ray spectra, consisting of three linked FITS files: the PHA file (the spectrum itself), the ARF file (ancillary response), and the RMF file (redistribution matrix). Because all code and tests already exist in SpectralFitting.jl, this migration is a focused and informative starting task — it is relatively easy and will teach me the internals of Spectra.jl in the process.

- Copy and adapt the OGIP parser and its test suite from SpectralFitting.jl into Spectra.jl.
- Ensure all existing tests pass under the new package.
- Identify design inconsistencies with the Spectra.jl interface and refactor accordingly with mentor guidance.

### Milestone 2 – New Format Loaders

The second milestone implements loaders for three additional spectral file formats, chosen in order of increasing complexity:

- **SDSS Optical Spectra:** Well-documented FITS files with standardized column names. This serves as a gentle but real entry point for writing new loaders and will act as a template for future contributors.
- **JWST/Spitzer Infrared FITS:** FITS binary tables with instrument-specific extensions. My experience with image processing pipelines and data engineering makes this a natural intermediate step.
- **Radio FITS Spectral Cubes:** Radio spectra are often embedded in spatial data cubes, requiring careful handling of axes and metadata. This is the most challenging loader and will round out coverage across the full electromagnetic spectrum.

### Milestone 3 – Manipulation Routines

The third milestone delivers the following common spectral manipulation utilities:

- **Rebinning:** group spectral bins by minimum flux, signal-to-noise ratio, or count rate thresholds.
- **Wavelength/energy shifting:** apply Doppler corrections and cosmological redshift adjustments.
- **Unit conversion:** convert between common spectral units (eV, keV, Angstrom, nm, Hz, etc.) using Unitful.jl.

---

## Testing

Writing tests is a crucial part of the software development lifecycle. Most software products should have some kind of automated testing process whose mission is to ensure the delivery of correct functionality. Spectra.jl is no exception. Each function and loader implemented in this project will be accompanied by test cases to assert that the function is properly working. For file format loaders, tests will be written against real example spectra from the curated library in the repository. For manipulation routines, both unit tests and regression tests against known reference outputs will be written. All tests will run automatically via CI on every pull request.

---

## Documentation Strings

Like testing, documentation is an important part of any software product. The plan for documentation includes a documentation string inside every function and type so that future developers will have an easier time maintaining the existing codebase. As for users, if the situation demands it, there will be step-by-step guides explaining the usage of the loaders and manipulation routines. I will also maintain a blog with regular updates throughout the coding period.

---

## Timeline

### Community Bonding Period (May 1 - 26)

**Week 1-2 (May 1 - 15):**
- Getting to know mentors, deep dive into documentation, getting up to speed to begin working on the project.
- Socialize and interact with other community members (students and mentors).
- Learn more about OpenAstronomy as an organization with a mission and vision.
- Familiarize myself more with the architecture of Spectra.jl and SpectralFitting.jl, focusing on the OGIP parser.
- Clarify ambiguous portions in the proposal.
- Start a blog.

**Week 3 (May 16 - 23):**
- Begin migrating the OGIP parser from SpectralFitting.jl into Spectra.jl.
- Write skeleton test files for the OGIP migration.
- Study the FITS and OGIP standards in depth. Download and manually inspect example PHA, ARF, and RMF files.

**Week 4 (May 24 - 31):**
- Complete the OGIP migration and ensure all existing tests pass.
- Create an initial pull request and seek initial reviews.
- Begin planning the interface design for new loaders with mentor feedback.

### Coding Phase 1 (June 1 - July 14)

**Week 1-2 (June 1 - June 14):**
- Implement the SDSS optical spectra loader using TDD — write tests first, then the loader.
- Add docstrings and update the blog.
- Open PR for SDSS loader and seek review.

**Week 3-4 (June 15 - June 28):**
- Begin the JWST/Spitzer infrared FITS loader. Study instrument-specific FITS extensions.
- Write tests against real example spectra from the curated library.
- Open PR and seek review.

**Week 5-6 (June 29 - July 12):**
- Complete the infrared loader, finalize tests and documentation.
- Begin the radio FITS spectral cube loader.
- Write initial tests for the radio loader.

**Week 7 (July 8 - July 14):**
- Finalize work for midterm evaluation. Ensure all PRs are open and seeking reviews.

**Midterm Evaluation (July 14)**

### Coding Phase 2 (July 15 - September 1)

**Week 8-9 (July 15 - July 28):**
- Complete the radio FITS spectral cube loader and its tests.
- Address all outstanding feedback from the midterm evaluation.

**Week 10-11 (July 29 - August 11):**
- Implement rebinning routines (minimum flux, S/N, count rate heuristics).
- Implement wavelength and energy shift correction functions.
- Write tests and open PRs for all manipulation routines.

**Week 12 (August 12 - August 18):**
- Implement unit conversion utilities (eV, keV, Angstrom, nm, Hz) using Unitful.jl.
- Write tests and full documentation for all unit conversion functions.

**Week 13 (August 19 - September 1):**
- Buffer week: address all outstanding review comments, fix edge cases, polish documentation.
- Ensure CI passes on all open PRs.
- Write a final blog post summarizing all deliverables and suggesting future work.

**Final Evaluation**
