# PyArrow Improvements for Astronomy

## Contributor Information
* **Name:** Omar Mohammed
* **Time-zone:** GMT +2:00 (Cairo)
* **Matrix:** @s9npai:matrix.org
* **Slack:** s9npai
* **Github:** S9npai
* **Blog:** https://s9npai.hashnode.dev/
* **PR link(s):**
  * **Drafted (1)-**
    1. [#1298: Added __repr_html__() method for catalog](https://github.com/astronomy-commons/lsdb/pull/1298)

### Background

My name is Omar Mohammed and I'm a 4th year CS major at Menoufia University, interested in low-level systems programming and machine learning.

One of my major career goals is to contribute to open source (as of my desire and curiosity to try something new).
So, I actively read **contribution guides** and also attended technical meetings held by veteran developers to know the exact steps and best practices to follow in the world of open source

Due to my technical background in **C++ & Python development**, I focused on finding organizations mainly working with these languages
I found **lincc-frameworks** and decided to explore one of their projects, which is **LSDB**, and practically start figuring out my way through the contribution process as a whole
I've got my hands on issue **#1274** which I resolved better than I initially thought, which eventually pulled me into the project codebase and motivated me to learn more about the project purposes and architecture

So what started as a quick look, turned into an engaging experience
Eventually I decided to pick up their idea for improving Arrow C++ libraries and its pyarrow API developed by **Apache Organization**, which I'm writing my GSoC proposal for today


### Interest in OpenAstronomy

What made me interested is the engineering aspect of Apache Arrow **in-memory** format, representing a pretty solid example for countless conceptual areas I studied, mainly **operating systems, high performance computing, big data, data science and data infrastructure**

Also, given the fact OpenAstronomy being an organization focusing on developing and improving stellar scientific computing technologies used by developers and scientists, this aligns well with my deep interests in astronomy and how it could benefit from data science and high performance computing

I am motivated to join the Arrow community and work on major core improvements for Arrow that are going to leave huge impacts for astronomy and beyond for the data science community as well


## Project Proposal Application

**Proposal Title:** **PyArrow Improvements for Astronomy**

**Organisation:** **OpenAstronomy (lincc-fw)**

**Project size:** **350h (Large)**

### **Summary:**
Large-scale astronomical catalog analysis produces billions of objects, libraries and tools such as **LSDB** and **Hiearchical Adaptive Tiling Scheme (HATS)** enable that by storing variable-length nested data in Parquet files via pyarrow’s nested types.
However, there are 3 main issues and shortcomings in the underlying Arrow C++ core: <br>
**1- No Sub-column projection for nested types <br>
2- Current threading model for nested struct-list data is slow <br>  
3- Missing compute kernels for nested types** <br>

The issues mentioned above are not minor gaps — they affect any workflow using HATS-format catalogs, nested-pandas, or pandas `ArrowDtype` with nested columns.   
Fixing these at the root in Arrow is the right approach rather than working around them at the LSDB or nested-pandas layer, where the workarounds would need to be maintained indefinitely  
This project will contribute fixes for all three problems directly to Apache Arrow, benefiting the broader data science ecosystem beyond astronomy


### Deliverables

**1.** **Nested Projection:** Enabled sub-column selection
(e.g., `columns=[observations.flux]`) in the Parquet reader

**2. Parallel nested reads:** Thread-safe, multi-threaded column reader for `list<struct>` types, with benchmarks for validating throughput gains compared to the current Parquet reader implementation

**3.** **C++ Compute Kernels:** New implementations of `replace_with_mask` and other  
compute kernels for List and Struct arrays

**4. Test and benchmark suites for the new features**: Unit tests for validating functionality and edge cases, as well as performance benchmarks for new implementations

**5. PyArrow Python API and bindings:** Exposing the new features to the **Python API** and writing integration tests ensuring it could use them correctly and utilizing their full performance

**6. Documentation and showcases**: Adding detailed guides and clear usage examples for the **PyArrow API** to show how the new improvements work and how to use them for different analytical workloads performed in **LSDB, nested-pandas** and other **Arrow-dependent libraries**


### Description/timeline


| Period                                            | Description                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Community Bonding** (May 1–16)                  | Join Arrow mailing list; introduce self in #arrow-dev<br><br>Build Arrow C++ from source and run existing test suite for the project, study `replace_with_mask` and other kernels' implementations for primitives<br><br>Coordinate with mentors on exam-period availability.                                                                                                                          |
| **Week 4** (Jun 24-30)                            | Implement `replace_with_mask` kernel on `list<T>` array<br>Write unit tests covering basic functionality and edge cases, including: empty lists, nulls, nested nulls                                                                                                                                                                                                                                   |
| **Week 5** (Jul 1–7)                              | Add benchmarks for list kernel using <br> address mentor feedback<br><br>Draft design docs for `struct<>` kernel                                                                                                                                                                                                                                                                                       |
| **Week 6** (Jul 8–14)                             | Implement `replace_with_mask` for struct arrays<br><br>Write tests for struct with mixed primitive/nested fields, update Python bindings in `python/pyarrow/_compute.pyx`                                                                                                                                                                                                                              |
| **Week 7** (Jul 15–21)                            | Finalize kernels for review: add docstrings, verify CI passes<br><br>Document Python API usage examples; prepare midterm demo notebook showing kernel functionality with synthetic nested data.                                                                                                                                                                                                        |
| **Week 8** (Jul 22–28)                            | Begin sub-column projection research: study `parquet/arrow/schema.cc`, `projection.cc`, and `reader.cc`<br>Draft design docs for path-parsing logic for dot-notation (e.g., `obs.flux`); open design discussion with mentors.                                                                                                                                                                          |
| **Week 9** (Jul 29 – Aug 4)                       | Extend C++ projection logic to handle dot-notation paths for `list<struct>` types<br> Add unit test: read only `flux` field from nested Parquet column, verify schema reconstruction matches expected narrowed output.                                                                                                                                                                                 |
| **Week 10** (Aug 5–11)                            | Implement schema reconstruction to map selected sub-fields to narrowed Arrow schema; integrate with Parquet reader to skip unneeded leaf columns on disk; add integration tests with real HATS-format catalogs.                                                                                                                                                                                        |
| **Week 11** (Aug 12–18)                           | Expose sub-column selection in PyArrow API: `pq.read_table(columns=["obs.flux"])`<br><br>Write Python integration tests & add usage example in docs                                                                                                                                                                                                                                                    |
| **Week 12** (Aug 19–25)                           | Thread-safety audit: review `RepetitionLevelDecoder` and `DefinitionLevelDecoder` for concurrent access; document reassembly path invariants; draft design for parallel leaf-column reads within RowGroup.                                                                                                                                                                                             |
| **Week 13** (Aug 26 – Sep 1)                      | Implement task decomposition: split struct-list read into independent leaf-column tasks; add thread-pool dispatch logic in `RowGroupReader::ReadColumns`; write concurrency test skeleton.                                                                                                                                                                                                             |
| **Week 14** (Sep 2-8)                             | Add synchronization barriers for nested reassembly (ensure list boundaries preserved across threads); write stress tests using ThreadSanitizer; fix any data races detected.                                                                                                                                                                                                                           |
| **Week 15** (Sep 9–15)                            | Benchmark nested vs. flat column throughput: vary row group size, nesting depth, core count<br><br> Profile **CPU / memory** usage; optimize task granularity based on results                                                                                                                                                                                                                         |
| **Week 16** (Sep 16–22)                           | Integrate all three features: ensure sub-column projection + threading + kernels work together; run full Arrow testing and CI suite<br><br>Fixing regression test failures and updating changelog entries.                                                                                                                                                                                             |
| **Week 17** (Sep 23–29)                           | Write usage documentation, add examples to **LSDB, nested-pandas** and other libraries' docs showing performance gains; ensure public APIs have docstrings.                                                                                                                                                                                                                                            |
| **Week 18** (Sep 30 – Oct 6)                      | **Integration Testing.** Final validation on real HATS catalogs **(e.g., ZTF, Gaia)**. Measure end-to-end speedup in LSDB workflow.                                                                                                                                                                                                                                                                    |
| **Week 19** (Oct 7–13)                            | **Buffer & Polish.** Address any late-stage review feedback. Ensure all PRs are merged or have clear follow-up issues                                                                                                                                                                                                                                                                                  |
| **Week 20** (Oct 14–20)                           | Prepare final demo video/notebook. Publish blog post draft with benchmark charts and usage guide                                                                                                                                                                                                                                                                                                       |
| **Wrap-up and Final Evaluation** (Oct 21 – Nov 2) | Address final mentor feedback, polish code comments, ensure docs are complete<br><br>Submit any remaining PRs and mention mentors for final review.<br><br>Ensuring all PRs merged or have clear follow-up issues<br><br>Publish final blog post<br><br>**Submit final report.** Document what was covered, what wasn't, and what would be needed to finish the remaining modules (for future contributors) |
|                                                   |                                                                                                                                                                                                                                                                                                                                                                                                        |


## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time applying to GSoC.
### Are you also applying to other projects?
No, I am focusing exclusively on OpenAstronomy this year

### Schedule availability
My practical and final exams run from May 17th to June 23rd, 2026, which overlaps with the community bonding period. During that time I can commit 6–8 hours per week rather than the full **20-26hrs** capacity.
Community bonding is intentionally lighter (Deeper codebase and architecture study, drafting initial design docs and mentor alignment, rather than major implementation work)

I will tell my mentors at least one week before exams start and share a revised schedule if needed, with regular weekly updates to ensure transparency, and I'll be available for meetings with mentors and project maintainers between **2PM-7PM GMT**

This will be my primary summer project, so I have no other competing commitments. No other internships or part-time work


## Other comments
Finally, thanks for considering my proposal, I'm so excited to be soon a part of Arrow and the astronomical data science community

