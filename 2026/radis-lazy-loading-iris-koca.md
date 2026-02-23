# Application Title
Modernizing Spectroscopic Data Processing in RADIS With High-Performance Lazy-Loading Integration

## Contributor Information
* **Name: İris Koca** 
* **Time-zone: UTC+3 (Istanbul, Turkey)**
* **Matrix/slack/IRC Handle: İris (Slack)**
* **Github/forge username: archiristo**
* **Blog: mathcs.online** 
* **PR link(s):**
  * https://github.com/radis/radis/pull/935

### Background
I am a student of Artificial Intelligence Engineering with a strong focus on high-performance computing and its applications in astrophysics. Currently, I am collaborating with the European Southern Observatory on spectroscopic dataset analysis, where I handle large-scale data workflows.
My experience includes a project supported by the Scientific and Technological Research Council of Turkey (TÜBİTAK), where I developed predictive environmental models. This role required rigorous performance profiling and memory optimization, skills that are directly transferable to the RADIS database engine migration. I have a strong command of Python and modern DataFrame ecosystems (like Pandas, Polars). Furthermore, my current academic coursework in Ordinary Differential Equations and computational logic provides the mathematical foundation necessary to understand and optimize the physics-driven data structures within RADIS.

### Interest in OpenAstronomy
OpenAstronomy represents the perfect synergy between my two greatest interests, rigorous software engineering and the mysteries of the universe.
I believe that scientific discovery should not be bottlenecked by unmaintained dependencies or inefficient I/O. 
I want to contribute to RADIS because it is at the forefront of spectral code performance, and I want to ensure its infrastructure remains sustainable and scalable for the next generation of 50GB+ molecular databases.


## Project Proposal Application
**Proposal Title: Integrate a modern lazy-loading alternative (Polars/DuckDB) for large-scale spectroscopic database processing.**

**Organisation: RADIS (under OpenAstronomy)**

### **Summary:**
The current RADIS infrastructure relies on Vaex for out-of-core processing of large HITEMP/HITRAN databases. 
However, Vaex's unmaintained status poses a significant sustainability risk. 
This project aims to replace Vaex with a modern, high-performance alternative, primarily Polars or DuckDB.
The goal is not just a 1:1 replacement, but an optimization of the entire I/O pipeline. 
By leveraging Lazy Evaluation and Memory Mapping, I plan to reduce the 3+ hour parsing times for 50GB+ files (like HITEMP CO2) and ensure the code remains robust and maintainable. 
This project is attractive to me because it challenges both my "Data Engineering" skills and my "Physics-Guided AI" intuition.



### Deliverables
**1. Comprehensive Benchmarking Suite: A systematic performance evaluation comparing Polars, DuckDB, and Dask against the current Vaex implementation, focusing on memory peaks and lazy-loading latency.**

**2. Core Engine Integration: Refactoring radis.json and the internal database loading logic to support DATAFRAME_ENGINE selection, with Polars as the primary modern alternative.**

**3. Optimized I/O Pipeline: Implementation of lazy-loading for HITEMP CO2/H2O databases and exploring Parquet as a more efficient storage alternative to HDF5.**

**4. Documentation & API Migration Guide: A detailed guide for contributors on how the new engine handles out-of-core data and performance metrics (before/after).**


### Description/timeline
*Break your project in blocks, what do you expect you will do each week?*

|Period|Description|
|------|-----------|
|Community Bonding period| Engaging with the RADIS community on Slack, setting up the dev environment, and identifying the exact I/O bottlenecks in current HDF5 parsing. |
| week 1-2| Creating a reproducible benchmark report using 50GB+ synthetic and HITEMP data.|
| week 3-4 | Implementing the basic Polars/DuckDB engine choice in radis.json.|
| week 5 (Evaluation 1)| Finalizing the engine selection based on benchmarks and mentor feedback. |
| week 6-8 | Transitioning the core spectroscopic loading functions and ensuring backward compatibility with existing files.|
| week 9-10 |Implementing lazy-loading specifically for CO2/H2O HITEMP databases. |
| week 11 (Evaluation 2)| Unit testing, integration testing with production-ready spectral calculations. |
| week 12-13 | Exploring Parquet integration and documenting the migration strategy. |
| Final Week | Merging to main, finalizing CI/CD tests. |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this would be my first GSoC participation.

### Are you also applying to other projects?
Yes.

### Schedule availability
I am available during the whole GSoC coding period. I am fully committed to the 350-hour project size.


## Other comments
I am highly motivated to make RADIS the fastest and most sustainable spectral code in the world. 
My background in AI Engineering allows me to see this project not just as a refactoring task, 
but as a crucial step towards "Physics-Informed" data processing at scale.
