# High-Performance X-Ray Spectral-Timing: Porting Covariance Spectra to Stingray.jl

## Contributor Information
* **Name:** Abdul Mateen
* **Time-zone:** Rawalpindi, Pakistan (PKT / UTC+5)
* **Matrix/slack/IRC Handle:** abdulmateen
* **Github/forge username:** abdulmateen
* **Blog:** abdulmateen.dev
* **Blog RSS feed:** abdulmateen.dev/rss
* **PR link(s):**
  * https://github.com/StingraySoftware/Stingray.jl/pull/76 (Fixes the `fftfreq` CI pipeline import error, demonstrating workflow competence with Stingray.jl's continuous integration and branch management).

### Background
I am a final-semester BSc Computer Science student at Virtual University. My technical focus lies in high-performance computing, type-stable architectures, and handling complex data pipelines. Recently, I engineered data solutions during the Summer of Bitcoin and earned the Microsoft AI-900 Azure AI Fundamentals certification. I prefer writing deterministic, bare-metal code over heavily abstracted object-oriented wrappers. My development philosophy prioritizes strict memory management, cache-locality, and algorithmic efficiency.

### Interest in OpenAstronomy
[cite_start]The high-energy astrophysics community relies on extracting temporal variability signatures from X-ray photon streams[cite: 3]. [cite_start]However, as observatories scale up their temporal resolution, raw event lists routinely exceed tens of millions of discrete photon arrival times[cite: 71]. [cite_start]Python's `stingray` library is fundamentally bottlenecked by CPython's execution model: mandatory runtime dynamic type checking blocks CPU instruction pipelining [cite: 84][cite_start], and continuous temporary array allocations trigger severe Garbage Collector overhead during nested temporal segmentation[cite: 92, 97]. 

[cite_start]I am applying to OpenAstronomy to execute the GSoC 2026 Stingray.jl port because Julia provides the exact architectural primitives necessary to solve these bottlenecks natively[cite: 110]. [cite_start]Bypassing Python's single dispatch via Julia's compile-time devirtualization (Multiple Dispatch) and utilizing zero-allocation pointers (`@views`) presents a rigorous engineering challenge that directly translates into order-of-magnitude performance gains for astrophysical research[cite: 117, 125, 131].

## Project Proposal Application
**Proposal Title:** Spectral timing in Julia: Covariance Spectra Architecture

**Organisation:** Open Astronomy

### **Summary:**
[cite_start]This 350-hour project migrates the core covariance spectrum framework from Python to Julia, replacing deep Object-Oriented Programming (OOP) overhead with type-stable, high-performance computing paradigms[cite: 73, 110]. [cite_start]The legacy Python implementation dynamically allocates heap memory for every time segment, causing devastating L1/L2 cache misses[cite: 95]. [cite_start]My implementation will utilize Julia's `SubArray` architecture (`@views`) to maintain a constant $O(1)$ memory footprint during cross-spectral averaging[cite: 125, 130]. [cite_start]Furthermore, I will resolve the algorithmic flaw in the legacy GTI filtering—where binary masking destroys valid temporal boundaries [cite: 102, 106][cite_start]—by implementing a continuous matrix bounds approach utilizing binary search (`searchsortedfirst`)[cite: 157, 158]. [cite_start]The final mathematical execution will leverage advanced loop-fused broadcasting (`@.`) to push SIMD vectorized array normalization to the theoretical bandwidth limits of the RAM[cite: 135, 138, 142].

Mentors: @fjebaker and @matteobachetti.

### Deliverables
**1. [cite_start]Periodogram Parity Tests Against Python:** Implement the foundational `PowerSpectrum` and `AveragedPowerSpectrum` types[cite: 149]. [cite_start]Encode standard normalizations (Leahy, fractional rms, absolute rms) directly into the parametric type signatures[cite: 150, 151]. [cite_start]Build an integration testing suite via `PythonCall.jl` to process seeded random noise vectors in both languages concurrently, enforcing $10^{-15}$ floating-point parity[cite: 152]. [cite_start]Audit all algorithms using `@code_warntype` to guarantee type-stability[cite: 153].

**2. Cross-Spectrum Algorithms Handling Fragmented GTIs:**
[cite_start]Replace Python's destructive binary masking temporal filter with a native matrix geometry approach for Good Time Intervals (GTIs)[cite: 155, 156, 157]. [cite_start]Utilize `Intervals.jl` or `AbstractMatrix{Real}` bounds coupled with binary search (`searchsortedfirst`) to determine fractional interval containment[cite: 158]. [cite_start]This isolated indexing approach scales efficiently and eliminates the artificial $O(N \log N)$ complexity bottleneck present in the legacy boundary evaluation logic[cite: 159, 160].

**3. Time Lags and Coherence Spectra Architecture:**
[cite_start]Implement advanced diagnostics by extracting the complex phase angle $\arg$ across massive data arrays using `SpecialFunctions.jl`[cite: 165]. [cite_start]Map polymorphic dispatch methods cleanly onto `CrossSpectrum` and `AveragedCrossSpectrum` struct instances[cite: 166]. [cite_start]Error propagation will strictly adhere to Bendat & Piersol (2010) phase variance equations, executed via SIMD Fused Multiply-Add (FMA) instructions for maximal throughput[cite: 167].

**4. The Capstone Covariance Spectra Framework:**
[cite_start]Synthesize prior modules to reverse-engineer and replace `_compute_covariance` and `_calculate_excess_variance`[cite: 170]. [cite_start]Implement iterative cross-spectral calculations concurrently across energy channels using `Threads.@threads`[cite: 171]. [cite_start]Execute reference band generation ($y_{corr}(t) = y_{total}(t) - x(t)$) dynamically in-place utilizing `@views` and pre-allocated `FFTW` workspace plans[cite: 37, 129, 172]. [cite_start]Finalize physical unit scaling via universal loop fusion ($C(E) = \text{Cov}_{unnorm} / \sqrt{\sigma^2_{excess}}$)[cite: 59, 60, 175].


### Description/timeline

|Period|Description|
|------|-----------|
|Community Bonding (May 4 - May 24)| Audit legacy CPython `covariancespectrum.py` source code. Map out the `AbstractTimeSeries` and `AbstractSpectrum` type hierarchies with mentors. Finalize local environment configurations and familiarize myself with the existing `Stingray.jl` codebase.|
| week 1 (May 25 - May 31) | **Deliverable 1**: Define `PowerSpectrum` structs. Implement underlying FFT logic using `FFTW.jl`. Encode Leahy, fractional, and absolute rms normalizations into dispatch signatures. |
| week 2 (Jun 1 - Jun 7) | **Deliverable 1**: Construct integration tests using `PythonCall.jl` against Python Stingray. Execute type-stability audits with `@code_warntype` and `@code_llvm`. Set up CI pipeline checks. |
| week 3 (Jun 8 - Jun 14) | **Deliverable 2**: Begin GTI overhaul. Discard binary masking. Implement `Intervals.jl` bounds mapping for continuous time domains. |
| week 4 (Jun 15 - Jun 21) | **Deliverable 2**: Implement `searchsortedfirst` binary search for fractional interval boundary containment. Benchmark execution speeds for overlapping segments crossing fragmented GTI boundaries. |
| week 5 (Jun 22 - Jun 28) | Buffer week for Deliverable 1 & 2 CI/CD debugging, profiling memory allocations, and writing core `Documenter.jl` documentation strings. |
| week 6 (Jun 29 - Jul 5) | **Deliverable 3**: Develop intrinsic coherence functions. Map polymorphic bounds for `CrossSpectrum` types to remove `isinstance` overhead. |
| week 7 (Jul 6 - Jul 12) | **Deliverable 3**: Implement complex phase angle $\arg$ extraction. Program strict error propagation mapping (Bendat & Piersol 2010) using SIMD FMA instructions. |
| week 8 (Jul 13 - Jul 19) | Initial setup for **Deliverable 4**. Outline the multithreaded architecture for computing covariance matrices across hundreds of energy channels simultaneously. |
| week 9 (Jul 20 - Jul 26) | **Deliverable 4**: Engineer the `_compute_covariance` logic. Implement zero-allocation reference band subtraction ($y_{total} - x_{ci}$) utilizing strictly `@views` to prevent GC stalls. |
| week 10 (Jul 27 - Aug 2) | **Deliverable 4**: Implement `_calculate_excess_variance` logic. Transition away from heuristic NaN-trapping to deterministic variance estimators. Test basic loop-fused scalar mathematical passes. |
| week 11 (Aug 3 - Aug 9) | **Deliverable 4**: Apply multi-threading (`Threads.@threads`) and vectorization (`@.`) for final covariance normalizations ($C(E)$). Run performance benchmarks against the legacy Python implementation. |
| week 12 (Aug 10 - Aug 16) | Final buffer. Complete all integration tests, finalize inline documentation, compile the `Documenter.jl` hosted pages, and prepare the final GSoC report and merged PR list. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time participating in Google Summer of Code.

### Are you also applying to other projects?
No. My focus is exclusively on the Stingray.jl spectral-timing architecture. 

### Schedule availability
I have completed the vast majority of my degree coursework and am only managing final semester requirements (44 remaining credit hours, mostly standalone final exams). I have no planned holidays or external commitments between May 25 and August 16. I am fully available to dedicate 30-35 hours a week to this project.

## Other comments
None.