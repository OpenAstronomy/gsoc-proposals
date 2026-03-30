# Hardening Astropy's Core Stability

## Contributor Information
* **Name:** Adwait Amlekar
* **Time-zone:** IST (UTC/GMT + 5:30)
* **Matrix/slack/IRC Handle:** N/A 
* **Github/forge username:** adwait-ops
* **Blog:** https://medium.com/@adwait7003
* **Blog RSS feed:** https://medium.com/feed/@adwait7003
* **PR link(s):**
  * .

### Background
I am Adwait Amlekar, a first-year Mechanical Engineering student at BITS Pilani (Hyderabad Campus). By day, I study physical systems; by night, I am a computational problem solver. I prefer deconstructing systems to understand their core logic rather than just interacting with the surface level; a mindset that drives my interest in software infrastructure.  

While my degree focuses on the physical world, my technical toolkit is built for the digital one. I have a strong foundation in Python and experience with libraries like NumPy, Astropy, PyCBC, and GWpy, alongside tools like MATLAB and LaTeX. My academic background is rooted in discipline, having achieved 96% in Class X and 92% in Class XII. At BITS, I am an active research team member for AdAstra (Astronomy Club) and Spectrum (Physics Association).  

Beyond standard library applications, I frequently develop custom environments to test physical theories. For instance, in one of my projects, I formulated a non-conservative force law using p5.js. By coupling a Yukawa interaction with a chiral component, I simulated two-body dynamics via Velocity Verlet integration to investigate stability and angular momentum transfer. This represents my broader approach: blending theoretical curiosity with rigorous, functional code.  


### Interest in OpenAstronomy
OpenAstronomy feels like a natural fit for me because it sits at the intersection of my primary interests: physics, coding and problem solving. As part of my independent and club projects, I have already engaged with the scientific questions that OpenAstronomy tools aim to solve.  

What draws me most is the opportunity to contribute to a codebase that I personally rely for research. For instance, my work on Exoplanet Transit Analysis for TRAPPIST-1e and developing a Gravitational Wave Signal Processing Pipeline using PyCBC and GWpy has shown me how crucial library stability is for accurate working. I enjoy the rigorous process of refining systems and doing so within a scientific context makes the work compelling.  

Hardening Astropy's Core Stability is especially attractive because it required the same systems-level thinking that needs to be applied in complex physical problems. I am excited by the challenge of working on real-world codebases where improvements directly impact the research community.  
While I am comfortable working independently, I value the Open Source philosophy where ideas are tested and discussed. I would love to learn from the expertise of the OpenAstronomy team. 


## Project Proposal Application
**Proposal Title:** Hardening Astropy's Core Stability

**Organisation:** OpenAstronomy

### **Summary:**

This project interests my because it focuses on the internal structure of a complex mixed-language codebase. From what I've gathered, the goal is to strengthen Astropy's foundations by building independent, robust testing systems for its low-level components. I'm particularly drawn to the challenge of working where performance and maintainability intersect; the fact that a relatively small amount of low-level C code can dictate the complexity of the entire build process makes this a deeply meaningful problem to solve.  

I'm choosing this project because I enjoy the process of deconstructing existing systems to see how they can be made more structurally sound. I believe I can take this on because I’ve spent the last year moving beyond just using libraries to actually building my own environments from scratch. Whether it was implementing Velocity Verlet integration for a custom force law or setting up signal processing pipelines for gravitational wave data, I’ve had to get comfortable with the unseen parts of code-handling dependencies, debugging linking errors, and ensuring numerical accuracy. My background in Mechanical Engineering and my experience with computational physics have shaped a very iterative, structured approach to problem-solving. I'm comfortable navigating messy code, experimenting with build configurations, and refining solutions step-by-step. For me, this is an opportunity to contribute to a scientific ecosystem I already use, while getting under the hood to understand codebase design and maintainability at scale. 



### Deliverables
**1. Standalone Build Scripts:** A set of localized pyproject.toml or setup.py configurations that allow the C-extensions to be compiled and linked independently of the full library (like astropy.convolution or astropy.stats) 

**2. Direct C/Cython Test Suite:** A collection of pytest scripts designed to hit the low-level functions directly. This ensures that the core math remains accurate even when isolated. 

**3. CI Workflow Integration:** A functional GitHub Actions .yml file that runs these isolated tests. The goal is to provide a fast track CI option that gives developers feedback on core changes while also saving time.  

**4. Developer Guide:** A document explaining the dependency structure of the isolated modules and how future contributors can add standalone tests for other subpackages. 

**5. Compatibility Report:** A technical summary of how these isolated modules behave across different Python versions, including notes on any experimental work done with Meson or the Python Limited API. 


### Description/timeline

|Period|Description|
|---|---|
|Community Bonding period| Introduced myself in the OpenAstronomy community |
| Week 1 (May 20th - 26th) | Audit dependencies in modules (like astropy.convolution or astropy.stats to establish a standalone local build |
| Week 2 (May 27th - June 2nd) | Develop a pytest framework for direct C/Cython testing without a full library import |
| Week 3 (June 4th - 10th) | Scale the isolation template to other subpackages and integrate GitHub actions for CI |
| Week 4 (June 11th - 17th) | Solve linking issues for complex modules with external C dependencies (like wcs) |
| Week 5 (June 18th - 24th) | Finalize a helper script for low-level testing and complete the first evaluation | 
| Week 6 (June 25th - July 1st) | Extend the testing script to core modules (like timeseries) that have high usage | 
| Week 7 (July 2nd - 8th) | Perform testing across Python version to ensure stability, and explore Meson or Python Limited API |
| Week 8 (July 9th - 15th) | Finish final documentation and reports, evaluate a potential Rust-based prototype | 



## GSoC

### Have you participated previously in GSoC? When? With which project?  
No, this is my first time applying for GSoC.

### Are you also applying to other projects?  
I am not applying to any other projects via GSoC. 

### Schedule availability
I will be dedicated to this project full time (approx 25-30 hours a week) from May 19th through July 19th. My schedule is consistent, with the exception of one personal holiday on June 3rd. I intend to maintain a standard Monday-Friday or Monday-Saturday work week, keeping Sunday for rest and project review. 

## Other comments
