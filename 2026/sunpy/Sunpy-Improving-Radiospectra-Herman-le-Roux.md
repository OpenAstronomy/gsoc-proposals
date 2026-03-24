# [SunPy/radiospectra] Improving Radiospectra Functionality and Interoperability 

## Contributor Information
* **Name:** Herman le Roux
* **Time-zone:** Irish Standard Time (UTC+1)
* **Matrix/slack/IRC Handle:** @hermanlrx:matrix.org
* **Github/forge username:** [@hermanlrx](https://github.com/Hermanlrx)
* **Blog:** [hermanlrx.github.io](https://hermanlrx.github.io/)
* **Blog RSS feed:** [RSS feed](https://hermanlrx.github.io/index.xml)
* **PR link(s):**
  * PR Add support for SOLO L3 [#148](https://github.com/sunpy/radiospectra/pull/148) (merged)
  * PR Updates SOLO RPW examples [#173](https://github.com/sunpy/radiospectra/pull/173) (review required)

### Background
I am currently a 2nd-year PhD student affiliated with the Dublin Institute for Advanced Studies in the Solar Physics and Space Weather research group and the Technological University of the Shannon: Midlands. My current research focuses on creating tools for tracking and classifying low-frequency solar radio emissions using I-LOFAR data. 

Prior to my PhD program, I completed a M\.Sc\. in Computer Science at the North-West University in South Africa. The focus of my master's research project was creating machine learning models to classify spectrograms containing solar radio bursts in e-CALLISTO data. During the course of my project, a previous version of `SunPy/radiospectra` was partly used to create the final dataset. 

Throughout the course of my research projects and undergraduate studies, I have gained experience with libraries such as `numpy/scipy/pandas/matplotlib/astropy` and scalable object-orientated design patterns for software development.

Since the start of my PhD I have made use of some of the functionality included in `SunPy/radiospectra`. There are, however, several generic radio spectrogram operations that would simplify my workflow (e.g. Issue [#129](https://github.com/sunpy/radiospectra/issues/129)). I recently contributed to the `SunPy/radiospectra` package through pull requests ([#148](https://github.com/sunpy/radiospectra/pull/148) and [#173](https://github.com/sunpy/radiospectra/pull/173)) implementing data ingesting code for new Level 3 SOLO data products. I hope to contribute to the package throughout my PhD and thereafter.



### Interest in OpenAstronomy
OpenAstronomy has endeavoured to create a community where not only open source software is prioritised, but also the sharing of resources and ideas. This aligns with what I believe researchers in academia should be aiming to achieve. Moreover, the opportunity to learn from the mentors at SunPy and the OpenAstronomy community is exciting and invaluable for as a prospective researcher and software developer. It also creates an ideal platform for me to start contributing to open-source projects in solar physics. 


## Project Proposal Application
**Proposal Title:** Improving radiospectra’s Functionality and Interoperability

**Organisation:** SunPy

### **Summary:**
Radiospectra currently enables users to plot radio spectra from several solar radio telescopes and receivers. Expanding its functionality would significantly enhance researchers' ability to collect and analyse available solar radio data. During my PhD, I will need to implement functionalities similar to those proposed for the improved radiospectra package that could benefit the broader community. Along with my undergraduate degrees in Information Technology and Computer Science and my hands-on experience working with e-CALLISTO, I-LOFAR, and SOLO data, I am well-suited and eager to contribute to a package the solar radio community will benefit from.


### Deliverables
**1.** A redesigned Spectra object that:
 *  integrates cleanly with existing SunPy and Astropy data structures (`u.Quantities`), 
```python
spec.crop(freq_range=(20*u.MHz, 80*u.MHz))
```
 *  enables operations such as cropping along the frequency or time axis using array indices or physical coordinates (frequency bands or time ranges),
 *  preserves metadata through operations.

**2.** Background subtraction methods:
 *  built-in support for common background subtraction techniques,
 ```python
 spec.subtract_background(method="median")
 ```
 *  extensible support for tailored methods. 

**3.** Enhanced visualisations that:
 * improve current plotting to address irregular sampling and gapped data,
 * update docstrings and test coverage,
 * expand example gallery.
  
**4.** Stretch goal: Add type hints incrementally
Considering the extent of the refactoring required I would like to suggest type hinting as a stretch goal throughout the project.
```python
def crop(...) -> Spectra:
```


### Description/Timeline
Important to note that the creation of tests, docstrings and type hints should ideally be done alongside core deliverables to ensure it doesn't lead to large loads at the end of the project.

|Period|Description|
|------|-----------|
|Community Bonding Period (May 1-24)| Meet with mentors, set up development environment, review radiospectra codebase and open issues. Survey existing approaches (NDCube, xarray) and finalise design choices for the new Spectra data model. Finalise implementation plan.|
|Week 1 (May 25-31)| **Deliverable 1 (Part 1)**: Begin implementing redesigned Spectra object. Establish WCS-like interface for mapping physical coordinates (time, frequency) to array indices.|
|Week 2 (June 1-7)| **Deliverable 1 (Part 1 & Part 2)**: Coninue work on Spectra object. Begin implementation of coordinate-aware slicing (by indices and physical coordinates).|
|Week 3 (June 8-14)| **Deliverable 1 (Part 2 & Part 3)**:  Finalise Spectra object. Add basic cropping operations along time and frequency axes. Expand test coverage.|
|Week 4 (June 15-21)| **Deliverable 2 (Part 1)**: Begin background subtraction framework. Implement first common method. Define interface for custom methods.|
|Week 5 (June 22-23)| **Deliverable 2 (Part 2)**: Add second background subtraction method. Prepare midterm progress report and documentation of work completed. Coordinate midterm evaluation timeline with mentor before holiday.|
|**Holiday Break (June 24 - July 9)**| **Personal leave - coordinated with mentor for midterm evaluation**|
|Week 6 (July 10-16)| **Submit midterm evaluation (July 10).** Review mentor feedback and adjust timeline if needed. **Deliverable 2 (Part 3)**: Complete additional built-in background subtraction methods. Finalise extensible interface with examples.|
|Week 7 (July 17-23)| **Deliverable 3 (Part 1)**: Begin visualisation improvements. Update existing plotting functions to use new Spectra object.|
|Week 8 (July 24-30)| **Deliverable 3 (Part 2)**: Continue visualisation enhancements. Address irregular sampling and gapped data rendering. Implement sensible defaults with customisation options.|
|Week 9 (July 31 - Aug 6)| **Deliverable 3 (Part 3)**: Create example gallery demonstrating coordinate slicing, background subtraction workflows, and improved plotting of irregular data.|
|Week 10 (Aug 7-13)| **Deliverable 3 (Part 4)**: Expand example gallery with real-world use cases. Complete docstring updates and expand test coverage across all deliverables.|
|Week 11 (Aug 14-20)| Integration testing with real observational data. Address outstanding issues and bugs. **Deliverable 4 (Stretch)**: Add type hints to core functions if time permits. Prepare final documentation.|
|Week 12 (Aug 21-24)| Final code cleanup and refactoring. Complete API documentation. Merge upstream and address reviewer feedback. **Submit final work product by August 24**.|

## GSoC

### Have you participated previously in GSoC? When? With which project?
No
### Are you also applying to other projects?
No
### Schedule availability
The scheduled leave during the end of June and start of July period is contingent on the agreement of mentors. Should they deem necessary, then I will be available throughout the course of the GSoC project.

## Other comments
During the coding phase, it will be essential to communicate with the mentors. While regular contact will be made on the SunPy Matrix channels, I propose that we meet weekly and that I provide async updates throughout the week as progress is made. 

Furthermore, the scope of deliverable 1 can change significantly depending on final decisions. Early prototyping, draft pull requests and iterative design discussions will ensure issues are efficiently addressed. Finally, while it is not necessarily required by the proposed application, it will be beneficial to discuss backwards compatibility of any changes made.  
