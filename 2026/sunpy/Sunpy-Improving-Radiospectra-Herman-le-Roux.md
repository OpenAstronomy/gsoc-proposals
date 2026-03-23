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
I am currently a 2nd year PhD student affiliated with the Dublin Institute for Advanced Studies Solar Physics and Space Weather research group and Technological University of the Shannon: Midlands. My current research focus is creating tools for tracking and classifying low frequency solar radio emissions using I-LOFAR data. 

Prior to my PhD program, I completed a M\.Sc\. in Computer Science at the North-West University in South Africa. The focus of my masters research project was creating machine learning models to classify spectrograms containing solar radio bursts in e-CALLISTO data. During the course of my project, a previous version of `SunPy/radiospectra` was partly used to create the final dataset. 

Throughout the course of my research projects and undergraduate experiences I have gained experience with libraries such as `numpy/scipy/pandas/matplotlib/astropy` and scalable object orientated design patterns for software.

Since I have started my PhD I have made use of some of the functionality included in `SunPy/radiospectra`, but there are several generic radio spectrogram operations that would simplify my workflow significantly (e.g. Issue [#129](https://github.com/sunpy/radiospectra/issues/129)). I recently contributed to the `SunPy/radiospectra` package through pull requests([#148](https://github.com/sunpy/radiospectra/pull/148) and [#173](https://github.com/sunpy/radiospectra/pull/173)) adding small data ingesting code for new Level 3 SOLO data products and hope to contribute to the package throughout my PhD and hopefully thereafter.



### Interest in OpenAstronomy
OpenAstronomy's has endeavoured to create a community where not only open source software is prioritised but also the sharing of resources and ideas. This aligns with what I believe researchers and academia should be aiming to achieve. Moreover, the opportunity to learn from the mentors at SunPy and the OpenAstronomy community is exciting and invaluable for a prospective researcher and software developer. It also creates an ideal platform for me to start contributing to open source projects in the exciting field of astronomy and astrophysics. 


## Project Proposal Application
**Proposal Title:** Improving radiospectra’s Functionality and Interoperability

**Organisation:** SunPy

### **Summary:**
Radiospectra currently enables users to plot radio spectra from several solar radio telescopes and receivers. Expanding its functionality would significantly enhance researchers' ability to collect and analyze available solar radio data. During my PhD, I will need to implemented functionalities similar to those proposed for the improved radiospectra functionality that could benefit the broader community if developed as an open-source package. Combined with my undergraduate degrees in Information Technology and Computer Science and my hands-on experience working with e-CALLISTO, I-LOFAR, and SOLO data, I am well suited and eager to contribute to a package the solar radio community will benefit from.


### Deliverables
**1.** A redesigned Spectra object that:
 *  integrates cleanly with existing SunPy and Astropy data structures (`TimeRange` / `u.Quantities`) 
 *  enables operations such as cropping along the frequency or time axis using array indices or physical coordinates (frequency bands or time ranges).

**2.** Built-in support for common background subtraction techniques and extensible support for tailored methods. 

**3.** Enhanced visualisations that:
 * improve current plotting to address irregular sampling and gapped data
 * update docstrings and test coverage
 * expand example gallery.
  
**4.** Stretch goal: Add type hints
Considering the extent of the refactoring required I would like to suggest type hinting as a stretch goal through out the project.


### Description/timeline
*Break your project in blocks, what do you expect you will do each week?*

|Period|Description|
|------|-----------|
|Community Bonding period| ... |
| week 1 (dates) | ...|
| ... | ...|
| week n | ... |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No
### Are you also applying to other projects?
No
### Schedule availability
I will be available throughout the course of the GSoC period 


## Other comments
