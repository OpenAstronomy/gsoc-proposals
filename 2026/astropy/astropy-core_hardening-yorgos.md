# Hardening astropy’s core stability - Yorgos Vasilantonakis


## Contributor Information
* **Name:** Yorgos Vasilantonakis
* **Time-zone:** EET (GMT+2)
* **Matrix/slack/IRC Handle:**
* **Github/forge username:** toastmt
* **Blog:**
* **Blog RSS feed:**
* **PR link(s):** #19452 WIP: Standalone C-level tests for 1D convolution (GSoC 2026) (https://github.com/astropy/astropy/pull/19452)

### Background
My name is Yorgos and I'm currently doing a CS conversion degree, building on my previous BSc in Physics. Over the last year I have focused on C and Python. I was drawn to this project because it perfectly bridges my two backgrounds. It allows me to apply my recent CS focus to the mathematical concepts I studied in physics.

### Interest in OpenAstronomy
Coming from a physics background I've worked with free and open-source programs and have come to appreciate all the software that powers modern scientific discovery. As someone who grew up with sci-fi and astronomy magazines and books, I find the prospect of contributing to a major astronomical codebase like Astropy to be incredibly exciting. I think the specific project idea looks interesting, as it alligns with my current interest in C, and it seems like something I could be useful for.

## Project Proposal Application
**Proposal Title:** Hardening astropy’s core stability

**Organisation:** Astropy

### **Summary:**
I am drawn to this project because it sits exactly at the intersection of my skill sets. My primary programming focus is in C, making Astropy's C core an ideal environment for me to contribute. I am also very interested in analyzing performance-critical C code in a real world codebase.

Astropy’s low-level C code is currently tightly coupled with NumPy and the Python runtime. Because the existing test suite evaluates the codebase through pytest, the core mathematical algorithms cannot be tested in isolation. This architectural coupling makes it extremely difficult to verify the memory safety and precision of the native code on its own, and currently prevents the low-level layer from being extracted into a separate, independent package. 

My approach to this project will be to start writing standalone C tests for the core of Astropy, as well as refactoring the C core header files in order to decouple them from Python libraries and make them able to be compiled in isolation. This will both enable the core packages to be tested without any link to the rest of astropy, and also serve as a first step towards the decoupling of the C core into a separate package, enabling astropy itself to become a pure Python package.

### Deliverables
* **1.** Standalone C Tests: A dedicated testing package that verifies the C files independently of Astropy's existing Python test suite.
* **2.** Refactoring of C core headers, to structurally decouple them from NumPy/Python.
* **3.** CI Integration: Automated GitHub Actions workflows that compile and execute the standalone native tests in an isolated environment.
* **4.** Detailed documentation of the test package, and assessment of next steps required for decoupling.

### Description/timeline

|Period|Description|
|------|-----------|
|Community Bonding period| Map all C-functions to be tested in the project, set up the dev environment, assign priorities and testing implementation strategy with mentors.|
| week 1-2 | convolution: Expand the current prototype tests into a full testing package for convolve.c. Focus on edge cases (NaNs, empty arrays, infinity). Decouple C files from Python libraries.|
| week 3-4 | bls: Create a full c test package to validate the mathematical accuracy of the bls.c periodogram core.|
| week 5-10 | wcs: Work to create tests for the c files in the world coordinate system folder.|
| week 11-12 | Handle final bug fixes, CI/CD test integration, write documentation. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No
### Are you also applying to other projects?
No
### Schedule availability
I'll be fully available during the time of the programme, no holidays planned


## Other comments
I have spent some time trying to understand the logic in some of the c files in the astropy codebase. To better understand the implementation I concentrated on the convolve.c file which implements convolution algorithms for 1D, 2D and 3D. I compiled the file and developed a small standalone testing file that interacts directly with the convolve1d_c function at the C level, and which contains a few simple tests for the 1D case. This has helped me understand the 1D convolution algorithm well.
