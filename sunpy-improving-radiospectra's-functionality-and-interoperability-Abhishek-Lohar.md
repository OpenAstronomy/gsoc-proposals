## Page 1

![&lt;img&gt;GitHub logo with code tags&lt;/img&gt;](https://github.com/AbhiLohar/portfolio-site/blob/82aba9367c25599f79e050bb10c4390f85b0b4e7/images/Google_Summer_of_Code_sun_logo_2022.svg.png)

# GSoC Proposal

## Contributor Information:

Name: Abhishek Lohar

University: VIT - AP University.

Email: abhisheklohar0509@gmail.com

GitHub: https://github.com/AbhiLohar

Time Zone: IST (GMT + 5:30)

Chat Handel: @abhisheklohar_01:matrix.org

PR links: open source Code Contributions:

1. Relevant PR: radioscpectra #164

Summary: I refactored the e-Callisto client to fetch remote headers with range requests, to demonstrate that I can work with Fido,FITS file structure and the radioscpectra.net architecture.

My other PR's in SunPy other packages and Astropy:

---


## Page 2

1. PR : [package-template #241](https://github.com/astropy/package-template/pull/241): Update: Syncing template configurations.
2. [sunpy.org #483](https://github.com/sunpy/sunpy.org/pull/483) & [#482](https://github.com/sunpy/sunpy.org/pull/482): Documentation and Website Updates.
3. [astropy #19162](https://github.com/astropy/astropy/pull/19162): Fix: Use list comprehension for better readability and performance
4. [astropy #19149](https://github.com/astropy/astropy/pull/19149): Fix: Simplify and optimize coordinate logic.
5. [astropy #19144](https://github.com/astropy/astropy/pull/19144): Style: Improve code consistency in core utilities.

## Background:

### Interests & Motivation

My interest in open source software development comes from a fascinating combination of high-level software engineering and the raw complexity of solar physics data. In the past few months, I have progressed from minor changes to coding style to tackling architectural problems in the SunPy and Astropy ecosystems.

My involvement in the radiospectra project, particularly in my latest endeavors to improve remote metadata retrieval in e-Callisto, has been a real eye-opener, and I realize that there is a lot of potential to improve the overall state of affairs in solar physics data analysis and discovery. The sheer amount of data available from solar observations is incredible, and I feel that the tools we use to analyze and discover this data are still in a rather disorganized state, particularly in comparison to other branches of astronomy,

---


## Page 3

where a higher level of mathematical rigor is employed in data analysis and discovery.

I'd like to spend my summer working on this project because I think that through integration of standards such as WCS and exploitation of powerful data structures such as NDCube or xarray, we can make solar radio data more accessible to the global research community. My objective is to make sure that a researcher's first experience with radiospectra is smooth, accurate, and physically intuitive. I am excited to use my developing skills in the scientific Python stack to build a foundation that makes life easier for heliophysicists all over the world.

**Interest in OpenAstronomy:**

My enthusiasm for OpenAstronomy really boils down to one thing: I want to make complex solar data easier to use. I've always thought that good science shouldn't be hindered by bad software and by seeing OpenAstronomy's goal of creating a unified, open-source toolkit for all is something I find incredibly inspiring.

By contributing to the PR #164 really opened my eyes, though. It's not just about writing code, it's about realizing how much opportunity there is to learn and to improve the tools heliophysicists use every day. I'm motivated to help radiospectra move towards a coordinate-aware system because I believe researchers should focus their time studying the sun,

---


## Page 4

not array indexing. But beyond all of that, I think the help I've received and will receive in the near future from the community from both GitHub and from the Matrix channels have made me feel motivated , and I'm very excited to contribute and learn from this project and community.

## Project Proposal Application:

**Project Title:** Improving radiospectra's Functionality and Interoperability

**Organisation:** [SunPy] Sunpy organisation

**Mentors:** samaloney, hayesla

**Project Size:** Large (350 hours)

## Project Overview:

### Abstract:
The sunpy/radiospectra package is the fundamental tool for analyzing radio data from the sun. The data models used in this module are not completely compatible with modern Python data models such as NDCube and xarray. This project will redesign the Spectra class to make it coordinate-based using Astropy WCS, thus

---


## Page 5

providing full interoperability within the SunPy framework. I will also develop a comprehensive background subtraction mechanism and optimize the discovery of metadata using optimized remote header peeking, as I did in my PR.

**Problem Statement:**

Presently, the radiospectra tool has three limitations that must be addressed:

1.  **Non-Coordinate-Aware Slicing:** User requires manual computation of array indices based on time and frequency.
2.  **Inaccurate Metadata Discovery:** Current Fido clients often rely on filename parsing, resulting in missing or incorrect search results (e.g., missing End Times for e-Callisto).
3.  **Fragmented Analysis Tools:** Common tasks like background subtraction lack a standardized, extensible API capable of robustly handling RFI and data gaps through intelligent masking.

**Technical Implementation:**

1.  **Coordinate-Aware Data Structures (NDCube/xarray):**

    *   Implement coordinate-aware data structures using NDCube and xarray to enable automatic slicing based on time and frequency coordinates.
    *   This will allow users to slice data without manually computing array indices, making the tool more user-friendly and less error-prone.

---


## Page 6

The major objective is to replace the current Spectrogram class with a new Spectra object.Data structures that can be used:

i. NDCube: Evaluating its performance in WCS-aware slicing and its integration with SunPy.

ii. Xarray: Evaluating its performance with labeled dimensions and metadata-rich data sets.

<table>
  <tr>
    <td>NDCube(SunPy/Astropy)</td>
    <td>xarray(PyData ecosystem)</td>
  </tr>
  <tr>
    <td>WCS Support as native integration with astropy.wcs, design for astronomy axes.</td>
    <td>It requires rioxarray or xoak for advanced WCS mapping.</td>
  </tr>
  <tr>
    <td>Metadata is stored in .meta dict,and can be lost during some slicing operations.</td>
    <td>Metadata are first class citizens and very persistent.</td>
  </tr>
  <tr>
    <td>Its core design goal is coordinate based slicing.</td>
    <td>Does label-based slicing.</td>
  </tr>
  <tr>
    <td>It fits perfectly into the SunPy ecosystem.</td>
    <td>Considered industry standard for multi-dimensional arrays.</td>
  </tr>
  <tr>
    <td>It has strong, native support for extra_coords and complex array masks for RFI.</td>
    <td>High, uses Numpy masked arrays or NaNs. It is very good for broadcasting operations across gaps.</td>
  </tr>
</table>

---


## Page 7

2. **Implementing the Spectra Object:**

Once the data structure to be used, decision is made with the help of mentor, the next step will be to:

i. Build the object to handle N-dimensional radio data.

ii. Ensuring that it supports slicing based on coordinates.(e.g., slicing by MHz or UTC Time).

iii. To provide a seamless transition path for users moving from the old Spectrogram class.

3. **Background Subtraction & Metadata Hooks:**

i. Analysis Framework: Designing a standard API to handle background subtraction(constant,rolling median, etc.).

ii. Generalized Peeking: Designing a general-purpose ‘post_search_hook’ utility to retrieve FITS headers remotely for all ‘radiospectra’ clients,so that it can achieve 100% accurate search metadata in Fido.

---


## Page 8

# System Architecture Diagrams.
(Just a rough flow diagram for better understanding)

https://github.com/AbhiLohar/portfolio-site/blob/82aba9367c25599f79e050bb10c4390f85b0b4e7/images/Screenshot%202026-03-17%20235455.png

Figure 1: Proposed System Architecture. This design combines the remote header peeking via HTTP range requests and the Astropy WCS engine to encapsulate the data and metadata into a unified and coordinate-aware object called Spectra. This design also accommodates professional-grade analysis tools such as physical coordinate slicing and modular background subtraction.

## GSOC timeline and my availability:

<table>
  <thead>
    <tr>
      <th>Period</th>
      <th>Tasks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>May 8 - May 20<br>(Community Bonding)</td>
      <td>
        1. Connect with mentors and understand expectations for the project.<br>
        2. Revisit radiospectra to understand the scope of porting and do analysis into NDCube and xarray for radio data structure.
      </td>
    </tr>
  </tbody>
</table>

---


## Page 9

<table>
  <tr>
    <td></td>
    <td>
      3. Familiarize with the internals of the core radiospectra code: Spectrogram legacy code, Fido search architecture, WCS coordinate systems.<br>
      4. Discuss and finalize milestones, coding standards, testing strategies,and the new Spectra object structure.
    </td>
  </tr>
  <tr>
    <td>
      May 25 – June 20<br>
      (Coding Phase 1)
    </td>
    <td>
      1.Finalize Data Model: Decide on the underlying structure(NDCube/xarray) based on research.<br>
      2. Core Implementation: Create the initial Spectra object with Astropy WCS integration for time/frequency mapping.<br>
      3. I/O Enhancements: Generalize the “Remote Header Peeking” logic(PR #164) as a utility for all radiospectra clients.<br>
      3. Document code and get early feedback from mentors via notebooks.<br>
      4. Overall, completing this milestone.<br>
      5. Drift Rate Characterization: Use of new coordinate-aware API
    </td>
  </tr>
</table>

---


## Page 10

<table>
  <tr>
    <td></td>
    <td>for calculating frequency drift rate of solar radio bursts. For distinguishing between Type II and Type III solar bursts.</td>
  </tr>
  <tr>
    <td>June 21 - July 12<br>(Midterm Evaluation)</td>
    <td>1. Coordinate Slicing:<br>Complete the implementation of the code for slicing the data by physical coordinates.<br>2. Background Subtraction:<br>Initialize the framework for noise reduction (Constant and Rolling-Median methods).<br>3. Ensure consistency with SunPy standards and Astropy units (MHz, GHz, s).<br>4. Visualization: Add matplotlib recipes for the new Spectra object to handle gappy or irregular data.<br>5. Prepare midterm documentation and submit progress reports for mentor evaluation.</td>
  </tr>
  <tr>
    <td>July 13 - August 16<br>(Coding Phase 2)</td>
    <td>1. Advanced Analysis:<br>Develop energy-dependent variability and frequency-drift analysis modules.<br>2. Cross-Client Support: Add</td>
  </tr>
</table>

---


## Page 11

<table>
  <tr>
    <td></td>
    <td>1.Integrate the optimized Fido search hooks to the Mexico City and Nobeyama clients.<br>3.Performance Optimization: Enhance memory usage and error catching for large FITS files.<br>4.Validation: Apply the new framework to synthetic and actual datasets from the e-Callisto network.<br>5.Continue writing unit tests and update documentation.</td>
  </tr>
  <tr>
    <td>August 17 - August 31</td>
    <td>1.Final performance benchmarking and stress testing of the Spectra object.<br>2.Refactoring: Replace all remaining legacy patterns with the new coordinate-aware implementation.<br>3.Final Deliverables: Deliver clean code, documentation, an "Example Gallery", and user tutorials.<br>4.Write the final GSoC blog post and usage guide for the radiospectra community.</td>
  </tr>
  <tr>
    <td>Post-GSoC</td>
    <td>1.Work on further integrating this tool with other OpenAstronomy packages, e.g., ndcube and astropy.</td>
  </tr>
</table>

---


## Page 12

2. Explore GPU acceleration for heavy spectrogram processing using CuPy.
3. Continue maintenance and planning for the radiospectra package.
4. Participate in the SunPy community as a long-term contributor.

NOTE: My availability will be little less during 4th May to 16th May 2026, due to my University semester end exam, but I am ready to compensate and am happy to work after 17th May 2026. Additionally, more work will be added, and it will be flexible dates. These are just for context :)

Availability:

Since this is a 350-hour project, I plan to dedicate around 5-6 hours each weekday and 9-10 hours on weekends and holidays. As a student, I'll make sure to manage my time well to maintain a healthy balance between academics and GSoC. I'm genuinely excited about the project and fully committed to giving it my best.

GSoC:

---


## Page 13

Have you participated previously in GSoC?

> No, this is my first time applying.

Are you also applying to other projects?

>No.

**Schedule Availability:**

I am completely available, during the contribution period. I am also fully dedicated to give my time to complete this 350h project, also i don't have any planned holidays and travel during the contribution timeline.
