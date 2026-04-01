# Hardening Astropy's Core Stability

## Contributor Information
* **Name:** Adwait Amlekar
* **Time-zone:** IST (UTC/GMT + 5:30)
* **Matrix/slack/IRC Handle:** N/A 
* **Github/forge username:** adwait-ops
* **Blog:** https://medium.com/@adwait7003
* **Blog RSS feed:** https://medium.com/feed/@adwait7003


### Background
I am Adwait Amlekar, a first-year Mechanical Engineering student at BITS Pilani (Hyderabad Campus). By day, I study physical systems; by night, I am a computational problem solver. I prefer deconstructing systems to understand their core logic rather than just interacting with the surface level; a mindset that drives my interest in software infrastructure.  

While my degree focuses on the physical world, my technical toolkit is built for the digital one. I have a strong foundation in Python and experience with libraries like NumPy, Astropy, PyCBC, and GWpy, alongside tools like MATLAB and LaTeX. My academic background is rooted in discipline, having achieved 96% in 10th Grade (Sophomore year) and 92% in 12th Grade (Senior year). At BITS, I am an active research team member for AdAstra (Astronomy Club) and Spectrum (Physics Association).  

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

This project interests me because it focuses on the internal structure of a complex mixed-language codebase. From what I've gathered, the goal is to strengthen Astropy's foundations by building independent, robust testing systems for its low-level components. I'm particularly drawn to the challenge of working where performance and maintainability intersect; the fact that a relatively small amount of low-level C code can dictate the complexity of the entire build process makes this a deeply meaningful problem to solve.  

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
| Week 1 (May 20th - 26th) | - Map out dependencies to find the modules with fewest external python dependencies. (likely start with .convolution or .stats) <br>- Look at the current build files to figure out if and how to separate these without having to install the whole library <br>- Goal is to get a local build running where i compile only THAT module and not entire astropy |
| Week 2 (May 27th - June 2nd) | - Set up a pytest structure to send data straight to the C/Cython code<br>- Goal is to get these tests to pass without importing astropy (because if i get that, i’ll have a template for the rest of the project) |
| Week 3 (June 4th - 10th) | - Apply the week 2 template to other subpackages<br>- Set up a simple github actions workflow to run new tests <br>- Goal is to show how much time can be saved to test just one module vs the whole library |
| Week 4 (June 11th - 17th) | - Handling modules with messier builds (like wcs) that have external dependencies (mostly C, like wcs has wcslib)<br>- Lot of trial and error to solve the linking and header issues when building such modules in a standalone environment |
| Week 5 (June 18th - 24th) | - General cleaning up of code, making a helper script to add low-level tests for other modules<br>- Complete Evaluation 1 | 
| Week 6 (June 25th - July 1st) | - Push the testing pattern to remaining core modules (like timeseries)<br>- Goal is to harden the most crucial and high traffic C functions that astropy relies on, even if 100% line coverage isn’t possible | 
| Week 7 (July 2nd - 8th) | - Ensure tests are working, final checks<br>- Spend some time on extra goals - try to build one of the hardened modules using Meson or figure out if we can use Python Limited API<br>- Making sure the code doesn’t break when new versions of python come out |
| Week 8 (July 9th - 15th) | - Making the final report, document my work and findings<br>- If time permits - a small Rust-based prototype just to see how it fits into the new system | 

## GSoC

### Have you participated previously in GSoC? When? With which project?  
No, this is my first time applying for GSoC.

### Are you also applying to other projects?  
I am not applying to any other projects via GSoC. 

### Schedule availability
I will be dedicated to this project full time (approx 25-30 hours a week) from May 19th through July 19th. My schedule is consistent, with the exception of one personal holiday on June 3rd. I intend to maintain a standard Monday-Friday or Monday-Saturday work week, keeping Sunday for rest and project review. 

## Other comments
While I am new to formal open-source contributions, I intend to treat GSoC 2026 as my entry point into the community. I see this project not just as a summer task, but as an opportunity to learn the rigorous standards of the Astropy codebase and contribute to a foundation that supports the entire global astronomy community. I am ready to dive into the collaborative process of code reviews, documentation and the iterative feedback that makes open source so resilient. 
