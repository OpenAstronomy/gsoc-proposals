# Hardening Astropy's core stability - Jett Higgins

## Contributor Information
* **Name:** Jett Higgins
* **Time-zone:** Eastern Standard Time
* **Matrix/slack/IRC Handle:** Jett Higgins
* **Github/forge username:** Jett Higgins
* **Blog:** jetthiggins.github.io/Jblog/
* **PR link(s):**
  * https://github.com/astropy/astropy/pull/19322
  * https://github.com/astropy/astropy/pull/19238
  * https://github.com/astropy/astropy/pull/19449/

### Background
My name is Jett Higgins, I am a 19 year old year old sophomore currently studying Computer Science at Stevens Institute of Technology. Before this project was announced I contributed a small documentation fix to get familiar with the codebase. I did this because I knew I wanted to do a GSoC project related to astronomy. Once I saw the project was a combination of low-level and high-level programming I thought that this project is perfect for my interests. I have learned C, C++, and Assembly through school courses and some personal projects, like building an 8 Bit CPU in Logisim, based off the CPU concepts I covered in class, and when building it I had to eventually split up my components of the CPU into modules to reduce the complexity of managing all the logic gates, which is very similar to the APE linked with this proposal. Another project I really liked from school was recreating a simple version of printf in assembly, it taught me how to manage stack frames, labels, and taught me how to debug low level code with gdb. Recently I did a hackathon where my team ended up winning one of the project tracks. I was able to work with a graduate student who took my initial ideas refined the database schemas into the winning project.

### Interest in OpenAstronomy
My interests in Astronomy came from watching and reading sci-fi content like Star Wars, Interstellar, the Martian. I most recently read the book Project Hail Mary. So when I started initial work on tests for the FITS subpackage I started thinking about how the data recorders in Project Hail Mary could have stored data in FITS files. Stories like that and seeing the creativity of how they are used makes me feel connected to astronomy.

## Project Proposal Application
**Proposal Title:** Hardening astropy’s core stability

**Organisation:** Astropy

### **Summary:**
Astropy is a core building block of the astronomy ecosystem. It needs to be reliable and performant. Astropy gets its performance from low level source functions written in Cython and C. These low-level packages in astropy are rarely updated but take up to 50% of CI time recompiling. The [APE](https://github.com/neutrinoceros/astropy-APEs/pull/1) proposes the idea of splitting the low-level functions into a separate package. This will give astropy significantly reduced build times, a clearer separation of the low-level and high-level layer and allows for more modern build systems (meson suggested which would avoid recompiling unchanged code). The goal of this GSoC project is a step toward completing the APE. Currently the low-level layer of astropy has minimal tests, if astropy were to split now, the low-level functions would be untested. This project is to write tests for the core low-level sub-packages in astropy. I developed an approach for implementing these tests when I was writing tests for the FITS parse_header function. First I spent time fully understanding how the current function works and FITS files actually work. Then I started thinking about edge cases specific to the function and then began writing code. A well tested low-level layer will allow the APE to move forward.

### Deliverables

Due to the nature of the GSoC process I see that there are a lot of PRs currently being created for tests for low level functions (me included). I believe that there should be some time allocated to making sure that these tests do cover all the edge cases necesary and cleaned up if needed. Here is a list of low level functions that I can see that do have or have tests pending from open PRs right now.

Should I have time after all the tests are written, I would like to experiment with Meson to reduce recompilation of the packages.

### Implemented Or Open PRs
time/src/parse_times.c - _parse_times (Merged) [Link](https://github.com/astropy/astropy/pull/19410)

cosmology/_src/flrw/scalar_inv_efuncs.pyx (Merged) [Link](https://github.com/astropy/astropy/pull/19407)

table/_np_utils (Merged) [Link](https://github.com/astropy/astropy/pull/19458)

io/fits/_utils.pyx - parse_header (Open PR) [My PR](https://github.com/astropy/astropy/pull/19449)

stats/_stats.pyx (Open PR) [Link](https://github.com/astropy/astropy/pull/19378)


stats/src/fast_sigma_clip.c (Open PR) [Link](https://github.com/astropy/astropy/pull/19377)

table/_column_mixins.pyx (Open PR) [Link](https://github.com/astropy/astropy/pull/19492)

convolution/_convolve.pyx (Open PR) [Link](https://github.com/astropy/astropy/pull/19470)

An Example of How I would Map out these files during the community bonding phase

io/ascii/cparser.pyx (Untested)

*Map of cparser.pyx*

- FileString
    - __cinit__ (non-trivial logic, should probably test assignments and file status) *I think this function might not ever raise the OSError*
    - splitlines

- CParser
    - __cinit__ (errors should be tested, assignments could be skipped probably)
    - setup_tokenizer (test with 4 different input types: filename/data, file-like object, iterable lines, error)
    - read_header
    - read

- _copy_cparser

- read_chunk

- FastWriter
    - __cinit__
    - write

- get_fill_values

**Other untested directly low-level packages**

timeseries/periodograms/lombscargle/implementations/cython_impl.pyx (Untested)

timeseries/periodograms/bls/_impl.pyx (Untested)

utils/xml/src/iterparse.c (escape_xml is tested but escape_xml_cdata is untested)


wcs - biggest low level package, multiple files

### Timeline

| Period | Dates | Description |
|--------|-------|-------------|
| **Community Bonding** | Apr 28 – May 25 | Meet with mentors to finalize the test plan. Map out all untested low-level subpackages. Agree on priority ordering and milestone targets with mentors. |
| **Week 1–2** | May 26 – Jun 8 | Go through all currently implemented or open-PR low-level tests. Identify missing edge cases, inconsistencies, or gaps. Submit fixes/improvements to existing test PRs as needed. |
| **Week 3** | Jun 9 – Jun 15 | Begin and complete tests for `io/ascii/cparser.pyx`. Open PR and iterate on review feedback. |
| **Week 4** | Jun 16 – Jun 22 | Tests for `timeseries/periodograms/lombscargle/implementations/cython_impl.pyx` and `timeseries/periodograms/bls/_impl.pyx`. |
| **Week 5** | Jun 23 – Jun 29 | Tests for `utils/xml/src/iterparse.c` and `cosmology/_src/signature_deprecations.c`. |
| **Week 6–7** | Jun 30 – Jul 13 | Tests for any unmapped low-level packages discovered during community bonding. |
| **Midterm Evaluation** | Jul 14 – Jul 18 | Midterm evaluation period. Review progress with mentor, adjust remaining schedule if needed. |
| **Week 8–9** | Jul 19 – Aug 1 | Begin tests for the `wcs` subpackage, should take time to fully understand what each exposed low level function/object is doing. |
| **Week 10–11** | Aug 2 – Aug 15 | Continue and complete `wcs` tests. Cover edge cases, error handling, and boundary conditions. Open PR and work through mentor review feedback. |
| **Week 12** | Aug 16 – Aug 22 | Buffer week: address any remaining untested functions identified during the project, finalize all open PRs, write/update documentation for new tests, and ensure CI passes cleanly. |
| **Final Evaluation** | Aug 24 – Aug 31 | Submit final work product. Write up a summary of all tests added, coverage improvements, and notes for the APE moving forward. |

**Hours per week:** 30+ hours

### Have you participated previously in GSoC? When? With which project?
No this is my first time

### Are you also applying to other projects?
No

### Schedule availability
I will be fully availible during the GSOC period, I will be staying with my family in the UK during after my school semester ends on May 10th. But I should remain availible to code for 30+ hours per week during the gsoc timeline.
