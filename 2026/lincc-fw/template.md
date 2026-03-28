> [!CAUTION]
> Do not edit this template directly!
> Instead copy the file into this year directory as explained in the [README](../README.md#how-to-apply) and open a Pull-Request.
>
> Please, follow this template as indicated and do not add any personal information like phone number/email/home address.
>
> If you wish to add images, create a directory under this year directory named: `your-name/` and add any extra images required.
>
> You should delete this warning box.

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
One of this year's major goals for me was to contribute to open source (as of my desire and curiosity to try something new). 
So, I started watching some guides and also attended some meetings held by veteran developers who previously went through the open source contribution experience as a whole 
For my background in C++ & Python, I mainly focused on finding organizations that used these languages. 
I found **lincc-frameworks** and decided to start with one of their projects, which is **LSDB**. 
I've got my hands on issue **#1274**, which eventually pulled me into the project codebase and motivated me to learn more about the project purposes and architecture. 
What started as a quick look, turned into an engaging experience
Eventually I decided to pick up their idea for improving Arrow C++ core libraries and its pyarrow API, which I'm writing my GSoC proposal for today.


### Interest in OpenAstronomy
What made me interested is the engineering side of Apache Arrow, representing a pretty solid example for countless conceptual areas I studied, mainly big data, data science and data infrastructure



I am motivated to work on major core improvements for Arrow that are going to leave huge impacts for astronomy and the data science community in general


## Project Proposal Application
**Proposal Title:** **PyArrow Improvements for Astronomy**

**Organisation:** **OpenAstronomy (lincc-fw)**

**Project size:** **350h (Large)**

### **Summary:**
Large-scale astronomical catalog analysis produces billions of objects, libraries and tools such as LSDB and Hiearchical Adaptive Tiling Scheme (HATS) enable that by storing variable-length nested data in 
Parquet files via pyarrow’s nested types. However, there are 3 main issues and shortcomings in the underlying Arrow C++ core: <br>

**1- No Sub-column projection for nested types <br>
2- Current threading model for nested struct-list data is slow <br>
3- Missing compute kernels for nested types** <br>

The issues mentioned above are not minor gaps — they affect any workflow using HATS-format catalogs, nested-pandas, or pandas ArrowDtype with nested columns. 
I believe fixing these at the root in Arrow is the right approach rather than working around them at the LSDB or nested-pandas layer, where the workarounds would need to be maintained indefinitely
This project will contribute fixes for all three problems directly to Apache Arrow, benefiting the broader data science ecosystem beyond astronomy




### Deliverables
**1.** **C++ Compute Kernels:** new implementations of `replace_with_mask` and other
compute kernels for List and Struct arrays

**2.** **Nested Projection:** Enabled sub-column selection
(e.g., `columns=[observations.flux]`) in the Parquet reader

**3.** 


### Description/timeline

|Period|Description|
|------|-----------|
|Community Bonding period| ... |
| week 1 (dates) | ...|
| ... | ...|
| week n | ... |

## GSoC

### Have you participated previously in GSoC? When? With which project?
No, this is my first time applying to GSoC.
### Are you also applying to other projects?
No, I am focusing exclusively on OpenAstronomy this cycle.

### Schedule availability
Mt practical and final exams run from May 17th to June 23rd, 2026, during that time
My end-semester exams run May 5th to May 26th, 2026, which overlaps with the community bonding period. During those three weeks,
I can commit 6–8 hours per week rather than the full 20-26hrs. 
Community bonding is intentionally lighter (codebase reading, environment setup, mentor alignment rather than major implementation and design work)
I will tell my mentors at least one week before exams start and share a revised schedule if needed
This will be my primary summer project, so I have no other competing commitments. No other internships or part-time work


## Other comments

