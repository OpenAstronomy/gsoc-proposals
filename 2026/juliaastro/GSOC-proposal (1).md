# **Google Summer of Code Proposal: Spectra.jl across the electromagnetic spectrum**

## **Contact Information**

| Field | Detail |
| :---- | :---- |
| **Name** | Alisha |
| **Email** | Alisha Patel |
| **University** | Scaler School of Technology (2nd Year) |
| **Time Zone** | $IST (GMT+5:30)$ |
| **GitHub Profile** | Alisha1701 |
| **Other GitHub** | Alisha-2005 |

---

## **Project Details**

* **Project Title:** Spectra.jl across the electromagnetic spectrum  
* **Mentors:** To be determined (OpenAstronomy/Spectra.jl)  
* **Organization:** JuliaAstro / Spectra.jl

---

## **Why I am a Good Fit for This Project**

I have spent significant time learning the **Julia language** and its benefits, such as **Multiple Dispatch** and **Composability**, which are essential for a package like Spectra.jl that must handle diverse data types (Radio, X-ray, Optical) under a uniform interface.

I have a strong foundation in **FITS file structures**, having already grasped basic column terminology and the use of **FITSIO.jl**. My approach focuses on "avoiding the noise" and focusing on precision and robustness rather than high-volume, low-quality code. I am passionate about space-obsessed analysis and am eager to transition from "generating code" to "thinking like an astronomer".

---

## **Work Approach: The "Atomic PR" Method**

A key challenge in open-source is reviewing massive Pull Requests. I commit to submitting all work in **atomic PRs**—small, manageable, and highly tested chunks. This ensures that work remains reviewable and robust, preventing the bottlenecks seen in previous years.

---

## **Proposed Work & Milestones**

### **Phase 1: Core Integration & OGIP Parser (Weeks 1–6)**

The goal is to provide a uniform interface for loading datasets and metadata by leveraging existing work in SpectralFitting.jl.

* **Week 1-2: Enhanced FITS I/O & TDD Setup**  
  * Establish a comprehensive test suite using **Test Driven Development (TDD)**.  
  * Curate a library of spectral data samples for compatibility testing.  
* **Week 3-4: OGIP Parser Migration**  
  * Move and integrate the **OGIP (Office of Guest Investigator Programs) parser** from SpectralFitting.jl into Spectra.jl.  
  * Ensure the parser correctly handles X-ray and Gamma-ray headers common in OGIP standards.  
* **Week 5-6: Primary Spectral Type Implementation**  
  * Implement data loading for a specific spectral datatype (e.g., X-ray or Optical).  
  * **Deliverable (1st Evaluation):** Integrated OGIP parser and a working loader for the first chosen file format.

### **Phase 2: Multi-Wavelength Framework & Routines (Weeks 7–12)**

Expanding the library to support diverse electromagnetic ranges and common analysis manipulations.

* **Week 7-8: Multi-Format Support**  
  * Implement data loading for at least two additional file formats (e.g., Radio or Infrared spectra) to ensure the library handles different physics and instrument types.  
* **Week 9-10: Common Analysis Routines**  
  * Implement **Rebinning Heuristics**: Create functions to rebin data based on minimum flux, signal-to-noise ratio, or count rates.  
  * Implement **Unit Conversions**: Support transformations between wavelength (Angstrom/nm) and energy (keV/eV) or frequency (Hz).  
* **Week 11-12: Refinement & Documentation**  
  * **Integration Testing:** Ensure cross-compatibility between loaders.  
  * **Documentation:** Create a **Pluto.jl notebook** demonstrating the loading of a star's spectrum and performing a "first-look" analysis.  
  * **Deliverable (Final Evaluation):** A revamped Spectra.jl with support for 3+ file formats and a robust rebinning/transformation API.

---

## **Design Principles**

* **Julia Idioms:** Use parametric types to handle different spectral ranges (Radio vs X-ray) without OOP overhead.  
* **Composability:** Ensure Spectra.jl types "just work" with DataFrames.jl and plotting recipes.  
* **Performance:** Utilize @views and in-place operations to handle large spectral datasets efficiently.

---

## **Timeline Summary**

| Week | Phase | Deliverable |
| :---- | :---- | :---- |
| **Pre-coding** | Community Bonding | Learn spectral data types (Radio/X-ray differences), explore SpectralFitting.jl internals. |
| **1-2** | Phase 1 | TDD infrastructure and spectral data library curation. |
| **3-4** | Phase 1 | OGIP Parser migration and integration. |
| **5-6** | Phase 1 | First spectral datatype loader; **1st Evaluation**. |
| **7-9** | Phase 2 | Implementation of 2+ additional file formats (Radio/Optical). |
| **10-11** | Phase 2 | Rebinning heuristics and unit conversion routines. |
| **12** | Phase 2 | Final cross-validation, Pluto notebook demo, and **Final Evaluation**. |

---

## **My Recent Contributions**

* **Stingray.jl:** Refactored plotting recipes and added test suites (\#72).  
* **Stingray Notebooks:** Fixed ntrial calculation in Pulsar search notebook (\#125).  
* **OpenAstronomy:** Alphabetized members list and improved student guide visibility.

I am fully available during the GSoC coding period and will manage my academic load at Scaler School of Technology through proactive communication with my mentors.

