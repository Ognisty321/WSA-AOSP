<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="WSA-AOSP: Microsoft's WSA host with Android 17, a Linux 6.18 kernel and graphics rebuilt from source. New: the host graphics DLLs, Google Play and Berberis, Android 17 and Linux 6.18 with the root of your choice. Kept from Microsoft's WSA 2407: the Windows host and the vendor partition.">
</p>

<p align="center">
  <a href="https://github.com/Ognisty321/WSA-AOSP/releases"><b>Download</b></a> ·
  <a href="#install">Install</a> ·
  <a href="CHANGELOG.md">Changelog</a> ·
  <a href="#known-limitations">Limitations</a> ·
  <a href="https://github.com/Ognisty321/WSA-AOSP/issues">Report a problem</a>
</p>

<p align="center">
  <a href="https://github.com/Ognisty321/WSA-AOSP/releases"><img src="https://img.shields.io/github/v/release/Ognisty321/WSA-AOSP?include_prereleases&label=build&color=3DDC84" alt="Latest build"></a>
  <a href="https://github.com/Ognisty321/WSA-AOSP/releases"><img src="https://img.shields.io/github/downloads/Ognisty321/WSA-AOSP/total?color=6CA6E8" alt="Downloads"></a>
</p>

> [!WARNING]
> Unofficial test builds. Not affiliated with, endorsed or supported by Microsoft or Google.

Microsoft ended support for Windows Subsystem for Android on March 5, 2025, at Android 13.
WSA-AOSP keeps
Microsoft's Windows side of WSA (Android apps as Windows windows, Start menu entries, clipboard,
notifications, shared folders) and its vendor partition, and replaces Android, the kernel and
the graphics libraries.

<p align="center">
  <img src="./assets/readme/proof.svg" width="100%" alt="adb shell on a running build reports Android 17, kernel 6.18.33-android17, SELinux enforcing, Vulkan 1.3.261 on the host NVIDIA GeForce RTX 5070 Ti through gfxstream and the OpenGL ES Android Extension Pack. Measured against the upstream code of each part: Genshin shader warm-up 45 min to 2.5 min, Android to Windows graphics call 13 µs to 1.2 µs, Hill Climb Racing 3 hitch 450 ms to 260 ms, 4 to 8 times less ETC2 and ASTC texture memory on integrated GPUs and GPUs under 6 GB.">
</p>

## What's inside

- **Android 17** built from AOSP (`android-17.0.0_r1`): a `user` build signed with its own keys,
  SELinux enforcing, with the Android security bulletin fixes (patch level in each release).
- **Linux 6.18** from the Android Common Kernel instead of Microsoft's 5.15, with the root you
  pick ([variants](#variants)): **SukiSU-Ultra**, **KernelSU** or **KernelSU-Next** built in, with
  **SuSFS** hiding the root from apps; **Magisk**, **Kitsune Mask** or **Magisk Alpha**; or none.
- **Graphics built from source**: gfxstream (upstream, with a layer for the WSA host) and Dozen
  from Mesa replace Microsoft's `gfxstream_backend.dll` and `vulkan_dzn.dll`, and Android's
  Vulkan driver comes from Mesa. Apps that need it get **OpenGL ES 3.2 with AEP** through ANGLE,
  so the Play Store offers games such as Genshin Impact, and **Vulkan 1.3** runs on the host GPU.
- **Google Play services and the Play Store** in the image.
- **ARM apps**: 64-bit through Berberis, Google's translator from the Android emulator image;
  32-bit through Intel's Houdini 14.
- **`wsapad.exe`** maps keyboard and mouse to an Xbox controller for games that only take a pad
  (F8 turns the mapping on and off).

## Compared with WSABuilds

[WSABuilds](https://github.com/MustardChef/WSABuilds) is Microsoft's WSA 2407.40000.4.0 with
Google Play (MindTheGapps) and, if you pick it, Magisk added. Here Microsoft's WSA 2407 gives the Windows host, the vendor partition, the
initrd and its own Android apps; Android, the kernel and two graphics DLLs are new.

| | WSABuilds | WSA-AOSP |
|---|---|---|
| Android | 13 (Microsoft) | **17** (AOSP) |
| Kernel | 5.15.104 (Microsoft) | **6.18** (Android Common Kernel) |
| Root | Magisk (KernelSU in older builds) | **SukiSU-Ultra, KernelSU, KernelSU-Next** (with SuSFS), **Magisk, Kitsune Mask, Magisk Alpha** |
| OpenGL ES | 3.0 | **3.2 + AEP** |
| Vulkan ("Vulkan driver" System / D3D12) | 1.3 / 1.1 | 1.3 / **1.2** |
| Host graphics DLLs | Microsoft's | gfxstream and Dozen built from source |
| ARM64 apps | Houdini (Intel Bridge Technology) | **Berberis** |
| Windows | 10 x64, 11 x64 and arm64 | 11 x64 only |
| Variants | seven per platform (with or without GApps, Magisk, Amazon Appstore) | eight, all with Google Play: one per root, and one without |

## Variants

Every build comes as one ZIP per root, with the same Android 17 and Google Play in each:

| `WSA-AOSP17_<build>_….zip` | Root | You install yourself |
|---|---|---|
| `NoRoot` | none | |
| `SukiSU-SUSFS` | SukiSU-Ultra with SuSFS | the SukiSU manager from the ZIP |
| `SukiSU` | SukiSU-Ultra | SukiSU-Ultra's release manager, from the ZIP |
| `KernelSU-SUSFS` | KernelSU with SuSFS | the KernelSU manager and the SuSFS module, from the ZIP |
| `KernelSU-Next-SUSFS` | KernelSU-Next with SuSFS | the KernelSU-Next manager and the SuSFS module, from the ZIP |
| `Magisk` | Magisk | the Magisk app, from the ZIP |
| `KitsuneMask` | Kitsune Mask | the Kitsune Mask app, from the ZIP |
| `MagiskAlpha` | Magisk Alpha | the Magisk Alpha app, from the ZIP |

SukiSU-Ultra, KernelSU and KernelSU-Next are built into the kernel, and SuSFS hides the root from
apps. Magisk and its forks start from `Tools\initrd.img` through a hook in the kernel, as WSA has no
boot image to patch. The `README.txt` of each ZIP names the exact versions and the steps for its
root.

## Install

You need Windows 11 x64, 8 GB of RAM or more (16 GB recommended), CPU virtualization turned on
in the BIOS/UEFI, and 2.7 GB of disk space for the extracted build plus room for your Android
apps and data. Uninstall a WSA from the Microsoft Store first. A developer-mode WSA (e.g. from
WSABuilds) is replaced: its Android data is backed up, its folder stays where it is.

1. Download the ZIP of your [variant](#variants) from
   [Releases](https://github.com/Ognisty321/WSA-AOSP/releases) and, if you like, compare
   `Get-FileHash .\WSA-AOSP17_*.zip` with its line in `WSA-AOSP17_<build>_SHA256SUMS.txt`.
2. Extract it to a folder that stays, e.g. `C:\WSA`. WSA runs from there.
3. Turn on **Developer Mode** in **Settings > System > Advanced** (before Windows 11 25H2:
   **Settings > System > For developers**).
4. Run `Run.bat` and allow administrator rights. It installs the frameworks from `Dependencies`,
   offers to turn on "Virtual Machine Platform" (restart Windows and run it again) and registers
   WSA. Log: `%LOCALAPPDATA%\WSA-AOSP17-backups\install.log`.
5. Start **Windows Subsystem for Android** from the Start menu. The Play Store and your Android
   apps then appear in the Start menu.

Vulkan 1.3 and OpenGL ES 3.2 need **"Vulkan driver" = System** in WSA Settings (restart WSA
after changing it). With D3D12 apps get Vulkan 1.2 and OpenGL ES 3.0; with None, OpenGL ES 3.0
and no Vulkan.

### Updating

What each build changes is in the [changelog](CHANGELOG.md). Extract the new build to a new
folder, run `.\Run.bat -KeepData` in a terminal there to keep your apps and data, then delete the
old folder. Without `-KeepData` the installer backs up the Android data to
`%LOCALAPPDATA%\WSA-AOSP17-backups` and starts fresh, as it must when coming from Microsoft's WSA
or WSABuilds. Going back to an older build with the same data is not supported.

Another variant goes in the same way. Between two different roots leave out `-KeepData`: the
modules and settings of one root (in `/data/adb`) are not meant for another.

## adb

1. Turn on **WSA Settings > Advanced settings > Developer mode**.
2. Get [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools).
3. Start any Android app, then run `adb connect 127.0.0.1:58526` and allow debugging in the
   Android window the first time.

## Google Play

Sign in to the Play Store with your Google account. This Android is not Google certified (Play
Store > Settings > About > Play Protect certification: "Device is not certified"). If Google
blocks the sign-in or the Play Store stops working:

1. Read the GSF ID with [adb](#adb):
   ```
   adb shell "dumpsys activity service com.google.android.gms/.gcm.GcmService | grep DeviceID"
   ```
2. Register that number at <https://www.google.com/android/uncertified>.
3. Wait from a few minutes to a few hours, clear the data of the Play Store only (not of Google
   Play services) and restart WSA.

This is needed once per installation, and again only after the WSA data is cleared.

## Root

The app of the root is not preinstalled: install it from the WSA folder with
[adb](#adb), e.g. `adb install "C:\WSA\Magisk-v31.1.apk"`. The `README.txt` there has the exact
file names and steps.

- **SukiSU-Ultra with SuSFS**: the manager shows "Working" and the SuSFS version. Use the one from
  the ZIP: its `ksud` supports SuSFS on x86_64 and the kernel trusts its signature; the stock
  manager from GitHub does not handle SuSFS here.
- **SukiSU-Ultra**: the stock manager of SukiSU-Ultra's release.
- **KernelSU, KernelSU-Next with SuSFS**: the manager from the ZIP, which upstream's CI built from
  the same commit as the kernel's driver (a manager that does not match asks for an update and
  hides Superuser and Modules). SuSFS's tool and settings come as `ksu_module_susfs-…-x86_64.zip`:
  `adb push` it to `/sdcard/Download` and install it under Modules.
- **Magisk, Kitsune Mask, Magisk Alpha**: on the first start the app asks for additional setup
  and restarts Android; then Zygisk can be turned on. Its Install and Uninstall buttons do not
  apply (there is no boot image); a newer version comes with a newer build of this port.

KPM modules of SukiSU-Ultra do not work on x86_64.

## Uninstall

**Settings > Apps > Installed apps > Windows Subsystem for Android > Uninstall**, then delete the
folder.

`Restore.bat` undoes the last install: it registers the WSA folder that was registered before it
again and puts back the newest Android data backup from `%LOCALAPPDATA%\WSA-AOSP17-backups`. An
install with `-KeepData` makes no backup, so after one name the backup and the folder that belong
together: `.\Restore.bat -Backup <folder> -PreviousInstall <folder>`.

## Known limitations

- Test builds, tested on one PC: AMD Ryzen 7 7800X3D with an NVIDIA GeForce RTX 5070 Ti. AMD and
  Intel GPUs and Intel CPUs should work but have not been checked.
- Windows 11 x64 only, and every variant has Google Play.
- Not Google certified: apps and games that check Play Integrity may refuse to run or to log in
  (Pixel Gun 3D starts, but its server rejects the login).
- Apps from Microsoft's WSA or WSABuilds have to be installed again; their data stays in the
  backup.
- Task Manager may show a huge, growing "Dedicated GPU memory" for WsaClient. It is a Windows
  accounting quirk; the real use is a few hundred MB.

## Reporting problems

Open an [issue](https://github.com/Ognisty321/WSA-AOSP/issues) with the build number (top of
`README.txt` in the WSA folder), your Windows version, CPU and GPU, and what happened. Attach
`%LOCALAPPDATA%\WSA-AOSP17-backups\install.log` for installation problems and the output of
`adb logcat -d > logcat.txt` for problems in Android.

## Source code and licenses

The build system of this port is not public yet. Every release comes with
`WSA-AOSP17_<build>_GPL-sources.tar.xz`, the source of everything in its ZIPs under the GPL or
another copyleft license:

- the Linux kernels: the exact tree of the kernel without root, with the port's changes as
  patches, and for each root the patches that add its driver and SuSFS; the configuration of
  every kernel and the files that build them;
- each root at the commit its app was built from: SukiSU-Ultra (with the port's changes to its
  `ksud`), KernelSU or KernelSU-Next with the SuSFS module for x86_64, or Magisk, Kitsune Mask or
  Magisk Alpha with the program that starts it from the initrd;
- the copyleft projects of Android 17 in the images (libcore, e2fsprogs, iptables, ...), at the
  commits the images were built from.

<details>
<summary>Where each part comes from</summary>

| Part | Comes from | License |
|---|---|---|
| Windows host, initrd, vendor partition, Microsoft's Android apps | WSA 2407.40000.4.0 | Microsoft, proprietary |
| `dxil.dll` | Windows SDK | Microsoft, proprietary |
| Android 17 system | AOSP | Apache 2.0 and others (Android Settings > About > Legal information) |
| Linux kernel, kernel part of the root | Android Common Kernel, SukiSU-Ultra, KernelSU, KernelSU-Next | GPL-2.0 |
| SuSFS | susfs4ksu | GPL-3.0-or-later (the project's license) |
| Root managers, `ksud` | SukiSU-Ultra, KernelSU, KernelSU-Next | GPL-3.0-or-later |
| Magisk apps | Magisk, Kitsune Mask, Magisk Alpha | GPL-3.0 |
| `gfxstream_backend.dll` | gfxstream | Apache 2.0 (some parts MIT) |
| `vulkan_dzn.dll`, Android Vulkan driver | Mesa | MIT |
| ANGLE | ANGLE | BSD-3-Clause |
| Berberis (ARM64 translator) | Google's Android 17 emulator image | Apache 2.0 source; the image is under the Android SDK License Agreement |
| Google Play services, Play Store | Google's Android 17 emulator image | Google, proprietary (Android SDK License Agreement) |
| Houdini 14 (32-bit ARM translator) | Google Play Games Developer Emulator | Intel, proprietary |

</details>

## Credits

Microsoft for WSA. Google for AOSP, [gfxstream](https://android.googlesource.com/platform/hardware/google/gfxstream),
[ANGLE](https://chromium.googlesource.com/angle/angle) and
[Berberis](https://android.googlesource.com/platform/frameworks/libs/binary_translation).
[Mesa](https://gitlab.freedesktop.org/mesa/mesa) for Dozen.
[SukiSU-Ultra](https://github.com/SukiSU-Ultra/SukiSU-Ultra), [KernelSU](https://github.com/tiann/KernelSU),
[KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next) (with SuSFS by
[pershoot](https://github.com/pershoot/KernelSU-Next)), [SuSFS](https://gitlab.com/simonpunk/susfs4ksu),
[Magisk](https://github.com/topjohnwu/Magisk), [Kitsune Mask](https://github.com/Jordan231111/KitsuneMagisk)
and [Magisk Alpha](https://github.com/vvb2060/Magisk) for the root.
[WSABuilds](https://github.com/MustardChef/WSABuilds) for keeping WSA builds coming after
Microsoft stopped, and [MagiskOnWSALocal](https://github.com/LSPosed/MagiskOnWSALocal) for the
tooling they are made with.
