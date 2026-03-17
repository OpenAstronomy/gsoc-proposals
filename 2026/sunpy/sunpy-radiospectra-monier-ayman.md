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

# **Method to Retrieve Frequency from Index in Spectrogram**

## **Introduction**

The radiospectra library, part of the SunPy ecosystem, provides tools for working with radio spectrogram data used in solar physics. A spectrogram represents radio intensity as a function of **time and frequency**, typically stored as a two-dimensional array.

**Problem Definition:** The **`GenericSpectrogram`** class provides access to the frequency axis through **`freq_axis`**, but it does not offer a direct method to retrieve the frequency corresponding to a specific index. In many data analysis workflows, particularly in machine learning and signal processing, operations are performed on array indices rather than physical values. As a result, users must manually access the frequency axis and perform the conversion themselves. Providing a dedicated method to convert indices to frequencies would simplify this process and improve usability.

**Solution:** The solution for this problem could be done with the following:

* Implement a new method **`freq_at_index(idx)`** in the **`GenericSpectrogram`** class to return the frequency corresponding to a given index on the frequency axis using the existing **`freq_axis`** information.  
* Add validation to ensure the provided index is within the valid bounds of the frequency axis and raise appropriate errors for invalid indices.


## **Project Deliverables**

This project will focus on improving the usability of spectrogram data handling in the radiospectra library by adding utilities for retrieving frequency values from spectrogram indices.

**This project will focus on the following tasks:**

* Implement **`freq_at_index(idx)`** and validate index bounds.  
* Implement additional utilities: **`index_at_freq(freq)`**, **`time_at_index(idx)`**, and **`index_at_time(time)`**.  
* Create a **`utils.py`** file to host helper functions for validation and conversions, while keeping the core **`GenericSpectrogram`** class focused on data storage and manipulation.  
* Write documentation strings for the new method and provide usage examples.  
* Add unit tests to verify the correctness of the implementation.  
* Update the package documentation to include the new functionality.

In addition to the above tasks, other crucial aspects must be integrated into the implementation process:

* Clean, reliable, and maintainable code.  
* Blog posts containing a log of everything that has been done and what's next.

## **Implementation**

**Design the Class Diagram**

The following diagram describes the whole **sunpy.radiospectra** classes in a high-level fashion:

**![][image1]**

**Refactoring**

Inside the **`GenericSpectrogram`** class and **`radiospectra`** package, I plan to refactor and improve the existing code to support the new functionality more cleanly. This includes the following tasks:

* **Reorganize** frequency-related methods (e.g., **`freq_axis`** access, **index-to-frequency conversions**) to reduce **complexity** and improve **maintainability**.  
* **Standardize** attribute names and internal data structures for consistent access to **frequency** and **time axes**.  
* Move **helper functions** for **index-time** and **index-frequency conversions** into **`utils.py`**, keeping the core **`GenericSpectrogram`** class focused on **data storage and manipulation** and avoiding **code duplication**.  
* Ensure each **refactored function** includes clear **docstrings** and **validation**, making it easy for **future developers** to maintain and extend the functionality.

**Testing**  
Writing tests is a crucial part of the software development lifecycle. Most software products should have automated testing processes to ensure correct functionality. The radiospectra library is no exception. Each new function and helper utility, including **`freq_at_index(idx)`** and other index/coordinate conversion functions, will be accompanied by test cases in **`test_spectrum.py`** to assert correct functionality for valid and out-of-bounds inputs.

**Documentation Strings**

Like testing, documentation is a critical part of any software product. Documentation strings will be added to all new functions, including **`freq_at_index(idx)`** and other index/coordinate conversion utilities, to help future developers maintain and extend the codebase. For users, examples and usage guides will be provided to illustrate typical workflows with spectrogram data. Existing radiospectra docstrings and coding standards will be followed to ensure consistency and clarity.

## **Timeline**

**Community Bonding Period   (May 1 \- 24):**

**Week 1-2   (May 1 \-  15):**

* Getting to know mentors, deep dive into documentation, getting up to speed to begin working on the project.  
* Socialize and interact with other community members (students and mentors)  
* Learn more about OpenAstronomy as an organization with a mission and vision  
* Familiarize myself with the radiospectra architecture and codebase  
* Clarify ambiguous portions in the proposal  
* Start a blog

**Week 3   (May 16 \-  24):**

* Plan and design the **`freq_at_index(idx)`** method and supporting utilities for **`GenericSpectrogram`**  
* Refactor frequency-related methods to simplify the code and support new functionality  
* Begin creating **`utils.py`** for helper functions (e.g., index-to-frequency and index-to-time conversions).

**Week 4   (May 25 \- 31):**

* Complete implementation of **`freq_at_index(idx)`** and supporting utilities  
* Add documentation strings and usage examples  
* Write initial **unit tests** for the new methods  
* Create an initial pull request and seek reviews from mentors

**Final Exams  (June 1 \- June 30):**

* I will be studying and taking final exams in college, but I will be active in the community

**Week 5  (July 1 \- July 8):**

* Complete refactoring frequency-related methods in **`GenericSpectrogram`**  
* Ensure all helper functions in **`utils.py`** are implemented correctly and tested  
* Creating a pull request for **GenericSpectrogram** updates and **utils.py** and seek reviews

**Midterm Evaluation  (July 6 \- 10\)**

**Week 6  (July 11 \- July 17):**

* Write comprehensive unit tests for **`freq_at_index(idx)`** and all utilities, covering valid and out-of-bounds cases  
* Update documentation and examples to reflect test coverage  
* Submit pull requests for **`test_spectrum.py`** and utility tests.

**Week 7 (July 18 \- July 25):**

* Seeing if there is additional work to be done, and seeking reviews before the final evaluation.

**Final Evaluation**

[image1]: <data:image/png;base64,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>