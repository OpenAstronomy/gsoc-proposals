## **Basic Information**

**Name:** Monier Ayman Monier  
**University:** Suez Canal University, Faculty of Computers and Informatics (2024 \- Present)  
**Time Zone:** GMT+2  
**Overview:** I am a second-year Computer Science student passionate about exploring technology and building impactful solutions. I excel at problem-solving, enjoy tackling challenging technical problems, and approach learning with curiosity and a hands-on mindset.

**Contact Info:**  
[**monierayman2007@gmail.com**](mailto:monierayman2007@gmail.com)  
[**https://github.com/Monier-Ayman**](https://github.com/Monier-Ayman)  
[**https://www.linkedin.com/in/monier-ayman/**](https://www.linkedin.com/in/monier-ayman/)  
**@monier\_ayman:matrix.org**  
**@monier\_ayman (Discord)**

## **Me and GSoC**

* I plan to dedicate approximately **12 hours per week** before the start of GSoC to familiarize myself with the codebase and documentation.   
* During the coding phase of GSoC, I am prepared to invest a minimum of **24 to 30 hours per week** into the project. Furthermore, I am open to continuing my involvement with the SunPy project beyond the GSoC period. I plan to remain involved with the SunPy community after GSoC if additional work arises.

## **Why Me?**

* Over the past weeks, I have spent a great deal of time inspecting the codebase of Sunpy and trying to resolve issues.

* I took a deep dive into the Sunpy documentation and I became familiar with many Sunpy’s modules.

* I joined the matrix [room](https://app.element.io/#/room/#sunpy:openastronomy.org) and became familiar with the project's community and mentors. And I was asking questions about the project and the codebase.

## **My Experience**

* **Contributions to Sunpy:**  
  * [Split contributors into New and Other sections \#8501](https://github.com/sunpy/sunpy/pull/8501) \[Waiting for review\]

* **Contributions to sunpy/radiospectra:**  
  * [Implement freq\_at\_index method for GenericSpectrogram \#138](https://github.com/sunpy/radiospectra/pull/138) *Closed*   
  * [*Fix \#56: Correct Callisto client and observatory fallback \#145*](https://github.com/sunpy/radiospectra/pull/145) *Closed*  
  * [Improve overview.rst with narrative and Quick Start example \#165](https://github.com/sunpy/radiospectra/pull/165)   
    \[Waiting for review\]

* **Personal Projects:**

- **Library Management System [https://github.com/Monier-Ayman/Library-Management-System](https://github.com/Monier-Ayman/Library-Management-System)**  
  - A C++ Library Management System project implemented using **Object-Oriented Programming (OOP)** and key data structures including **Map, Stack, Linked List, and Vector**. This project simulates a small library where users can borrow and return books, and administrators can manage books and users.

- **Mini Search Engine [https://github.com/Monier-Ayman/Mini-Search-Engine](https://github.com/Monier-Ayman/Mini-Search-Engine)**  
  - A C++ search engine using OOP, data structures, and SQLite for indexing and searching word frequencies across documents.

* I’m also a competitive programmer and I’m active on [codeforces](https://codeforces.com/profile/-Monier).

# ---

# **Improving radiospectra’s Functionality and Interoperability**

## **Introduction**

**Problem Definition:** The **`radiospectra`** library provides tools to work with solar radio spectrograms, but its current implementation has several limitations:

* Lack of **coordinate-aware operations** making it difficult to map time and frequency to array indices.  
* Missing utilities from old Spectrogram (e.g., `join_many`, `rescale`, background subtraction).  
* Visualization tools are incomplete, struggling with **gappy or irregular data**.  
* Limited interoperability with SunPy, Astropy, and `xarray` data structures, which makes integration with modern workflows challenging.


These limitations make solar radio data analysis cumbersome and less reproducible.  
**Solution:** Modernizing **`radiospectra`** involves redesigning its core data structures, adding utilities, improving visualization, and ensuring interoperability with scientific Python ecosystems. The solution will address the key limitations identified in the problem definition.

**Coordinate-aware Spectra object:**

* Redesign the **Spectra object** to map **physical coordinates** **(time, frequency)** to **array indices**.  
* Implement utility methods such as **`freq_at_index(idx)`**, **`index_at_freq(freq)`**, **`time_at_index(idx)`**, and **`index_at_time(time)`**.  
* **Enable axis-aware operations:** slicing, scaling, statistics, and rebinning.  
* Ensure integration with **SunPy**, **Astropy**, **xarray**, or **NDCube** for modern workflow compatibility.

**Background subtraction methods:**

* Provide built-in commonly used **background subtraction methods**.  
* Support **user-supplied custom functions** for flexible workflows.

**Enhanced visualization:**

* Handle **gaps**, **masked values**, and **irregular sampling** in spectral data.  
* Provide **sensible defaults** while allowing **user customization**.

**Documentation and examples:**

* Add comprehensive **docstrings** for all new functionality.  
* Create an **example gallery** demonstrating slicing, analysis, background subtraction, and plotting workflows.

**Testing and CI:**

* Implement **unit tests** for all new methods to ensure correctness.  
* Use **continuous integration (CI)** to maintain code quality and prevent regressions.

## **Project Deliverables**

This project aims to improve the functionality and interoperability of the **radiospectra** library by redesigning core data structures and enhancing analysis and visualization capabilities for solar radio spectrograms.

**This project will focus on the following tasks:**

* Implement a **coordinate-aware Spectra object** that maps physical coordinates (time and frequency) to array indices.  
* Add conversion utilities such as `freq_at_index(idx)`, `index_at_freq(freq)`, `time_at_index(idx)`, and `index_at_time(time)`.  
* Support **axis-aware operations** on spectrogram data, including slicing, scaling, and statistical reductions.  
* Integrate **background subtraction methods**, while allowing users to provide custom implementations.  
* Improve **visualization tools** to better handle gappy or irregular spectrogram data.  
* Refactor parts of the radiospectra codebase to improve maintainability and organization.  
* Provide **documentation, example workflows, and an example gallery** demonstrating the new functionality.  
* Add **unit tests and ensure compatibility with the existing CI system** to maintain code quality.

In addition to the above tasks, other crucial aspects must be integrated into the implementation process:

* Clean, reliable, and maintainable code.  
* Blog posts containing a log of everything that has been done and what's next.

## **Implementation**

**Design the Class Diagram**

The following diagram describes the whole **sunpy.radiospectra** classes in a high-level fashion:

**![][image1]**

**Refactoring**

I plan to improve the **`GenericSpectrogram`** class and the radiospectra package to make the code easier to use, maintain, and extend. The main steps are:

* **Organize time and frequency methods** so it’s easier to convert between coordinates (time, frequency) and array indices.  
* **Standardize names and data structures** for consistent access to time and frequency information.  
* **Move helper functions** (like index <-> time and index <-> frequency conversions) into **`utils.py`** to avoid duplicate code. 
* **Improve visualization methods** to handle gappy or irregular data with clear defaults.  
* **Add background subtraction tools** with built-in methods and support for custom user functions.

This refactoring will make the radiospectra code cleaner, easier to maintain, interoperable with the SunPy ecosystem, and ready for the new features of the project.

**Testing**  
Writing tests is a crucial part of the software development lifecycle. All new functionality in the **radiospectra** library—including the coordinate-aware Spectra object, conversion utilities (**`freq_at_index(idx)`**, **`index_at_freq(freq)`**, **`time_at_index(idx)`**, **`index_at_time(time)`**), axis-aware operations, background subtraction methods, and improved visualization for gappy or irregular data—will have dedicated test cases. These tests, written in **`test_spectrum.py`**, will verify correct behavior for valid inputs, edge cases, and out-of-bounds values. **Continuous integration (CI)** will be used to ensure that all tests pass and that new changes do not break existing functionality.

**Documentation Strings**

Documentation is essential for maintainability and usability. Docstrings will be added to all new classes, functions, and utilities, including the Spectra object, conversion methods, background subtraction utilities, and enhanced plotting routines. Usage examples and step-by-step guides will illustrate typical workflows with spectrogram data, including coordinate-based slicing, analysis, background subtraction, and visualization of gappy data. All documentation will follow existing **radiospectra** and **SunPy** coding standards for clarity, consistency, and readability. An **example gallery** (e.g., notebooks or scripts) will showcase the full capabilities of the new functionality.

## **Timeline**

**Community Bonding Period   (May 1 \- 24):**

**Week 1-2   (May 1 \-  15):**

* Get to know mentors and interact with the SunPy community.  
* Deep dive into radiospectra documentation and codebase.  
* Familiarize with SunPy, Astropy, xarray, and NDCube for interoperability.  
* Clarify ambiguous parts of the proposal and plan workflows.  
* Start a blog to track progress and learning.


**Week 3   (May 16 \-  24):**

* **Plan and design the coordinate-aware Spectra object.**  
* **Plan and design supporting conversion utilities:** `freq_at_index(idx)`, `index_at_freq(freq)`, `time_at_index(idx)`, `index_at_time(time)`**.**  
* Begin refactoring frequency- and time-related methods to simplify code.  
* Start creating **`utils.py`** for helper functions (coordinate conversions and reusable logic).

**Week 4   (May 25 \- 31):**

* Implement the **initial version of the Spectra object** with basic coordinate-aware functionality.  
* Implement conversion utilities and integrate with the Spectra object.  
* Begin initial **background subtraction framework** with at least one built-in method.  
* Write **initial unit tests** for new methods.  
* Add documentation strings and usage examples.  
* Submit initial pull requests and seek mentor feedback.

**Final Exams  (June 1 \- June 30):**

* Focus on college exams while remaining active in the SunPy community.  
* Address small code improvements or review comments when possible.

**Week 5  (July 1 \- July 8):**

* Continue refactoring GenericSpectrogram and related methods for maintainability and interoperability.  
* Complete remaining utilities in **`utils.py`** and ensure they are tested.  
* Enhance **visualization methods** to handle gappy, masked, or irregular data.  
* Submit pull requests for refactoring and utils.

**Midterm Evaluation  (July 6 \- 10\)**

**Week 6  (July 11 \- July 17):**

* Implement **axis-aware operations** (slicing, scaling, statistics, rebinning) for Spectra.  
* Complete **background subtraction methods** including support for custom user functions.  
* Write comprehensive **unit tests** covering all new functionality and edge cases.  
* Update **documentation, examples, and usage guides**.  
* Submit pull requests for tests and documentation.

**Week 7 (July 18 \- July 25):**

* Complete remaining improvements based on mentor feedback.  
* Finalize the **example gallery** demonstrating slicing, analysis, background subtraction, and visualization workflows.  
* Ensure all functionality passes CI and final testing.  
* Prepare final pull requests, finalize documentation, and complete project reports.  
* **Review the project for any additional improvements and seek final mentor feedback before the final evaluation.**

**Final Evaluation**

[image1]: <data:image/png;base64,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>