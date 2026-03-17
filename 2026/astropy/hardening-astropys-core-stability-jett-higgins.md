# Hardening Astropy's core stability - Jett Higgins

## Contributor Information
* **Name:** Jett Higgins
* **Time-zone:** Eastern Standard Time
* **Matrix/slack/IRC Handle:** JettHiggins
* **Github/forge username:** Jett Higgins
* **Blog:** jetthiggins.github.io/Jblog/
* **PR link(s):**
  * https://github.com/astropy/astropy/pull/19322
  * https://github.com/astropy/astropy/pull/19238
  * https://github.com/astropy/astropy/pull/19449/

### Background
My name is Jett Higgins, I am a second year student currently studying Computer Science at Stevens Institute of Technology. I have enjoyed programming and working on projects since I started highschool. I just enjoy learning new things and working on and endlessly growing list of projects that continute to challenge me. I have picked up C, C++ and Assembly through school courses and some personal projects that have popped into my head. I have created my own 8 bit CPU, recreated printf in Assembly, and learned core concepts in my classes. I am super open to working with other people, I try to learn as much as I can from people smarter than me and try to teach people that ask for it. I have had a great time working with people in Hackathons and Game Jams. I am super excited for this oppourtunity to work on this project!
                                                                                                                                                                                                
### Interest in OpenAstronomy
I my intrests in Astronomy came from watching and reading a ton of sci-fi content as a kid like Star Wars, Interstellar, the Martian. I most recently read the books Project Hail Mary and Dark Matter, consuming that kind of content really motivates me to work on projects like astropy, and I am always looking for more books to read. OpenAstronomy allows me to take my personal interests and apply it to what I have been learning throughout my life, which is programming, more specifically in Python and C.

## Project Proposal Application
**Proposal Title:** Hardening astropy’s core stability

**Organisation:** Astropy

### **Summary:**
I have really liked to deep dive into things and always have questions on how things work. Thats why I am so interested in C and other low level languages, because they are the reasons that we can do more abstract things in high level languages like python. Astropy (like C) is a core bedrock of the astronomy ecosystem. It needs to be reliable, well documented, and remain highly performant. That is why contributing to astropy and this project is so important to me. These low-level packages in astropy are rarely updated but can take up to 50% of the CI time just recompiling all this low level code. The APE https://github.com/neutrinoceros/astropy-APEs/pull/1 suggests we split the low level parts of astropy into a seperate package. Allowing for signifigantly reduced build times, a more modular approach to the codebase and even allows for a more modern build system using meson or maturin. This combination of low level code that supports higher level function really interests me, and I believe I can do it and learn a lot at the same time.



### Deliverables

**During GSoC:**
1. Direct tests for `astropy/table/_np_utils.pyx` - join_inner function
2. Direct tests for `astropy/table/_column_mixins.pyx` - __getitem__ overrides
3. Direct tests for `astropy/convolution/_convolve.pyx` - convolution operations
4. Direct tests for `astropy/io/ascii/cparser.pyx` - fast ASCII parser
5. Direct tests for `astropy/timeseries/periodograms/lombscargle/cython_impl.pyx`
6. Direct tests for `astropy/timeseries/periodograms/bls/_impl.pyx`
7. Written guide: "Testing Cython Extensions in Astropy" for future contributors
8. Final report with roadmap for remaining extensions (votable, compressed, xml)

**Reach goals:**
9. Direct tests for `astropy/io/votable/src/tablewriter.c`
10. Direct tests for `astropy/io/fits/hdu/compressed/src/compression.c`
11. Exploratory Meson build setup for isolated extension testing

---

### Timeline

| Period | Description |
|--------|-------------|
| Community Bonding (2 weeks) | Map all untested C/Cython extensions with mentors. Study the patterns from `time/_parse_times` direct tests. Discuss implementation strategy with mentors. |
| Week 1 (May 26 - June 1) | astropy/table/_np_utils.pyx - Start direct tests for `join_inner` function. Test all 4 join types (inner, outer, left, right). Test masking behavior with masked arrays. Open draft PR early for mentor feedback. |
| Week 2 (June 2 - 8) | Finish table/_np_utils - Edge cases: empty tables, single rows, duplicate keys. Memory safety tests (boundscheck disabled code). Ensure CI passes on all platforms. |
| Week 3 (June 9 - 15) | astropy/table/_column_mixins.pyx - Direct tests for __getitem__ overrides. Test structured array field access. Test multi-dimensional column slicing. Verify behavior matches Python fallback. |
| Week 4 (June 16 - 22) | Finish table extensions - Finalize table PRs. Write internal guide documenting testing patterns: How to call Cython functions directly, Structured dtype construction, Memory safety testing strategies. This guide helps future contributors. |
| Week 5 (June 23 - 29) | astropy/convolution/_convolve.pyx - Start direct tests for convolution functions. Numerical accuracy: compare C vs Python results. Test boundary conditions and padding modes. |
| Week 6 (June 30 - July 6) | Finish convolution -  Midterm report due. |
| Week 7 (July 7 - 13) | astropy/io/ascii/cparser.pyx - Direct tests for the fast C ASCII parser. Test parsing correctness for various file formats. Error path coverage: malformed input, encoding issues. |
| Week 8 (July 14 - 20) | astropy/timeseries/periodograms/lombscargle/cython_impl.pyx - Test periodogram computation directly. Numerical accuracy against known test cases. Performance comparisons. |
| Week 9 (July 21 - 27) | astropy/timeseries/periodograms/bls/_impl.pyx - Direct tests for BLS implementation. Test light curve fitting. Edge cases: sparse data, periodicity detection. |
| Week 10 (July 28 - Aug 3) | Buffer + stretch goals - Buffer time for any PR review iterations. If ahead of schedule: start `io/votable` or `fits/compressed` tests. Begin exploratory Meson build setup. |
| Week 11 (Aug 4 - 10) | No new features. Fix any platform-specific CI issues. Ensure all PRs passing on Linux, macOS, Windows. Performance benchmark documentation. |
| Week 12 (Aug 11 - 17) | Final report - Document which extensions tested. What strategies worked. What was difficult. Roadmap for remaining extensions (votable, compressed, xml). Submit final evaluation. |

**Hours per week:** 30+ hours

## GSoC

### Have you participated previously in GSoC? When? With which project?
No this is my first time

### Are you also applying to other projects?
No

### Schedule availability
I will be fully availible during the GSOC period, I will be staying with my family in the UK during after my school semester ends on May 10th. But I should remain availible to code for 30+ hours per week during the gsoc timeline.
