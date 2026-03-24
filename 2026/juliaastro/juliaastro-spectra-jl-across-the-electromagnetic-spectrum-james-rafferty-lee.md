# Spectra.jl Across the Electromagnetic Spectrum

## Contributor Information
* **Name:** James Rafferty Lee
* **Time-zone:** UTC+7 (WIB)
* **Matrix/slack/IRC Handle:** @jamesrlee:matrix.org
* **Github/forge username:** [@jamesrafe](https://github.com/jamesrafe)
* **Blog:** 
* **Blog RSS feed:**
* **PR link(s):**
  * https://github.com/JuliaAstro/SpectralFitting.jl/pull/239
  * https://github.com/JuliaAstro/SpectralFitting.jl/pull/240

### Background
I’m James R Lee, and I’m a junior software engineer in Indonesia! Professionally, I work in the ride-hailing sector, where I specialize in backend cartography. My day-to-day work involves building high-traffic services that handle massive amounts of traffic. The app also processes enormous amounts of geographic coordinates, massive user data, and spatial transformations every minute. Operating at this scale requires rigorous and careful attention to concurrency, data structuring, and engineering principles that I am eager to apply to the open-source community here in OpenAstronomy!

### Interest in OpenAstronomy
While my professional background is in software development, I have always had a fascination with astronomy. Since I was little, I’ve loved reading sci-fi novels and learning about the stars. Throughout the millions of years of the human race, we have barely stepped outside the bounds of our planet, and yet we know so much about the universe. From understanding the physics and even the photography of black holes to knowing about Pluto's existence before we even saw it. Maybe in comparison to the truth of the universe, it's nothing. But we know so much for a race who has never set foot outside the Kuiper Belt. We are incredibly microscopic in comparison, and yet we have this vast understanding of how the universe works.

I have always been deeply curious about the clever ways astronomers gather and decode information from the universe. Furthermore, I strongly believe in the power of open-source software to accelerate scientific discovery, and I am incredibly excited about the work being done here!

## Project Proposal Application
**Proposal Title:** Spectra.jl: A Uniform Interface for the Electromagnetic Spectrum

**Organisation:** JuliaAstro

### **Summary:**
`Spectra.jl` already handles UV/optical/IR spectra through its `Spectrum{S,F,M,N} <: AbstractSpectrum{S,F}` type hierarchy, but has no capability for loading spectra like X-ray and gamma-ray, as they are different from UV/optical/IR spectra. X-ray astronomy (and other high energy fields) typically operate on spectra that are binned into channels by the measurement device. An instrument response matrix (RMF) is required to map detector channels to physical energies, and an ancillary response file (ARF) provides the telescope's effective collecting area — both are needed to correctly convert raw channel counts into physical flux.

`SpectralFitting.jl` already contains an OGIP parser that handles these formats. It has been agreed upon in the JuliaAstro discussions that the correct long-term architecture is for the spectral *types* to live in `Spectra.jl`, with `SpectralFitting.jl` eventually depending on `Spectra.jl` rather than duplicating data structures. Beyond data loading, observers also need a standard set of tools before any science can be done, such as rebinning according to different heuristics, adjusting the wavelength or energy shifts, or converting between common units. This project implements those tools in `Spectra.jl` as well, making it easier for users of `Spectra.jl`.

I believe I am the candidate to deliver this, as I have hands-on experience with `SpectralFitting.jl`'s internals through the PRs I have made and have studied the OGIP standard and all three codebases (`Spectra.jl`, `SpectralFitting.jl`, and `FITSFiles.jl`) thoroughly in preparing this proposal.

I am well aware of one of my biggest weaknesses, which is my lack of knowledge regarding spectra and its underlying math and physics. But, I have already taken proactive steps to bridge this gap by contributing to the codebase and performing a deep dive to all the documentations, like the OGIP documentation, specutils, and other materials to get me the knowledge I need. I am also looking forward to using the Community Bonding Period and the rest of the GSOC period to learn more about this wonderful field, and its underlying math and physics, with my mentors to ensure the quality of this beautiful codebase. If you let me, I will learn as much as possible from the mentors and this incredible community, and try to contribute back.

The core work is:
- Introduce `BinnedSpectrum` (`Spectrum{S,F,2,1}`) and port the OGIP parser from SpectralFitting.jl
- Implement a data loader for gamma-ray spectra as the second supported format
- Implement a third spectral format (chosen with mentor guidance during the project)
- Add utility functions: rebinning, background subtraction, unit conversions, and wavelength/energy shifts

Together, these deliverables produce a revamped Spectra.jl that can load at least three distinct spectral file formats and provides the common reduction tools observers need before any deeper analysis.

### Deliverables

**1. The `BinnedSpectrum` For X-Ray and OGIP Loader**

X-ray data follows the OGIP standard, which often splits a single observation into multiple files. We already have a working draft of this in SpectralFitting.jl. I will port this over to Spectra.jl.
- Port the `OGIP` module from `SpectralFitting.jl/src/datasets/ogip.jl`, adapting `read_spectrum`, `read_rmf`, `read_ancillary_response`, and `build_response_matrix` to return Spectra.jl types. The response matrix is intentionally kept separate from the spectrum, so that SpectralFitting.jl can wrap them in its own `SpectralData` type.
- Test and document loading for the OGIP data.

The `BinnedSpectrum = Spectrum{S, F, 2, 1}` type alias extends the existing type family (`SingleSpectrum`, `EchelleSpectrum`, `IFUSpectrum`) to accommodate channel-based high-energy data. The `spectral_axis` is an N×2 matrix of `[E_low, E_high]` energy bin edges and `flux_axis` is an N-vector of counts or count rates. Separately, the mentors have proposed adding a `Tag` type parameter to `AbstractSpectrum{S, F, Tag}` to help disambiguate dispatch between different spectrum shapes; this is an orthogonal design decision I will explore in coordination with the mentors.
- Define `BinnedSpectrum` with `Base.show`, `getindex`, arithmetic operators, and uncertainty support via `Measurements.jl` arrays (for error propagation, with error statistics tracked in `meta`).
- Port `ResponseMatrix` and `AncillaryResponse` from `SpectralFitting.jl/src/datasets/response.jl` into Spectra.jl, along with their operations.
- Integrate `Measurements.jl` into the `flux_axis`, replacing manual error tracking with native support for error propagation.
- Write a comprehensive test suite using real OGIP data from various observatories, ensuring correctness of data and usage in code.

**2. Gamma-ray spectral data loader**

I will implement a data loader for gamma-ray spectra.
- Research gamma-ray files from real-world observatories and understand the differences and deviations from the OGIP standard.
- Implement `load_gammaray` (or extend `load_ogip` with dispatch) for gamma-ray data.
- Write tests using at least one public gamma-ray dataset and document the workflow.

**3. Third spectral format (TBD)**

For the final evaluation, the project requires data loading for a significant portion of the spectral library, ideally at least three different file formats. After completing deliverables 1 and 2, I will work with the mentors to choose a third format that is most valuable to the community. The specific choice will be made during the project based on the test library being curated and mentor guidance.

**4. Utility functions**

I will build a set of tools to help scientists reduce their data and prepare it for analysis. These tools already exist in SpectralFitting.jl in a specific form, and I will bring them into Spectra.jl for general purpose.
- I will implement rebinning tools: `rebin(spec, grouping)` for explicit bin merging (adapting the `regroup!` logic already in SpectralFitting.jl), and heuristic variants `rebin_mincounts(spec, n)` and `rebin_snr(spec, threshold)` that automatically merge bins until each group meets a minimum count or signal-to-noise target.
- I will move the subtract_background! code from `SpectralFitting.jl`. This uses a special number called `BACKSCAL` from the file header to make sure we subtract the noise correctly based on the telescope's "view."
- I will add a way to convert units. As the mentors requested, users need to switch between Energy (keV) and Wavelength (Å). I will use the Unitful.jl tool that is already in the package to make this automatic and error-free.

### Description/timeline

|Period|Description|
|------|-----------|
| Community Bonding (May 1 – May 24) | Deepen my knowledge on the codebase and the various formats, like the OGIP documents, the IVOA Spectral Data Model, and gamma-ray documentation. Practice TDD by writing failing tests against the planned results before any implementation is done. Discuss about the project regarding the `BinnedSpectrum` type design, the `Tag` parameter extension, and API with mentors. |
| Week 1 (May 25-31) | Understand and audit every internal dependency of the SpectralFitting OGIP Loader in `SpectralFitting.jl/src/datasets/ogip.jl`. Draft `BinnedSpectrum`, `ResponseMatrix`, and `AncillaryResponse` type definitions. Then, open draft PR for mentor review. |
| Week 2 (June 1-7) | Implement `BinnedSpectrum` fully (with functions like `Base.show`, `getindex`, `firstindex`, `lastindex`), and its arithmetic operators. Add Measurements.jl support for error tracking, while also adding unit tests. |
| Week 3 (June 8–14) | Port `OGIP.read_spectrum` and `OGIP.read_background` returning `BinnedSpectrum`. Add integration testing with real-life data. |
| Week 4 (Jun 15–21) | Port the other functions for OGIP Loader. Also port `ResponseMatrix` with its functions. Implement `load_ogip` public API with automatic path resolution. Draft API documentation page and docstring examples. Do preliminary research on gamma-ray. |
| Week 5 (Jun 22-28) | Deep-dive gamma-ray format research. Identify the differences vs X-ray OGIP. Implement `load_gammaray`, and handle the differences with X-Ray data. |
| Week 6 (Jun 29-Jul 5) | Finish implementing gamma-ray. Integration testing with real-life data for both formats. Address mentor review feedback. Write tutorial docs example for X-ray and gamma-ray. Prepare first evaluation. |
| **First Evaluation (Jul 6 - 10<-Max)** | **Deliverable 1  & 2 complete**: OGIP parser migrated, created `load_ogip` in `Spectra.jl`. `BinnedSpectrum` defined and tested on real-life X-ray data. Two formats (X-ray OGIP and gamma-ray) loading successfully. |
| Week 7 (Jul 13–20) | Work with mentors to decide the third spectral format. Begin research into the chosen format's file structure and any available example data. According to TDD, create failing tests against the desired outcome before implementation. |
| Week 8 (Jul 14–21) | Implement the data loader for the third format. Write tests against curated example files. |
| Week 9 (Jul 22–29) | Complete the third format loader and documentation. Implement `rebin` with explicit grouping. Add tests validated against cases. |
| Week 10 (Jul 30–Aug 5) | Implement utility functions like `convert_units` and `subtract_background!` and validate against known results. |
| Week 11 (Aug 6–13) | Implement the rest of the utility functions and write tests. Extend documentation for utilities. |
| Week 12/Final (Aug 13–20) | Buffer week: address open review comments, add edge-case tests, make sure documentation is complete, finish/address comments on PR. Prepare final evaluation report. |
| **Final Evaluation (Aug 24 Max)** | **Deliverables 3 & 4 complete**: Third spectral format loader implemented. Full utility suite implemented and documented. Tests pass with coverage of most code. At least three file formats loading successfully in Spectra.jl. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No

### Are you also applying to other projects?
No, I have only applied for JuliaAstro

### Schedule availability
- I will be available throughout the period.
- I will be able to dedicate 15-25 hours a week.

## Other comments
- I am always available for async communication as long as I am awake (2 AM to 5 PM UTC)
- If possible, I would like to propose a weekly synchronous meeting, where we can discuss on the progress, as well as any questions regarding the project.
