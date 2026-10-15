---
title: "Hypervisor Usage Guide"
subtitle: "How to use Gravity Hypervisor to RE a Mac"
weight: 10
wip: false
---

This describes a general workflow of how to use the hypervisor. It will likely
need to be adapted for your exact needs. The purpose of the hypervisor is to
trace the MMIO actions of a macOS guest so we can perform dynamic analysis and
understand how the hardware works without ever having to look at macOS code.

The hypervisor also has a built in Python shell, which can be used to interact
with a frozen guest. In particular, it is useful for gathering snapshots in
shared memory that are not captured by MMIO tracing.

## Retrieve the kernelcache

First, retrieve the correct kernelcache for your device and macOS version. To
find what kernelcache you need for your device, first get the Mac's product
type by running

```
sysctl -n hw.model
```

from *inside macOS*. This gives you a value like `Mac17,5` (with your Mac's
hardware numbers obviously).

As for macOS version, M4 class macs and the MacBook Neo should retrieve the
26.6.2 kernelcache, M5 and later class macs are not yet supported in the
upstream HV, but if you want to work on them anyways download the latest macOS
version.

### IPSW Extraction

First, install the `ipsw` command, this can be done using Homebrew:

```
brew install blacktop/tap/ipsw
````

If you do not have Homebrew installed, you can also download directly from
github. Then extract the right kernelcache:

```
ipsw extract --kernel --device PRODUCT_TYPE_HERE -o OUTPUT_DIRECTORY /PATH/TO/IPSW
```

For example:


```
ipsw extract --kernel --device Mac17,5 -o out ./UniversalMac_26.6.2_25G83_Restore.ipsw
```

The `ipsw` command itself is very useful and contains a significant amount of
other functionality.

If you have issues determining which kernelcache you need, use `ipsw info
/PATH/TO/IPSW`. Worst case, you can also look for your Mac's name in
`BuildManifest.plist` inside the IPSW:

```
unzip -p IPSW_HERE BuildManifest.plist > BuildManifest.plist
plutil -p BuildManifest.plist
```

### KDK Extraction

First, create an Apple developer account. Then visit
[https://developer.apple.com/downloads/all](https://developer.apple.com/downloads/all)
and search for the exact macOS version you want. Install that KDK and grab the
kernelcache from the default install directory.

## Using the Hypervisor

Temporary bring up note: right now you need to merge PRs:

https://github.com/GravityLinux/bootloader/pull/8

https://github.com/GravityLinux/bootloader/pull/9

and SMP is known to be flaky. This text will be removed when the PRs are
merged.

Before you start, set your `M1N1DEVICE` to the correct device:

```
export M1N1DEVICE=/dev/cu.usbmodemSERIALNUMBERHERE
```

0. Reboot the device

```
macvdmtool --rid CORRECT_RID_HERE reboot
```

If you don't have macvdmtool, build it from
[here](https://github.com/GravityLinux/macvdmtool).  If you're not sure which
RID to use, just start from 0 and go up.

If your host machine (ie, the one that is NOT being RE'd) is already running
Gravity then you can use [tuxvdmtool](https://github.com/AsahiLinux/tuxvdmtool)
instead of macvdmtool.


1. Chainload the current hypervisor build

```
python3 proxyclient/tools/chainload.py build/m1n1.bin
```

2. (Optional) Write a trace script for the component you're trying to RE

You can use the existing trace scripts as templates; refer to the ADT for the
right nodes/ranges.

3. Invoke the hypervisor entrypoint

```
python3 proxyclient/tools/run_guest.py \
    -m OPTIONAL_TRACE_SCRIPT_HERE.py \
    -l OPTIONAL_LOG_FILE_TO_WRITE_TO_HERE \
    PATH_TO_KERNELCACHE_HERE
```

4. Profit (ie, save the trace, then use that to build a driver)

## Tips and Tricks

### Single user mode and macOS shell

For some devices, it can be useful to boot in single user mode to reduce the
amount of noise in the MMIO trace.  You can do this by passing `-s` and
`serial=3` as guest boot arguments:

```
python3 proxyclient/tools/run_guest.py PATH_TO_KERNELCACHE -- -s serial=3
```

Now the USB proxy is bridged to the macOS USB interface. You can run commands
on macOS using:

```
screen /dev/cu.usbmodemSERIALNUMBER3 115200
```

Note the `3` after the serial number, the primary proxy uses `1`.

### GPU RE Notes

The GPU kernel driver is special in that most of the information is *NOT*
contained in the MMIO trace, it's actually located in the tree in shared
memory (there are other devices that communicate in shared memory, but the GPU
is *by far* the most complicated).

Thus, the following standard operating procedure should be followed when doing
GPU RE:

1. Write a small metal program that exercises whatever GPU functionality you want

2. Boot into single user mode with a macOS shell as described above.

3. Enable the mininum services to enable Metal:

```
# Mount the guest Data volume for temporary files used by Metal compilation.
/sbin/mount -P 1
/usr/libexec/init_data_protection
/sbin/mount -P 2
/bin/ls -ld /private/var/tmp /private/var/folders

# Start the services used by the Metal compiler, in the system launchd domain.
/bin/launchctl bootstrap system /System/Library/LaunchDaemons/com.apple.bsd.dirhelper.plist
/bin/launchctl print system/com.apple.bsd.dirhelper

/bin/launchctl bootstrap system /System/Library/LaunchDaemons/com.apple.notifyd.plist
/bin/launchctl print system/com.apple.notifyd

/bin/launchctl bootstrap system /System/Library/Frameworks/Metal.framework/Versions/A/XPCServices/MTLCompilerService.xpc
/bin/launchctl print system/com.apple.MTLCompilerService
```

4. Run your metal program on macOS

5. Intercept the macOS kick

6. Capture the full UAT state

7. Perform the kick-- verify the output pages change

8. Reboot the device

9. Replay the full UAT state: copy the saved pages to memory exactly where they were and populate the UAT root as before

10. Perform the kick and verify the output pages change

11. Deconstruct the capture: start building objects in Python source code until you've reconstructed the whole tree in memory

12. `b step1` until you've constructed the full drm-shim with all functionality

Once you've fully built `drm-shim`, you have enough information to start
writing a kernel driver.

LLM readable (there's way too much slop to read by hand) examples can be found
at [https://github.com/ADevWithAnIdea/m1n1/tree/PUBLIC_neo_agx_bringup](https://github.com/ADevWithAnIdea/m1n1/tree/PUBLIC_neo_agx_bringup)
