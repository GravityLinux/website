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

*This is extremely preliminary and will be replaced when I have more time.*

First, make sure you have *TWO* Apple Silicon macs. Unfortunately, this is
required in order to use
[macvdmtool](https://github.com/GravityLinux/macvdmtool).  One of these macs
will become the master which will send commands to a proxy running on the
slave.

Second, install [Gravity Bootloader](https://github.com/GravityLinux/bootloader)
on the slave. This itself is a fairly detailed process, but the tl;dr is:

0. Compile a copy of Gravity Bootloader, stick this somewhere on the primary
   macOS volume.

1. Create a new macOS container.

2. Install a fresh copy of macOS into this container. The best way to do this
   is to run:

   ```
   softwareupdate --fetch-full-installer --full-installer-version VERSION
   ```

   where VERSION is 26.6.2 for any M4 or A18 Pro class Mac, or 27.0 for any M5
   class Mac.

3. Install macOS into that container, then reboot into that container to finish
   the installation process.

4. Boot into 1TR. From 1TR, open a terminal and run:

   ```
   bless --mount /Volumes/THE VOLUME YOU JUST CREATED HERE --setBoot
   ```

5. Reboot the device again into 1TR.

6. First, enter permissive boot security so you can install custom objects:

   ```
   bputil -nkas
   ```
   This step will prompt you for a UUID, enter the UUID of the volume you just
   created. You can check the UUID of a volume using `bputil -d`, you want the
   one that says Paired.

7. Then, install the actual bootloader object:

   ```
   kmutil configure-boot \
        -v /Volumes/YOUR VOLUME HERE \
        -c /Volumes/PATH TO YOUR RAW GRAVITY BOOTLOADER OBJECT HERE \
        --raw \
        --entrypoint 2048 \
        --lowest-virtual-address 0
   ```

   If successful, connect your host to the DFU port of your machine and reboot,
   you should boot into Gravity Bootloader.

Third, pop a shell and verify that the proxy works. If it does,
congratulations! You just took your first step into the world of Linux on Apple
Silicon.

**Do not worry if this process takes you a very long time.** It took me months
to fully iron out all the issues, and getting it working for the first time
took weeks. I think this is a massive edge case, but the point is that this is
a difficult process you should be proud of accomplishing!
