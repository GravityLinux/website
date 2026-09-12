---
title: "About"
subtitle: "What we do"
---

Our goal is to add Linux support to modern (M4 and above) Apple Silicon Macs
and upstream our support to the Linux kernel and all necessary userspace
projects.

Currently, we ship a Fedora remix as a downstream distro with support for all
Apple Silicon hardware, as a way to quickly get you access to the latest
hardware and to test our drivers before release. One day, we will sunset
Gravity Linux the distro as it will simply become a part of all other Linux
distros via upstream projects.

## How we work

Everything in Gravity Linux is built by clean-room reverse engineering. We do
not look at Apple source code, disassembly, or leaked technical information,
and contributors who have had access to any of those cannot work on the
corresponding components. Our [policies](/policies/) spell out exactly what that
means in practice.

Our long-term goal is to upstream all of our work. That is the only practical
way of ensuring a sustainable project, so we document our methods and findings
as we go. In particular, we take care to ensure that our reverse engineering
methods and procedures are public, and that we publish artifacts sufficient to
verify the clean room nature of our work

## FAQ

### Gravity Linux vs Asahi Linux

Gravity Linux and Asahi Linux are two independent projects working towards the
shared goal of a strong, open OS ecosystem on Apple Silicon devices. Gravity
Linux was initially forked from Asahi Linux over differences in policy, namely
regarding LLM use. We have tremendous respect for the Asahi Linux project and
emphasize our shared goal of running Linux on Apple Silicon.

### When will X device be supported?

There's generally two reasons why a device isn't yet supported, either because
we don't have the device, or because there's some reason we need to wait for
Apple to fix something about the platform for us. For both of these cases, the
answer is "We don't know when X device will be supported."

Currently, as of September 2026, our team owns a MacBook Neo (no fingerprint
reader), M5 MacBook Air, and M4 Mac Mini. If you want Gravity Linux support for
a device not on this list, the most impactful thing you could do is reach out
and sponsor a device we don't yet have, or (if you are a developer) to
[add support for it yourself](/contributors/).

As of macOS 27.0 Beta 5, there are issues with the M5 Pro and Max devices. In
brief, they are misconfigured out of the box and we are unable to start
secondary CPU cores. We have no choice but to wait for Apple to fix this
problem. It has been reported to them and they are aware of its existance. This
is also going to hold back M5 support because, for maintainability reasons, we
want to keep the whole M5 line on the same firmware version.

No member of our team owns the M6, so we cannot reasonably give an
approximation of when it may be ready.

For all devices that our team owns, that do not have blocking issues, you can
assume we are making progress and can follow our blog posts and/or Discord
server to keep up to date with device support. The answer in this case is still
"We don't know exactly" but our shipping velocity is quite fast and want to get
the code in your hands as fast as possible.

### What's Apple's stance on all of this?

Apple went out of their way to allow the booting of alternative operating
systems on Apple Silicon ([as documented by Asahi
Linux](https://asahilinux.org/docs/platform/security/#per-container-security-policies)). We know that some Apple engineers are
supportive of projects of this sort. Our goal is to ensure we never become the
subjects of Apple's ire by treating their copyright with a wide bearth and
adopting well understood, defensible, and transparent reverse engineering
processes and procedures.

[Join the Discord &rarr;](https://discord.gg/sEEz2wN4v)
