---
title: "Reverse Engineering Policy"
weight: 2
summary: "The clean-room process: what clean and tainted contributors may each do, and how to reverse engineer without copyright risk."
---

We are committed to ensuring that all source code and documentation produced by
the project is legal and free of copyright violations and other legal problems.
In order to ensure this, we have a policy that all contributors must follow,
particularly if they are doing certain kinds of reverse engineering.

We strictly follow a clean-room reverse engineering process. For any given
component, developers must identify as either "clean" or "tainted". Clean
developers **MUST NOT** ever look at any Apple binary for said component. This
includes any agents they may be using if applicable. To this end, we also
require that clean developers keep sufficient records of their process to
demonstrate the information they have learned does not come from Apple
binaries. Experiment logs, hardware traces, and LLM transcripts are examples of
this. Tainted developers may not contribute code, however, they are allowed to
contribute documentation provided they verify the documentation is free of
tainted information.

This is not a silver bullet to ensuring that we are free of copyright
violations; such a thing does not exist. However, it is a strong legal defense.

## Non-reverse-engineering development

If you are merely looking at existing Gravity Linux code and improving it
(without taking or referencing code from anywhere else), you are not reverse
engineering anything, and you do not need to worry about anything else. Just
make sure to follow the [Copyright Policy](/policies/copyright/).

## Referencing other open source code

**Warning: Be very careful of taking code from other Apple Silicon RE
projects.** The advent of LLMs has made it significantly easier to simply point
your LLM at Apple binaries, decompile them, and then build a working driver.
This is fundamentally incompatible with our clean room goals, and thus code
from such projects will be default rejected unless we can independently
validate their clean room development process.

There are additional restrictions on open source code.  In particular, be
careful of open source dumps from Apple, as they are often licensed under the
APSL, which is not GPL compatible. Gravity Linux does not allow any APSL code
to be used directly. You can use it as a reference for how things work, and you
can take inspiration from register names (since those are effectively hardware
documentation; we do not consider mere low-level register names to be
copyrightable), but do not copy whole `#define` blocks or include files, other
code, or reimplement identical code flows or algorithms. Follow sensible
downstream register naming conventions (such as prefixes).

For example,
[this](https://github.com/opensource-apple/xnu/blob/master/pexpert/pexpert/arm64/arm64_common.h#L10)
register, as documented in XNU include files, should be defined like this when
used for Linux:

```
#define SYS_HID0                  sys_reg(3, 0, 15, 0, 0)
```

Or perhaps:

```
#define SYS_APL_HID0              sys_reg(3, 0, 15, 0, 0)
```

Strictly speaking, referencing incompatibly-licensed code can run into similar
copyright issues to binary reverse engineering as detailed below; please make
sure to read that section to know what not to do.

Internal tools intended only for exploration and not to be released to
end-users are allowed to use APSL code, including taking APSL releases directly
and modifying them, as long as all licensing requirements are met. For example,
taking an APSL release, modifying it to aid in reverse engineering, and using
the result back in macOS is fine, and we can host such changes as a Gravity
Linux repo, but they cannot become an actual release of the project.

Put another way: we want to ensure not just license compatibility, but also
compatibility with this policy itself. Therefore, we do not allow combining
code developed under this policy with code not developed under this policy in
cases where a policy violation could have plausibly occurred.

## Hardware exploration

One particularly safe way to reverse engineer without running into copyright
issues is to simply probe the hardware to find out what it does. Any knowledge
gained this way is safe to use when writing open source code. For example,
dumping register areas, twiddling bits, and seeing what other bits change and
how the hardware behaves. This is particularly useful to complete knowledge of
a piece of hardware that is only partially understood, and can often lead to
insight that is not used and thus not present in the vendor's binary code.

## Register and data tracing

When blind probing isn't enough, another effective trick is to run macOS and
look at what it does. This can be done by dumping hardware register state
before and after an action, or by intercepting data between different
components, such as the userspace graphics stack and the kernel graphics
driver, and dumping or modifying data.

Register values and command buffers are usually not considered copyrightable,
so it is safe to use this approach to obtain the necessary information to write
an open source driver. 

Exceptions include cases where code or large blobs of data are uploaded to the
hardware. For example, a binary shader obtained by compiling your own GPU
shader source code would normally not be copyrightable, but if it contains
significant sections of code inserted by the compiler (for example, significant
scaffolding or algorithms to implement a particular feature), then that part
may be. Use your best judgement in these cases, and do not copy long
instruction sequences that do not directly correspond to code you have written
in source form.

Similarly, if any firmware blobs are uploaded to the hardware, those cannot be
copied as-is, and we must figure out how to approach them on a case-by-case
basis.

## Binary disassembly and decompilation

No contributor who has disassembled or decompiled an Apple binary is allowed to
contribute code for that component. There are no exceptions to this policy, and
even core Gravity contributors are bound by it. If you have introspected an
Apple binary, you must disclose this and restrict yourself to writing
documentation.

Contributors doing binary reverse engineering are responsible for any legal
consequences of their work, including any consequences of the license
associated with said code.

**Important**: In order to ensure the legal safety of project members that
don't want to involve themselves in this reverse engineering approach, *all*
such discussion is allowed **only** in the #tainted-re Discord channel. This
will be strictly enforced, and work involving binary RE on other channels will
lead to a kick or a ban. Additionally, do not paste any decompiled/disassembled
code on Discord.

## Usage of unreleased materials

Gravity Linux absolutely forbids the usage of any copyrighted materials not
available to the public during reverse engineering. This includes any leaked
software (in source or binary form), unreleased documentation, non-public
releases (such as restricted betas), etc. Project contributors are expected to
refrain from acquiring or using any such content. Only materials that are
explicitly made available to the public at large may be used. This applies to
materials from both Apple and any other third parties.

As a consequence, anyone who has seen internal macOS source code is unable to
contribute.

Policy adapted from Asahi Linux's Reverse Engineering policy.
