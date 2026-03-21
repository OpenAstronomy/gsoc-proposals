[sunpy] Improving Radiospectra Functionality and Interoperability 
Contributor Information
•	Name: Dhanush Sai Mudari
•	Time-zone: IST (UTC +5:30) 
•	Matrix Handle: @dhanushsai:matrix.org
•	Github Username: dhanushsaimudari 
•	PR link(s):
	sunpy/radiospectra#170 - Documentation: Improved Dev Setup for Windows.
	sunpy/radiospectra#172 - Feature: Added frequency_at_index to GenericSpectrogram.
Background
I am an undergraduate Computer Science student with a Diploma in Mechanical Engineering. This combination has helped me approach problems both from a programming and a practical systems perspective.
Over the past few weeks, I have been exploring the radiospectra codebase and contributing small improvements. Through this, I explored how “GenericSpectrogram” stores data and metadata, and how different spectrogram sources are handled through the factory pattern.
I have experience working with Python and libraries such as NumPy and Matplotlib. While working on radiospectra, I also started learning parts of the scientific Python ecosystem, especially SunPy and Astropy, to better understand how real scientific data is handled.
While experimenting, I tried loading spectrogram data and visualizing it, which helped me understand how time and frequency axes are currently represented and where some limitations exist.
Interest in OpenAstronomy
What I like about OpenAstronomy projects is that they are not just software tools, but are also used in real research.
I am especially interested in how radiospectra sits between scientific data and software design, and how improving its data structures can make analysis more intuitive for researchers.
________________________________________
Project Proposal Application
Proposal Title: [sunpy]  Improving radiospectra functionality and interoperability 
Organisation: SunPy 
Summary:
The radiospectra package provides useful tools for working with solar radio spectrograms, but some parts of its design can be improved to make it more flexible and easier to use.
In this project, I plan to improve the Spectrogram data structure by making it more aware of physical coordinates such as time and frequency. This would allow users to work with data using real-world values instead of only array indices.
This will involve extending how time and frequency axes are stored in GenericSpectrogram, and implementing mapping logic to translate between coordinates and indices.
Another important part of the project is background subtraction. From my understanding, real radio data contains various sources of noise and baseline signals. I plan to implement a few commonly used background subtraction methods (such as median or percentile-based approaches) and design an API that also allows users to provide their own custom methods.
I also plan to improve visualization, especially for cases where data has have gaps or irregular sampling. Finally, I will create example notebooks to demonstrate how these features can be used in practice.
I would also like to explore coordinate-based slicing with tolerance, which allows users to query approximate physical values. This can be useful when working with real observational data where exact alignment is not always possible.
Deliverables
1.	Improved Spectrogram object with coordinate-aware operations
2.	Mapping between time/frequency and array indices
3.	Coordinate-based slicing (including optional tolerance handling)
4.	Background subtraction methods (median, percentile, etc.)
5.	Extensible API for user-defined background subtraction
6.	Improved plotting for irregular or missing data
7.	Example notebooks demonstrating usage
8.	Documentation updates and tests

Description/Timeline
Period	Milestone & Description
Community Bonding (May 1 – May 25)	•	Explore the radiospectra codebase in more detail 
•	Review relevant issues and past discussions 
•	Study related approaches (NDCube, xarray, Astropy WCS) 
•	Discuss and finalize design decisions with mentors 

Week 1-2 (May 26 – June 8)	Spectra Model Design
•	Design structure for coordinate-aware Spectrogram 
•	Decide how time and frequency axes will be stored 
•	Begin extending GenericSpectrogram 

Week 3-4 (June 9 – June 22)	Coordinate Mapping
•	Implement mapping between physical coordinates and indices 
•	Likely use interpolation over time/frequency axes 
•	Ensure bidirectional mapping (coordinate → index and index → coordinate)
•	Handling irregular or non-uniform time/frequency grids may require careful interpolation and validation

Week 5-7 (June 23 – July 12)	Core Operations & First Evaluation
•	Implement slicing using indices and coordinates 
•	Add support for tolerance-based slicing 
•	Provide initial working version for evaluation

Week 8-10 (July 13 – Aug 9)	Background Subtraction
•	Implement median and percentile-based methods 
•	Design a consistent interface for all methods 
•	Add support for user-defined functions (plug-in style) 

Week 11-12 (Aug 10 – Aug 23)	Visualization Improvements
•	Improve plotting for missing or irregular data 
•	Extend plotting mixins where needed 
•	Increase test coverage and refine implementation

Week 13 (Aug 24 – Aug 31)	Final Evaluation
•	Create example notebooks demonstrating workflows 
•	Update documentation and API references 
•	Final cleanup and preparation for merging 

Final Phase (Sept – Nov 11)	Refinement
•	Address feedback from mentors 
•	Handle edge cases and refinements 
•	Support integration into the broader SunPy ecosystem

________________________________________
GSoC
Q: Have you participated previously in GSoC?
A:  No
Are you also applying to other projects?
A:  No
Schedule Availability 
I will be available full-time during the GSoC period and can dedicate around 30–40 hours per week. I do not have any major commitments during this time.
Other comments
While working on my initial contributions, I set up the radiospectra development environment on Windows and worked through some setup and compatibility issues on my own, which helped me become more comfortable navigating the codebase and troubleshooting problems.
Coming from a Mechanical Engineering background, I tend to think about data in terms of real-world systems and limitations, which also influenced how I see background subtraction as preserving meaningful signals rather than just removing noise.
I have chosen to apply only to SunPy for GSoC 2026 so that I can stay fully focused on contributing to radiospectra.

