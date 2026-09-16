---
title: "Contributors"
subtitle: "How to contribute to Gravity Linux."
---

Thank you for your interest in contributing! There are two (and a half) ways
you can contribute to Gravity Linux: financial, development, and spreading the
word.

## Financial Support and Donations

If you would be interested in sponsoring a new Apple device, please reach out
on Discord. We do not currently have a means of taking donations, but will set
up a Github Sponsors if there is sufficient interest.

## Use Gravity and Spread the Word

Using Gravity Linux is itself useful. Install it on your hardware, report bugs,
tell us what works and what does not, and help us understand which machines and
configurations need attention. Good bug reports with clear reproduction steps,
logs, and hardware informationcan save us a significant amount of time.

You can also help by talking about Gravity, sharing the project with people who
may be interested, writing about your experience, and pointing potential
contributors in our direction. Gravity Linux is built by random people on the
internet passsionate about open source, systems, and building great things. A
project like this depends on having people willing to test and improve our code
and help grow the community around it.

## Contributing Technical Work

### Policies

Before any technical contributions, please read our policies:

- [Copyright Policy](/policies/copyright/)
- [Reverse Engineering Policy](/policies/reverse-engineering/)
- [LLM Use and Clean-Room Policy](/policies/llm/)

Additionally, please talk to us on [Discord](https://discord.gg/sEEz2wN4v)
first. We're always happy to discuss the project and ways you can contribute,
and want to ensure your work helps solve the highest impact problems our
project faces and doesn't duplicate work other people are doing.

Broadly, there are two kinds of reverse engineering work: **tainted RE** 
and **clean-room RE**.

### Tainted Reverse Engineering

If you—or an agent you use—have ever decompiled, disassembled, or otherwise
inspected Apple's kernelcache or other proprietary Apple binaries, you are
limited to tainted RE for the components you have inspected.

That does not make the work less important. Tainted RE is how we turn
information about the existing implementation into documentation that
clean-room contributors can safely use. The final output of this work is
*documentation*, not code.

The general process is:

1. **Analyze the Apple binary.** Inspect, disassemble, or decompile the
   relevant Apple implementation as necessary to understand how the hardware is
   operated.

2. **Build a private tainted implementation.** Write your own driver or test
   implementation based on that analysis and verify that your understanding of
   the hardware is correct.

   **Do not publish, share, or contribute this code to Gravity.** The code
   itself shall never become public, but its functionality proves the hardware
   understanding is correct.

3. **Document the hardware interface.** Once you understand the device, write
   documentation describing the hardware interface itself: registers, commands,
   data formats, MMIO offsets, protocols, required sequencing, firmware ABI,
   and other externally observable properties.

   You may also provide *high-level prose* describing what your private
   driver does. Be light on details and reference only widely known technical
   concepts.

   **Be extremely careful not to include implementation details derived from
   Apple's code.** The purpose of this documentation is to describe the
   hardware interface and observable behavior, not Apple's implementation of
   it. You are responsible for ensuring that no tainted implementation details
   leak. The general rule is that if you aren't 100 percent sure that a piece
   of information is safe to share, don't include it. Remember that the clean
   room implementer has powerful observability tools such as our hypervisor
   and thus does not need every piece of information.

4. **Publish the documentation.** The resulting documentation should be made
   publicly available so that clean-room contributors can implement support
   from the documentation without examining the Apple implementation
   themselves.

### Clean-Room Reverse Engineering

If you have not been exposed to relevant Apple proprietary implementation, you
can contribute clean-room reverse-engineering and implementation work.

Clean-room contributors may study the behavior of the hardware, collect traces,
experiment with devices, improve our tooling, and write new implementations
from the resulting documentation and observations. However, you **MUST NOT**
inspect any Apple proprietary code or other non-public implementation details.
There are no exceptions to this rule for experienced contributors, maintainers,
or core developers.

If a task requires crossing that boundary, stop and hand it off to someone
doing tainted reverse engineering instead.

Clean-room work will generally involve either improving our tooling, or writing
Linux drivers based on public documentation and hardware observations.

### First Time Contributor Guide

The first step is setting up a development machine. See the
[Tethered Boot guide](/docs/developers/tethered-boot/) in the documentation.
