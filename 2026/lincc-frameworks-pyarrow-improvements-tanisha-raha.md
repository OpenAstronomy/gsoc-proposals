
# [lincc-frameworks] PyArrow improvements for astronomy

## Contributor Information
* **Name:** Tanisha Raha
* **Time-zone:** IST (UTC+5:30)
* **Matrix/slack/IRC Handle:** tanisha-raha
* **Github/forge username:** tanisha-raha
* **PR link(s):**
  * . https://github.com/lincc-frameworks/nested-pandas/pull/474
  * .

### Background
I am a 3rd year university student from Mumbai, India, pursuing an undergraduate degree in Computer Science.I have about 2 years of experience with Python and C++ and have worked on data structures, algorithms, data analysis and machine learning. Recently, I completed the Amazon ML Summer School, which gave me hands on exposure to machine learning concepts and applied them to real world problems, this experience deepened my interest in building software at the intersection of data, performance, and science.
I also recently earned a global professional certification as a Linux Specialist in Containerization, which reflects my interest in understanding how software systems are built, deployed, and optimized at a deeper level.
I am comfortable with Git and GitHub, and I recently made my first open source contribution by submitting PR #474 to the nested pandas repository, which involved exporting a function to the top level package and adding documentation examples. I am genuinely excited about contributing to open source tools that power real scientific research.


### Interest in OpenAstronomy
Growing up, I was genuinely fascinated by astronomy, the kind of curiosity that makes you stay up reading about black holes and galaxies. That passion never really left me, and when I discovered OpenAstronomy, it felt like a rare opportunity to combine that childhood interest with the software skills I've been building. Contributing to tools that real astronomers use every day would be meaningful to me in a way that goes beyond a resume line, it would be my first open source contribution, and I couldn't imagine a better place for it to be.
Beyond the personal connection, I'm drawn to OpenAstronomy because of how seriously it takes software quality and scientific impact. The nested pandas project specifically caught my attention because it solves a real performance problem, handling large nested astronomical datasets efficiently using PyArrow, which connects directly to my interest in machine learning and data intensive systems from my Amazon ML Summer School experience. I want to work somewhere my code actually matters, and OpenAstronomy is exactly that.



## Project Proposal Application
**Proposal Title:** 
PyArrow improvements for astronomy

**Organisation:**
OpenAstronomy / lincc-frameworks

### **Summary:**
The PyArrow improvements project attracted me because it sits at the intersection of performance engineering and scientific computing, two areas I genuinely care about. Modern astronomical surveys generate billions of data points, and the fact that a missing compute kernel or an unparallelized Parquet reader is slowing down scientists feels like exactly the kind of problem worth solving.
I believe I can contribute meaningfully for several concrete reasons. I have already explored the nested-pandas codebase in depth, I identified issue #432, understood what was missing, and submitted a working PR (#474) that exports from_pyarrow as a top level importable function with a docstring example. A core maintainer (delucchi-cmu) has already reviewed my code, left detailed feedback, and the benchmarks show my changes introduce no performance regressions. This tells me I understand the codebase well enough to work in it productively.
Beyond that, my Linux containerization certification reflects a comfort with systems level thinking and professional software environments, which matters for a project that requires building Apache Arrow from source and working in its C++ internals.
I am motivated, already familiar with the contribution workflow, and committed to spending the summer making nested pandas and Apache Arrow faster and more capable for the scientific community that depends on them.



### Deliverables
**1.** Implement missing compute kernels for nested array types in Apache Arrow

**2.** Enable sub-column selection for list-struct types in the Parquet reader

**3.** Improve multi-threaded read performance for nested structures

**4.** Add comprehensive tests, documentation, and upstream PRs to Apache Arrow


### Description/timeline
*Break your project in blocks, what do you expect you will do each week?*
|Period|Description|
|---|---|
| Community Bonding (May) | Set up Arrow dev environment, study C++ codebase, review linked issues, discuss plan with mentors |
| Week 1-2 (June 2-13) | Study Arrow compute kernel framework, implement first missing kernel |
| Week 3-4 (June 14-27) | Write tests and benchmarks, submit first PR to Apache Arrow |
| Week 5-6 (June 28 - July 11) | Continue broadcasting implementation, investigate Parquet reader |
| Week 7 (July 12-18) | Midterm evaluation, one kernel implemented, tested, PR submitted |
| Week 8-9 (July 19 - Aug 1) | Implement sub column selection for list-struct Parquet reader |
| Week 10-11 (Aug 2-15) | Multi threaded read improvements, benchmarks |
| Week 12 (Aug 16-25) | Final documentation, address review feedback, ensure PRs ready |
| Final evaluation (Aug 25) | All improvements implemented, documented, PRs submitted upstream |


## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time applying to GSoC.

### Are you also applying to other projects?
No

### Schedule availability
I am available full time during the GSoC coding period with no major holidays or exams planned. I can commit approximately more than 35 hours per week.

