# Improving radiospectra's functionality and interoperability

## Contributor Information

* **Name:** Maryam Gohar
* **Time-zone:** (UTC+02:00) Cairo
* **slack:** https://reltrans-workspace.slack.com/team/U0APEL1008M
* **Matrix:** @mgoher:matrix.org
* **Github/forge username:** starAwesome123
* **Blog:** https://starawesome123.github.io/gsoc-blog/
* **Blog RSS feed:** https://github.com/starAwesome123/gsoc-blog/commits/main.atom
* **PR link(s):** https://github.com/sunpy/radiospectra/pull/190

### Background

I am a navigation science and space technology graduate with experience in orbital mechanics, space missions, programming, space subsystems, the space environment, and embedded systems. During my college years, I participated in the Astronomy Club, was motivated to enter engineering competitions, and became a member of PAN Africa, which includes several countries across the continent and is interested in space projects and contributes to space research, where I contributed to asteroid search (Astrometrica), contributed to exoplanet search, and wrote a research paper on double stars.

After graduation, I took a six-month internship at the Egyptian space agency, during which I developed a Python-based machine learning model to predict satellite collision risks using Conjunction Data Messages (CDM). This experience strengthened my skills in data analysis, debugging, and working with real-world scientific datasets.

Recently, I familiarized myself with SunPy, how to fetch data by Fido, choose a specific instrument, and visualize LYRA solar irradiance data. I faced some issues with this experience, solved them, and decided to share my experience to help others solve their issues faster and easier. I am now motivated to contribute to open-source scientific software, particularly in projects like radiospectra, where my background in space systems and growing experience with Python-based data analysis can be directly applied.

### Interest in OpenAstronomy

The main reason I want to be a part of OpenAstronomy is that I respect its efforts to bring together multiple scientific projects under one collaborative and open environment, making it easier for contributors to learn, share knowledge, and build tools that benefit the wider scientific community — providing open-source code that future scientists and students could use. I want to be part of this community, and I want to contribute to my field and my experience not only by writing code, but also by improving documentation and sharing practical experiences, especially for beginners who may face difficulties when working with scientific Python tools. With my recent experience with SunPy, I saw how challenging it can be to get started, and I would like to help make it clearer for others. I am interested in making space for everyone to be a part of a place that loves space as much as I do, to develop my technical skills, and contribute improvements to the radiospectra project.

## Project Proposal Application

**Proposal Title:** Improving radiospectra's functionality and interoperability

**Organisation:** SunPy

### Summary

This project focuses on improving the usability and flexibility of the radiospectra library within the SunPy ecosystem by making it easier to work with real-world data. During my initial experience with SunPy, I worked on fetching and visualizing LYRA data and faced several practical challenges related to data handling, dependencies, and usability. This motivated me to explore how the library could be improved for new users and for real data applications.

One of the main goals of this project is to extend radiospectra to better support real data sources such as Software Defined Radio (SDR), allowing users to work not only with pre-existing datasets but also with live or recorded observations. In addition, the project aims to improve handling of missing or incomplete data, enhance compatibility with different instruments, and make the analysis workflow clearer and more accessible. and exploritory solution to use ML to handle the missing data

By focusing on both functionality and usability, this project will help make radiospectra more practical for scientific analysis and more approachable for new contributors. I could handle this project because I have my fair share of working with space-related research during college and even after graduation. I analyzed the data for the projects I worked on, I have experience working with diverse teams toward the same goal, and I have experience on different types of Python projects to completion — whether simulation, machine learning, IoT, GUI, or APIs — so I can anticipate challenges, and challenges make me more determined to solve problems and reach my goal.

### Deliverables

**1. SDR Data Integration (Prototype)**
- Investigate how SDR data can be formatted and adapted to be compatible with radiospectra
- Develop a prototype pipeline to read and visualize SDR-based radio data

**2. Improved Data Handling**
- Implement basic handling for missing or incomplete data (e.g., gaps in spectrograms)
- Improve plotting behavior for clearer visualization of such cases

**3. Support for Additional Instruments (Exploration + Initial Support)**
- Analyze how new instruments can be integrated into radiospectra
- Add support for at least one additional data source or improve an existing one

**4. Documentation and Examples**
- Write beginner-friendly documentation based on real issues I faced
- Provide example workflows (e.g., fetching, loading, plotting, analyzing data)

**5. Testing and Stability**
- Add tests for new features
- Ensure compatibility with existing radiospectra and SunPy functionality

### Description/timeline

| Period | Description |
|--------|-------------|
| Community Bonding Period | Get familiar with the radiospectra codebase and its integration with SunPy. Set up the development environment and run existing examples. Discuss project scope with mentors, especially ideas related to SDR data and missing data handling. |
| Weeks 1–3 | Work on documentation improvements and beginner-friendly examples. Identify common issues faced when using radiospectra (based on my own experience). Explore the structure of spectrogram data and plotting functions. |
| Weeks 4–6 | Implement improvements for handling missing or incomplete data (e.g., gaps in spectrograms). Improve visualization to clearly represent missing data. Begin exploring classical methods (such as interpolation) and consider simple ML approaches for reconstruction. |
| Weeks 7–9 | Start developing a prototype pipeline for SDR data integration. Test reading and visualizing SDR-based data. Continue exploring ML-based approaches for missing data (if feasible) and compare with classical methods. Refine implementations based on feedback. |
| Weeks 10–12 | Work on adding support for an additional instrument or improving an existing one. Finalize SDR integration prototype. Improve code quality, testing, and documentation. Add clear usage examples for users. |
| Final Evaluation | Ensure all features are stable and documented. Finalize contributions and clean up code. Prepare final report and submission. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No.
### Are you also applying to other projects?
Yes, LibreCube.
### Schedule availability
have flexible hours.