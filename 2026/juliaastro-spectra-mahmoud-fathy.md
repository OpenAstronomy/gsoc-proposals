# JuliaAstro: Spectra.jl across the electromagnetic spectrum

**Name:** Mahmoud Mohamed Fathy
**University:** Al Alamein International University (AIU), Faculty of Computer Science – AI Science (2023 - Present)
**Time Zone:** GMT+2
**Email:** mahmoudmoh23101475@gmail.com
**Blog:** https://dev.to/mahmoud_mohamed_8561cb6b0/my-gsoc-2026-journey-spectrajl-across-the-electromagnetic-spectrum-2h40
**Contribution:** https://github.com/JuliaAstro/Spectra.jl/pull/52

## Abstract
Spectra.jl is a Julia package for loading and manipulating astrophysical spectra across the electromagnetic spectrum. This project will migrate the OGIP parser from SpectralFitting.jl into Spectra.jl, implement loaders for at least three spectral formats (SDSS optical, JWST/Spitzer infrared, radio FITS), and add common manipulation routines including rebinning, wavelength/energy shifting, and unit conversion using Unitful.jl.

## About Me
I am a third-year Computer Science student majoring in AI Science at Al Alamein International University, Egypt. I have a strong background in data engineering, machine learning, and computer vision. I am an ICPC participant and have hands-on experience building scientific data pipelines.

## Me and GSoC
- I plan to dedicate approximately 12 hours per week before GSoC starts to familiarize myself with the Spectra.jl and SpectralFitting.jl codebases.
- During the coding phase I am prepared to invest a minimum of 24 to 30 hours per week.
- I have joined the JuliaAstro community, commented on issue #47 (GSoC start here) and issue #41 (main design discussion).

## Why Me?
- I have inspected the codebases of Spectra.jl and SpectralFitting.jl and understand what the OGIP migration will involve.
- I have read the OGIP data format specification and understand the three-file structure (PHA, ARF, RMF).
- I have contributed a real patch to Spectra.jl (PR #52).
- My background in data engineering and competitive programming gives me the skills to build reliable, well-tested scientific software.

## My Experience

**Contributions to JuliaAstro:**
- Commented on Spectra.jl issue #47 (GSoC start here)
- Commented on Spectra.jl issue #41 (design discussion)
- PR #52: docs: improve citation section with BibTeX entry

**Personal Projects:**
- Image Enhancement Web Application (2025)
- Pneumonia Detection – CNN (2025)
- Customer Churn / Health / Kidney Disease Prediction (2025)
- Treasure Hunt Game – Python A* Search (2024)
- Student Performance Dashboard with Ticketing System (2025)

**Internships:**
- Data Engineering Intern – DEPI (June–Dec 2025)
- Mastering OOP using Java – ITI (July–Oct 2025)
- Computer Vision Team Member – Vortex Robotics (2025–Present)

## Problem Definition
Spectra.jl currently lacks a unified interface for loading and manipulating astrophysical spectra across different wavelength regimes. The OGIP parser lives in SpectralFitting.jl rather than Spectra.jl, common formats for optical, infrared, and radio data are not supported, and basic manipulation routines such as rebinning, unit conversion, and wavelength/energy shifting are missing.

## Solution
- Migrate the OGIP parser from SpectralFitting.jl into Spectra.jl.
- Implement loaders for SDSS optical spectra, JWST/Spitzer infrared FITS, and radio FITS spectral cubes.
- Implement rebinning, wavelength/energy shifting, and unit conversion routines.
- Full test coverage and documentation for all new code.

## Project Deliverables
- Migrated and integrated OGIP parser with all tests passing.
- Data loaders for at least three spectral file formats.
- Common spectral manipulation routines.
- Documentation strings for every function.
- Unit tests and CI integration.
- Blog posts documenting progress.

## Implementation

### Milestone 1 – OGIP Migration
Copy and adapt the OGIP parser and test suite from SpectralFitting.jl into Spectra.jl. Ensure all existing tests pass. Refactor to match Spectra.jl interface conventions.

### Milestone 2 – New Format Loaders
- **SDSS Optical Spectra:** Well-documented FITS files, gentle entry point.
- **JWST/Spitzer Infrared FITS:** FITS binary tables with instrument-specific extensions.
- **Radio FITS Spectral Cubes:** Spectra embedded in spatial data cubes.

### Milestone 3 – Manipulation Routines
- Rebinning by minimum flux, S/N, or count rate.
- Wavelength/energy shift corrections.
- Unit conversion using Unitful.jl.

## Testing
Every function will be accompanied by test cases written using Test Driven Development. Tests will run automatically via CI on every pull request.

## Documentation
Every function and type will have a Julia docstring. A blog will be maintained with regular updates throughout the coding period.

## Timeline

### Community Bonding Period (May 1 - 26)
**Week 1-2 (May 1 - 15):**
- Get to know mentors and community.
- Deep dive into Spectra.jl and SpectralFitting.jl architecture.
- Study FITS and OGIP standards.
- Start blog.

**Week 3-4 (May 16 - 26):**
- Begin OGIP migration. Write skeleton tests.
- Create draft PR and seek early feedback.

### Coding Phase 1 (June 1 - July 14)
**Week 1-2 (June 1 - 14):** SDSS optical loader + tests + docs + PR.

**Week 3-4 (June 15 - 28):** JWST/Spitzer infrared loader + tests + PR.

**Week 5-6 (June 29 - July 12):** Complete infrared loader. Begin radio loader.

**Week 7 (July 8 - 14):** Finalize for midterm. All PRs open for review.

**Midterm Evaluation (July 14)**

### Coding Phase 2 (July 15 - September 1)
**Week 8-9 (July 15 - 28):** Complete radio loader. Address midterm feedback.

**Week 10-11 (July 29 - Aug 11):** Rebinning + wavelength shift routines + tests + PRs.

**Week 12 (Aug 12 - 18):** Unit conversion utilities + tests + docs.

**Week 13 (Aug 19 - Sep 1):** Buffer week. Polish everything. Final blog post.

**Final Evaluation**
```
