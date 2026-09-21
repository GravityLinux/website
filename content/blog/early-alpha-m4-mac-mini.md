---
title: "Introducing Gravity Linux Early Alpha for the M4 Mac mini"
date: 2026-09-18
draft: false
summary: "It's finally ready!"
---

We are proud to announce the early alpha release of Gravity Linux for the M4
Mac mini. This release includes working support for the DCP and GPU (OpenGL ES
3.0 and OpenGL 3.3 compliance).

Gravity Linux is a Fedora remix and by default uses KDE with Wayland by
default.

**This release is primarily intended FOR DEVELOPERS. It is not yet ready to be
used as a daily driver.**

Before installing, there are some limitations you should know about:

- *There may be no upgrade path from this alpha to the beta or final release.*
  You should assume that the beta will require a complete reinstall.

- *The following features are known not to work:* suspend, display out on the
  USB-C ports (HDMI works), and Thunderbolt/USB4. Additionally, power
  consumption may be higher than expected (not very relevant for a desktop);
  we have set conservative limits for max speaker volume for safety reasons;
  and shutdown/reboot are flaky (you can just use the power button anyways).
  Every feature *not* listed here works and has been tested.

- *The entire release has gone through very limited testing.* There may be
  general stability/usability issues. Because this release is intended for
  developers, some settings are enabled that are not usually enabled for
  release builds.

If any of those are deal-breakers, we recommend waiting for a later release.

This release is meant to get Gravity Linux into the hands of developers early,
so people can test real workloads, find bugs, contribute fixes, and help us
harden the platform before beta. Our beta release will significantly alleviate
and/or remove the above limitations.

Additionally, we (Cody Ho and Niklas Sheth) are not experts in maintaining
Linux distributions. We take the quality of the user experience extremely
seriously and will do our best to avoid user-facing errors, especially those
impacting privacy and security. Nevertheless, it is likely that mistakes will
be made at some point.

## Installing

From macOS, just run the following shell command:

```bash
curl -fsSL https://install.gravitylinux.org | sh
```

The installer will handle the required setup and installation process from
there.

## What’s next

This alpha release on the M4 Mac mini is only the beginning. We’re working
toward extending Gravity Linux support to the MacBook Neo and the remaining M4,
M4 Pro, and M4 Max devices soon, and some promising new contributors have
already made significant progress on this front. All M5 and M6 devices remain
in scope as well.

Our long term-goal is to upstream all of our work as the only practical way of
ensuring a sustainable project. To this end, we have gone out of our way to
document our methods and findings, respect Apple's copyright (we have not
looked at any Apple binaries to write Linux drivers), and follow a textbook
clean-room reverse engineering process.

For now, if you have an M4 Mac mini, enjoy experimenting— and please report
everything you break.

## Join Us

If you want to be a part of our journey, please talk to us on
[Discord](https://discord.gg/sEEz2wN4v), and have a read through our
[contributor guide](/contributors/).
