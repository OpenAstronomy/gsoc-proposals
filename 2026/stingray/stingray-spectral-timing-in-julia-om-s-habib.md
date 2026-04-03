# GSoC 2026 Proposal

# Spectral Timing in Julia

## Contributor Information
- **Name:** Om S Habib (Omiii-215)
- **Time-zone:** IST (UTC+05:30)
- **Matrix/slack:** https://stingraysoftware.slack.com/team/U0A9D0CE12M (Slack)
- **Github:** Omiii-215
- **linkedin:** https://www.linkedin.com/in/om-s-habib-609a81381/
- **Blog:** https://medium.com/@omhabib311/spectral-timing-in-julia-extending-stingray-jl-for-x-ray-astronomy-66f4b40f5531?postPublishedType=initial


* **PR link(s):**

  * **Contributions to stingray.jl:**
    * [Mission Support - follow up (PR #75)](https://github.com/StingraySoftware/Stingray.jl/pull/75) - Open
    * [feat: Implementing frequency rebinning for PowerSpectrum and LightCurve (PR #71)](https://github.com/StingraySoftware/Stingray.jl/pull/71) - Open

  * **Contributions to stingray software:**
    * [convergence issue in power_upper_limit and improve robustness (PR #954)](https://github.com/StingraySoftware/stingray/pull/954) - Merged
    * [Fix notebook compatibility issues (PR #964)](https://github.com/StingraySoftware/stingray/pull/964) - Open
    * [Fix set_xdata scalar bug in InteractivePhaseogram (PR #126)](https://github.com/StingraySoftware/notebooks/pull/126) - Open

  
### Background

I am a computer science undergraduate student with a strong interest in astrophysics and scientific computing. I love working with Python and using it for data science, scientific computing, and software development. 
I have experience with NumPy, Pandas, and scientific Python tools. 
I am also comfortable with SQL for data querying and manipulation and have experience solving algorithms. 
I have been exploring topics such as spectral timing analysis, wavelet-based signal processing, Biosignature detection in exoplanet atmospheres. 
I worked with Unity, where I rendered pipelines such as HDRP (High-Definition Render Pipeline) and URP (Universal Render Pipeline) to build visually rich and optimized environments. Through these projects I explored real-time rendering, lighting systems, and performance optimization, along with integrating gameplay mechanics and 3D scene design. 


I have been contributing to the **Stingray.jl** repository, where I have submitted pull requests for:

- **Mission-specific FITS support** (PR #75): This is a follow up from Kashish srivastava's PR Implementing telescope-specific event file readers for NICER, NuSTAR, XMM-Newton, and RXTE

- **Frequency rebinning** (PR #71): Implementing logarithmic and linear frequency rebinning for `PowerSpectrum` and `LightCurve` objects, enabling proper broadband noise studies

These contributions help me understand the codebase architecture
(EventList -> LightCurve -> Fourier pipeline)

### Interest in OpenAstronomy
Open source astrophysics tools like Astropy, SunPy, and Stingray have made advanced astronomical data analysis available to researchers worldwide.

1. I'm interested in Stingray.jl because X-ray timing studies, like QPOs in black holes and pulsars, help us understand physics in extreme gravitational environments.

2. Julia's performance and scientific computing capabilities make it ideal for processing massive datasets from missions like XRISM and the future Athena observatory.

3. Contributing to this project would help build on the existing work and move closer to completing a full spectral-timing analysis pipeline for the community.


### 1.0 Project Overview

#### 1.1 Problem Statement

The Stingray Python package ([Huppenkothen et al. 2019](https://ui.adsabs.harvard.edu/abs/2019ApJ...881...39H)) provides a comprehensive suite of spectral timing tools used by hundreds of X-ray astronomers worldwide. However, for computationally intensive analyses -- such as computing averaged cross spectra over thousands of segments from multi-megabyte event files -- Python's overhead becomes a bottleneck.

Stingray.jl currently implements:

- EventList I/O from FITS files (with GTI reading, energy column detection)
- LightCurve creation with binning, rebinning, error calculation
- Good Time Interval (GTI) operations (masking, splitting, merging, BTI filling)
- Basic Fourier operations: averaged PDS and cross spectra from events
- Normalization: Leahy, fractional rms, absolute rms

What's missing:

| Gap | Impact |
|-----|--------|
| High-level `Powerspectrum` / `Crossspectrum` types | Users currently work with raw DataFrames -- no structured API |
| Time lags and coherence from cross spectra | Fundamental for understanding reverberation and jet emission |
| Covariance spectra | Key tool for energy-dependent variability studies |
| Variability-vs-energy framework (lag-energy, covariance spectra) | Required for spectral timing science |
| Lomb-Scargle periodograms | Needed for unevenly sampled data |
| Comprehensive test suite validating against Stingray Python | Essential for scientific credibility |
| Plot recipes (Plots.jl / Makie.jl) | Needed for rapid data exploration |
| Documentation and tutorials | Critical for user adoption |

#### 1.2 Anticipated Scientific Impact

This project will enable Julia-native astronomers to perform complete spectral timing analyses. Key scientific use cases include:

##### Quasi-Periodic Oscillation (QPO) Detection

A QPO detection pipeline in Stingray.jl to streamline the analysis of X-ray timing data from compact objects. The pipeline will read NICER photon event data, generate averaged power spectra with proper Leahy normalization, and apply logarithmic frequency rebinning. This will enable clear identification of quasi-periodic oscillations (QPOs) as peaks above the Poisson noise level. By automating the current multi-step workflow, the implementation will reduce complex analysis to a simple, efficient pipeline for studying black holes and neutron stars.

##### X-ray Reverberation Mapping

A cross-spectral timing pipeline in Stingray.jl to measure reverberation time lags between soft and hard X-ray energy bands. 
The pipeline will filter photon events into soft (0.3-1.0 keV) and hard (2.0-10.0 keV) bands, compute cross spectra using segmented light curves, and derive frequency-dependent time lags and coherence. 
This will enable the study of light-travel-time delays caused by disk reflection of coronal X-rays. 
By simplifying the current multi-step workflow into a streamlined pipeline, the implementation will make reverberation analysis more accessible for constraining black hole spin and corona geometry.

([Uttley et al. 2014](https://ui.adsabs.harvard.edu/abs/2014A%26ARv..22...72U))
([Vaughan & Nowak 1997](https://ui.adsabs.harvard.edu/abs/1997ApJ...474L..43V))

##### Variability-vs-Energy Spectra

lag-energy and covariance spectral analysis tools in Stingray.jl to study how X-ray variability changes across different energy bands. 
The pipeline will divide photon events into multiple energy bands, compute cross spectra against a broad reference band, and measure averaged time lags within a selected frequency range. 
This will produce lag-energy spectra, revealing which spectral components vary coherently and how they respond over time. 
Such analysis provides key diagnostics of disk reflection features, including the characteristic iron-line reverberation signature from the inner accretion disk.

#### 1.3 System Architecture

FITS File -> read_events -> EventList -> create_lightcurve / filter_energy -> LightCurve / Filtered EventList -> FFT / Cross-Correlate -> PowerSpectrum / CrossSpectrum -> rebin -> Rebinned PDS -> Derive Metrics -> Coherence / Time Lags / Covariance Spectrum / Lag-Energy Spectrum

All Results -> Plot Recipes (Makie/Julia) -> Rapid Visual Exploration

**Module Organization (proposed)**:

```
src/
|-- Stingray.jl          # Module root (existing)
|-- events.jl            # EventList, FITS I/O (existing)
|-- lightcurve.jl        # LightCurve (existing)
|-- fourier.jl           # Low-level Fourier operations (existing)
|-- gti.jl               # GTI operations (existing)
|-- utils.jl             # Utilities (existing)
|-- powerspectrum.jl     # [NEW] Powerspectrum / AveragedPowerspectrum
|-- crossspectrum.jl     # [NEW] Crossspectrum / AveragedCrossspectrum
|-- coherence.jl         # [NEW] Coherence and time lag computation
|-- covariance.jl        # [NEW] Covariance spectra
|-- varenergyspectrum.jl # [NEW] Variability-vs-energy framework
|-- lombscargle.jl       # [NEW] Lomb-Scargle periodograms
`-- plots/               # [NEW] Plot recipes extension
    `-- recipes.jl
```

#### 1.4 Summary

This project will transform Stingray.jl from a foundational codebase into a more complete spectral timing analysis framework for Julia.
By implementing cross-spectral products (time lags, coherence), variability-vs-energy techniques (covariance spectra, lag-energy spectra),
and user-facing types with plot recipes, we will provide Julia astronomers with a high-performance alternative to the Python Stingray package.

The work is scientifically motivated by the analysis needs of current X-ray missions (NICER, NuSTAR, XMM-Newton) and technically grounded in the existing codebase architecture established during GSoC 2022 and 2025.

### 2.0 Deliverables

#### 2.1 High-Level Spectral Types (`Powerspectrum`, `Crossspectrum`)

- `Powerspectrum` and `AveragedPowerspectrum` Types

Structured Powerspectrum and AveragedPowerspectrum types in Stingray.jl to provide a clean and type-safe interface for power spectral analysis. Instead of returning a raw DataFrame, the implementation will encapsulate frequency, power, normalization, and observational metadata within a dedicated Julia type. 
This design will mirror the structure of the Python Stingray API while leveraging Julia's parametric types for performance and type stability. The new hierarchy will also enable future extensions such as dynamical power spectra without breaking existing workflows.

- `Crossspectrum` and `AveragedCrossspectrum` Types

A structured Crossspectrum type in Stingray.jl to support cross-spectral analysis between different X-ray energy bands. The type will store complex cross-spectral power, where the real part represents the co-spectrum and the imaginary part represents the quadrature spectrum.
From this complex representation, phase differences can be computed to derive frequency-dependent time lags between signals.
Additional fields will store auxiliary power spectra and photon counts to enable accurate coherence calculations and proper noise estimation.
This implementation will provide a clear, type-safe interface for reverberation and spectral-timing studies.

- Rebinning for Spectral Types

Logarithmic rebinning is essential for broadband noise studies (as shown in my PR #71):

I will implement logarithmic rebinning functionality for spectral timing types in Stingray.jl to improve the analysis of broadband noise in power spectra. 
The method will group frequency bins geometrically, where each successive bin is slightly wider than the previous one, producing evenly spaced points on a logarithmic frequency scale. Within each bin, frequencies will be combined using the geometric mean and powers will be averaged with proper error estimation. 
This process enhances the signal-to-noise ratio at higher frequencies while preserving the overall spectral structure. The implementation will integrate directly with the new spectral types to provide a clean and efficient post-processing step.

---

#### 2.2 Time Lags and Coherence

- Time Lag Computation

The time lag between two signals is derived from the phase of the cross spectrum:

I will implement time lag computation functionality in Stingray.jl to measure frequency-dependent delays between two X-ray signals. 
The method will derive time lags from the phase of the complex cross spectrum and convert them into physical delays using the frequency of each Fourier component. 
The implementation will also include error estimation based on signal coherence and the number of averaged segments. This will provide a robust tool for quantifying delays between soft and hard X-ray bands. 
Such measurements are critical for studying disk-corona interactions and X-ray reverberation around compact objects.
[Nowak et al. (1999, Eq. 16)](https://ui.adsabs.harvard.edu/abs/1999ApJ...510..874N)

- Coherence Computation

The coherence function measures the linear correlation between two signals as a function of frequency:

Coherence computation utilities in Stingray.jl to quantify the linear correlation between two X-ray signals as a function of frequency. The method will compute coherence from the cross spectrum and auxiliary power spectra, with optional bias correction to remove Poisson noise effects. This will provide estimates of intrinsic coherence along with associated uncertainties for reliable spectral-timing analysis. By wrapping existing low-level functions into a clean user-facing API, the implementation will make coherence analysis easier and more consistent across workflows. Such measurements are essential for determining whether variability in different energy bands originates from the same physical process.

[Vaughan & Nowak (1997)](https://ui.adsabs.harvard.edu/abs/1997ApJ...474L..43V)

---

#### 2.3 Covariance Spectra and Variability-vs-Energy Framework

- Covariance Spectrum

The covariance spectrum isolates the variable component of the spectrum that is coherent with a reference band 
([Wilkinson & Uttley 2009](https://ui.adsabs.harvard.edu/abs/2009MNRAS.397..666W)):

- Lag-Energy Spectrum

The lag-energy spectrum shows how time lags vary with photon energy, revealing the geometry of the X-ray emitting region:
[Nowak et al. (1999)](https://ui.adsabs.harvard.edu/abs/1999ApJ...510..874N)
[Uttley et al. 2014](https://ui.adsabs.harvard.edu/abs/2014A%26ARv..22...72U)

---

#### 2.4 Lomb-Scargle Periodograms

For unevenly sampled data (gaps due to Earth occultations, GTI filtering), the Lomb-Scargle method provides frequency analysis without requiring binning:

---

#### 2.5 Comprehensive Test Suite

Every deliverable must be validated against the Python Stingray package outputs. The test strategy:
- **Powerspectrum tests**: [Leahy-normalized](https://ui.adsabs.harvard.edu/abs/1983ApJ...266..160L) Poisson noise power should have $\chi^2$ distribution with mean 2.0
- **Crossspectrum tests**: two correlated signals with known 3-bin lag (~0.012s via `circshift`)
- All tests cross-validated against [Python Stingray](https://ui.adsabs.harvard.edu/abs/2019ApJ...881...39H) outputs for numerical agreement


---

#### 2.6 Plot Recipes and Documentation

- Plot Recipes (Plots.jl/Makie.jl)

Addressing [Issue #29](https://github.com/StingraySoftware/Stingray.jl/issues/29):

- Documentation

Using Documenter.jl with tutorials:

---

### 3.0 Timeline

|Period|Description|
|------|-----------|
| **Community Bonding** | Finalize design docs with mentors; study Python Stingray cross-spectrum implementation; generate reference datasets for validation. **Deliverables:** Design RFC approved; validation data ready |
| **Week 1** | Implement `Powerspectrum` struct and constructor from EventList. **Deliverables:** `powerspectrum.jl` with EventList constructor |
| **Week 2** | Implement `Powerspectrum` constructor from LightCurve; add `show`, `summary` methods. **Deliverables:** LightCurve constructor; display methods |
| **Week 3** | Implement frequency rebinning (`geometric_rebin`, `linear_rebin`) for Powerspectrum. **Deliverables:** `rebin()` for Powerspectrum |
| **Week 4** | Comprehensive test suite for Powerspectrum (Leahy, frac, abs normalizations). **Deliverables:** `test_powerspectrum.jl` with >=90% coverage |
| **Week 5** | Implement `Crossspectrum` struct and constructor from two EventLists. **Deliverables:** `crossspectrum.jl` with EventList constructor |
| **Week 6** | Implement `time_lag()` and `coherence()` functions; error estimation. **Deliverables:** `coherence.jl`; lag and coherence computation |
| **-- Midterm Evaluation --** | *Submit midterm deliverables*: Powerspectrum, Crossspectrum, time lags, coherence. **Deliverables:** All M1-M3 milestones complete |
| **Week 7** | Implement `CovarianceSpectrum` type and constructor. **Deliverables:** `covariance.jl` |
| **Week 8** | Implement `LagEnergySpectrum` type and constructor. **Deliverables:** `varenergyspectrum.jl` |
| **Week 9** | Test suite for covariance and lag-energy spectra; validation against Python Stingray. **Deliverables:** `test_covariance.jl`, `test_varenergy.jl` |
| **Week 10** | Implement Lomb-Scargle periodogram. **Deliverables:** `lombscargle.jl` |
| **Week 11** | Plot recipes (Makie.jl and/or Plots.jl) for all spectral types. **Deliverables:** `plots/recipes.jl` |
| **Week 12** | Documentation, tutorials, example notebooks, performance benchmarks. **Deliverables:** `docs/` tutorials; benchmark report |
| **-- Final Evaluation --** | *Final submission*: All deliverables, documentation, blog post. **Deliverables:** Complete spectral timing framework |

## 4.0 GSoC

### Have you participated previously in GSoC? When? With which project?
No - This is my first time

### Are you also applying to other projects?
No - I am only applying to this project

### Availability

- **Commitment**: I am available for a full-time (350 hours) contribution during the GSoC period
- **Working hours**: 35-40 hours per week
- **Communication**: Daily async updates via Matrix/Slack; weekly video calls with mentors
- **No holidays planned as of now
- If I have any academic commitments that may temporarily reduce my availability, I will communicate this to mentors at least 2 weeks in advance and plan to front-load work accordingly.

## 5.0 Other comments

### Pre-Community Bonding and Current Working

#### What I've Already Done

### Codebase Familiarization
- Thorough study of all 6 source files: `events.jl` (1233 lines), `fourier.jl` (762 lines), `lightcurve.jl` (878 lines), `gti.jl` (1044 lines), `utils.jl` (98 lines)
- Understanding of the `EventList -> LightCurve -> avg_pds_from_events / avg_cs_from_events` pipeline
- Analysis of existing normalization schemes (Leahy, fractional rms, absolute rms)
- Deep familiarity with the GTI handling infrastructure

### Issue Awareness
I've studied all open issues and understood the roadmap for further work

### Real Data Analysis

Basic Structures of Codes Have been Stimultated here in my repo : [Spectral-Timing-Notebook-Om](https://github.com/Omiii-215/spectral-timing-notebooks)

I have worked with the NICER observation `ni1200120104_0mpu7_cl.evt` (828 MB, 21.2M events) to generate the visualizations, demonstrating capability with real X-ray timing data.

#### Pre-Bonding Period Plans

Before the official coding period, I plan to deeply connect with mentors and try to:

1. **Complete any pending PRs** (#71, #75) based on mentor feedback
2. **Study the Stingray Python Crossspectrum implementation** in depth to ensure API parity
3. **Write a design document** (RFC) for the `Powerspectrum`/`Crossspectrum` types for mentor review
4. **Generate reference outputs** from Python Stingray for cross-validation
5. **Set up benchmarking infrastructure** to measure Julia vs. Python performance improvements

### Some of My Projects

[Bionium-X](https://github.com/Omiii-215/Bionium-X)

### What I Am Studying and For What

I am studying computer science and scientific computing, focusing on data science, astrophysics, and AI-driven research applications. Working with technologies like Python, Julia, machine learning, and data analysis frameworks. Alongside this, I explore game development and advanced graphics pipelines and build technical projects whose overall goal is to apply advanced computing and AI & ML to scientific research and complex data problems..