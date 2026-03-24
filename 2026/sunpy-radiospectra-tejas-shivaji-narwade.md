  
			  
	

Project Proposal  
[Improving radiospectra’s functionality and interoperability](https://openastronomy.org/gsoc/gsoc2026/#/projects?project=improving_radiospectra%E2%80%99s_functionality_and_interoperability)

Mentors: [`samaloney`](https://github.com/samaloney), [`hayesla`](https://github.com/hayesla)

Personal Information  
Name: TEJAS SHIVAJI NARWADE  
emails: [Tejas Narwade](mailto:tejasnarwade.eng@gmail.com) [Tejas Narwade](mailto:tejasnarwade2k5@gmail.com)  
Github: [Tejas Narwade](https://github.com/tejasnarwade)  
Matrix: @tejasnarwade:matrix.org  
Time Zone: IST (GMT \+ 5:30)

    

* **PR link(s):**  
- [\[radiospectra \#178\] Improve metadata handling in spectrogram sources.](https://github.com/sunpy/radiospectra/pull/178)  
- [\[radiospectra \#177\] Add test for missing instrument metadata.](https://github.com/sunpy/radiospectra/pull/177)  
- [\[radiospectra \#175\] Improve robustness using `dict.get` for metadata.](https://github.com/sunpy/radiospectra/pull/175)  
- [\[radiospectra \#174\] Fix doctest failure for optional dependencies.](https://github.com/sunpy/radiospectra/pull/174)

* ### **Background**

I am a developer focused on astronomical data processing and Python software engineering. Through my recent contributions to `sunpy/radiospectra`, I have developed a deep understanding of how different radio instruments (RPW, CALLISTO, etc.) store and manage metadata. I am proficient with the SunPy core stack, `pytest` for robust testing, and the challenges of handling "messy" real-world observational data.

* ### **Interest in OpenAstronomy**

I am passionate about building tools that make solar physics more accessible. `radiospectra` is a vital tool for studying solar transients, and I am eager to help modernize it so that it reflects the high standards of the broader SunPy and Astropy ecosystems.

## **\# Project Proposal Application**

**Proposal Title:** Modernizing Radiospectra: WCS-Aware Data Structures and Enhanced Background Subtraction

**Organisation:** SunPy (OpenAstronomy)

### **Summary:**

Current `radiospectra` data structures treat spectrograms as simple 2D arrays, making it difficult to perform analysis across different instruments or time-ranges accurately. This project will redesign the core `Spectra` object to be **coordinate-aware** using the World Coordinate System (WCS).

By leveraging `astropy.wcs` and potentially `ndcube`, I will create a framework where slicing data by Time or Frequency is intuitive and mathematically sound. Additionally, I will implement a standardized API for background subtraction and improve visualization to handle data gaps, ensuring that `radiospectra` is a modern, extensible foundation for the solar community.

### **1.0 Proposed System Architecture**

The new `Spectra` object will act as a high-level wrapper that bridges raw data with coordinate-aware analysis.

* **Data Layer:** Uses `numpy.ma` (Masked Arrays) to handle RFI (Radio Frequency Interference) and instrument gaps.  
* **Coordinate Layer:** An `astropy.wcs.WCS` object generated from instrument metadata.  
* **Interface Layer:** Consistent API for slicing, plotting, and exporting to `xarray` or `pandas`.

### **2.0 Key Deliverables**

1. **A Redesigned `Spectra` Class:** A WCS-aware object supporting axis-aware operations (slicing by time/frequency).  
2. **Background Subtraction Module:** Built-in methods for "Constant," "Rolling Median," and "Polynomial" subtraction.  
3. **Gap-Aware Visualizations:** Robust plotting routines that use `pcolormesh` and masked arrays to properly represent missing data.  
4. **Interoperability Suite:** Exporters for `xarray.Dataset` and `ndcube.NDCube`.  
5. **Example Gallery:** Documentation showing end-to-end workflows (Cleaning data \-\> Background Subtraction \-\> Feature Identification).

3.0 GSOC timeline and my availability 

| Period | Tasks |
| ----- | ----- |
| **May 8 \- May 20** (Community Bonding) | **Architecture Design:** Discuss inheritance (e.g., from `ndcube.NDCube`) vs. composition for the new `Spectra` object. **Metadata Audit:** Review instrument-specific FITS keywords (CALLISTO, RPW, etc.) for accurate WCS mapping. |
| **May 20 \- June 20** (Coding Period) | **Core I/O & Metadata:** Implement robust FITS reading for radio data, ensuring all instrument metadata is safely ingested (extending my current work in PR \#178).  **Spectral Foundations:** Build core periodogram functionality and axis-aware slicing.  **Modular Design:** Create reusable modules for handling frequency grids and time-series data. **Feedback Loop:** Document initial code via Jupyter Notebooks for mentor review. |
| **June 20 \- July 2** (Midterm Evaluation) | **Optimization:** Finalize and polish the `Spectra` I/O tools. **Cross-Spectral Analysis:** Implement cross-spectrum routines and validate them against synthetic datasets. **Standards Compliance:** Ensure all coordinate transformations follow IAU and Astropy conventions. **Midterm Reporting:** Submit progress reports and update the project blog. |
| **July 2 \- August 2** (Coding Phase 2\) | **Advanced Timing:** Implement **Time Lag** and **Coherence Spectra** modules. **Energy/Frequency Variability:** Integrate tools for analyzing variability vs. frequency using covariance spectra.  **Performance:** Optimize data handling for large datasets (e.g., high-cadence NICER-style radio observations).  **Visualization:** Finalize recipes for plotting energy-dependent results with automated gap handling. |
| **August 2 – August 25** (Final Evaluation Week) | **Benchmarking:** Perform final stress tests on large FITS files.  **Refactoring:** Replace any deprecated patterns with robust, struct-like Python class implementations. **Documentation:** Deliver final tutorials, API references, and a "Getting Started" guide. **Submission:** Complete final mentor evaluations and submit the code to the upstream repository. |

After **Section 3.0 (Technical Strategy)**, a high-quality GSoC proposal needs to pivot from *how* you will build it to *how* you will ensure it stays built and stays high-quality.

To match the "Gold Standard" of the Julia proposal you uploaded, your next sections should be **Testing & Quality Assurance**, **Documentation**, and your **Detailed Track Record**.

Here is the content for the remaining sections of your proposal:

---

## **4.0 Testing and Quality Assurance**

A redesign of this scale requires a "Test-Driven Development" (TDD) approach to ensure no regressions occur in the existing `radiospectra` functionality.

* **Unit Testing:** I will use `pytest` to verify every new method in the `Spectra` class. This includes edge cases like null metadata, empty frequency arrays, and non-standard FITS headers.  
* **Validation against Python-Stingray/SunPy:** To ensure scientific accuracy, I will cross-validate the outputs of my new periodogram and background subtraction routines against established results from `sunpy.map` and the original `Stingray` Python implementation.  
* **Continuous Integration (CI):** I will maintain the GitHub Actions workflows I’ve already worked on (referencing PR \#174) to ensure that every change passes across different Python versions and OS environments.

## **5.0 Documentation and Community Engagement**

Code is only as good as its documentation. My goal is to make the new `Spectra` object the easiest tool for solar radio astronomers to use.

* **The "Radiospectra Gallery":** I will create at least three comprehensive Jupyter Notebooks:  
  1. *Basic Slicing:* Moving from a raw FITS file to a WCS-aware slice.  
  2. *Cleaning Data:* A step-by-step guide to background subtraction.  
  3. *Advanced Analysis:* Using coherence and time-lag spectra on multi-instrument data.  
* **Inline Documentation:** I will follow **NumPy-style docstrings** for all new functions, ensuring that parameters, return types, and "Notes" on astrophysical conventions are clear.

## **6.0 Track Record & Open Source Contributions**

I am not starting from scratch. I have already integrated myself into the `radiospectra` workflow:

* **\[PR \#178\] Metadata Handling:** This was a direct prerequisite for this project. By fixing how spectrogram sources handle metadata, I have cleared the path for WCS implementation.  
* **\[PR \#177\] Safety Tests:** Demonstrated my commitment to code reliability by catching "missing metadata" bugs before they hit production.  
* **\[PR \#175\] Robust Access:** Refactored core access patterns to prevent crashes during data ingestion.

## **7.0 Appreciation & Conclusion**

I would like to express my gratitude to the **OpenAstronomy** and **SunPy** communities. Engaging with mentors `@samaloney` and `@hayesla` over the past few weeks has been incredibly insightful. I am confident that my mix of technical Python skills and my growing understanding of solar radio data makes me the right candidate to lead this redesign.

---

### **8.0 GSoC Required Questions**

* **Have you participated previously in GSoC?** No, this is my first application.  
* **Are you applying to other projects?** No, I am 100% committed to SunPy.  
* **Availability:** I am available 40 hours/week from May to August. I have no conflicting internships or classes.

---

