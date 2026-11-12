<!--
How to keep this file:
- Every change users notice goes under "Unreleased" as it lands, in plain words: what changed
  for them, with the number that shows it when there is one.
- At a release, the section of the build (else "Unreleased") is the "What's new" of its GitHub
  release; the first release has none. Once it is out, rename "Unreleased" to
  `[BUILD] - YYYY-MM-DD`, add an empty "Unreleased" above it and a link line at the bottom.
- Headings: Added, Changed, Improved, Fixed, Security. Leave out the empty ones.
-->

# Changelog

What changed from one build to the next. The build number is the date and time of the Android
build (`YYYYMMDD.HHMM`); a `-rN` suffix marks the same Android image with new Windows host files.

## [Unreleased]

## [20261112.1628] - 2026-11-12

### Added

- A build without Google apps. Every build now comes as `GApps` (Google Play services and the
  Play Store, as before) and `NoGApps` (the same Android without them)
  ([issue 1](https://github.com/Ognisty321/WSA-AOSP/issues/1)).

### Changed

- A build no longer comes as one ZIP per root. Both variants come without a root and with every
  root in their folder `Root`: run `Root\Root.bat` and pick SukiSU-Ultra, KernelSU, KernelSU-Next
  (each with SuSFS), SukiSU-Ultra without SuSFS, Magisk, Kitsune Mask or Magisk Alpha. Your apps
  and data stay, `Root\Root.bat NoRoot` takes the root out again, and an update with
  `Run.bat -KeepData` keeps the root you had. A release is two downloads instead of eight.

### Improved

- wsapad (the keyboard and mouse as a controller, or as touches) costs next to nothing while it
  waits in the background: under 0.1 % of a processor core and about 10 wake-ups a second instead
  of 0.9 % and 740, 10 threads instead of 50, and 4 MB of memory instead of 96 MB. It no longer
  keeps a Direct3D device for a window nobody sees, starts no `adb` while WSA is not running, and
  is told when the game comes to the front instead of looking for it all the time.
- wsapad is between the keyboard and Windows only while a game of WSA with a profile is in front.
  Other windows, Windows games among them, run without it in between. In a game, Win, Alt+Tab,
  Alt+F4, Ctrl+Esc and Win+Shift+S work also when the profile uses Shift, Ctrl, Alt, Tab or Esc.
- The window of wsapad: View, Menu and the Xbox button (−, + and Home on the Switch pad) have
  their labels above the picture, next to the buttons; the keys of a button stay on one line
  where there is room; the line to a button no longer runs across its neighbour. Dialogs open in
  the middle of the window. The window fits a 1366 × 768 screen, and Settings scrolls.
- wsapad's window looks like WSA Settings, next to which you open it: Mica under the whole
  window, the accent shades of Windows (the same colour as the bar in WSA Settings), the name in
  the title bar, and Settings as cards with an icon, a name and what each setting does.
- The card in wsapad's window tells a touch screen from a controller, and when an app of WSA is
  open but WSA does not let `adb` in, it says so and opens WSA's settings, where developer mode
  is turned on.

### Fixed

- A window that you close no longer loses its content from the bottom up before it fades out.
  Android played its own closing animation in the window while Windows closed it; now only
  Windows animates the window ([issue 1](https://github.com/Ognisty321/WSA-AOSP/issues/1)).
- Copying text or a picture, in Android or in Windows, no longer brings up Android's clipboard
  preview over the desktop ([issue 1](https://github.com/Ognisty321/WSA-AOSP/issues/1)).
- `Run.bat` without `-KeepData` starts with fresh Android data again. It made the backup, but
  left the data of the WSA it replaced in place, so Android started on the old data.
- "Local network access" (WSA Settings, Advanced settings, Experimental features) works. With
  it on, Android had no network at all; now it takes the address of the Windows PC in your
  network. It is off by default, and what Windows does not pass in that mode stays so: `ping`
  gets no answer, and while NordVPN is connected on Windows, names do not resolve (they do
  with the switch off).
- Where the windows of two Android apps overlap, a click or a touch (also from wsapad) in the
  shared part goes to the window in front. It went to the window that had been in front
  before, and Windows then brought that one to the front, so the two windows swapped places.
- wsapad takes keys and the mouse only while its controller or touch screen exists in Android.
  With WSA's developer mode off, F8 took them and they went nowhere.
- wsapad: a key held while a game came to the front, or let go after it left, no longer stays
  down in Windows or in the game.
- wsapad: AltGr no longer presses what a profile has on the left Ctrl.
- wsapad, touch controls: a tap pressed again during the one before is no longer lost; a notch
  of the mouse wheel on a repeated tap is one tap; zooming and scrolling with the wheel go on
  when the fingers reach the end of their way; in a portrait game the camera finger stays inside
  the game; when the game's window moves, the fingers are lifted at once and a held joystick goes
  down again in the new place.
- `wsapad.ini`: a `;` in the name of a game, in a window title or in the path of adb is no longer
  the start of a comment; a repeated tap keeps both its count and its rate; an option with a value
  out of range says the range.
- wsapad: the keys that turn the mapping on and free the mouse cannot be keys of a game any more,
  also in a game read from a file or from a BlueStacks keymap. Choosing one of them anew in
  Settings takes it out of the games that used it.
- wsapad: importing a BlueStacks keymap into a game that already has touch controls asks before
  it replaces them. A renamed game keeps its picture. A settings file named on the command line is
  also the one changes are saved to.
- wsapad: the keys shown over a game no longer cover each other where two controls are in one
  place, such as a zoom on a button. The second key stands below the first.

## [20261111.0023] - 2026-11-11

### Improved

- ARM64 apps and games need less processor time. The translator kept every register exact before
  each memory access, in all code, so that a crash handler of the app could go on. Now it does so
  only for code that needs it: code made at run time, Mono and .NET, Go, and any library in which
  it turns out to be needed. Hill Climb Racing 3 uses a quarter less processor time while it
  starts (59-60 s instead of 77-82 s in its first 105 s), and plain computation in ARM code is up
  to twice as fast (matrix multiplication 14.4 instead of 6.6 GFLOPS, an SQLite test 1346 instead
  of 2200 ms).
- ARM64 code written in Go reads the clock ten times faster (`time.Now()` takes 86 ns instead of
  897 ns), and a Go library inside an app is told which processor instructions the translator
  has: its AES-GCM runs at about 1900 MB/s instead of 48 MB/s.

### Fixed

- VPN apps start. Android closed their "Connection request" window at once, because WSA's system
  app forbids VPN settings at every start. Android now asks you before an app may run a VPN, as
  on a phone. HTTP Toolkit's "Android Device via ADB" mode connects as well.
- Android no longer stops because Windows read old memory of the virtual machine. The Windows
  processes that serve the shared folders and the graphics of WSA keep views of the machine's
  memory, and a view went stale when Android gave memory back to Windows and took it again. About
  once in ten days of use the result was that all shared folders stopped and apps hung on their
  files, or that the picture stood still, until WSA was restarted; data copied to or from a
  Windows folder could also come out wrong. Microsoft's WSA has the same fault. The kernel now
  gives those processes only memory that it never frees.
- ARM64 programs written in Go run; they crashed when they started. An app with a Go library
  that is built for debugging no longer dies at its first SHA-256, for example at its first
  HTTPS connection.
- Root variants: Frida attaches to apps and starts them, and so does HTTP Toolkit's "Android App
  via Frida" mode. Both failed with "I/O error".

## [20261108.1547-r2] - 2026-11-08

This build replaces 20261108.1547, which was out for two hours the same day. It has the same
Android image, kernels and root files. Only WSA Tray changed: its amber dot went away when the
virtual machine came up, about 20 s before Android was ready, and now stays until Android has
fully started. In WSA's "Partially running" mode (the default with 16 GB of memory or more) the
icon is grey and says "partially running" while the machine waits for an app. If WSA is turned
off from elsewhere, the stand-by machine too, the dot shows until it is off.

### Added

- WSA Tray: an icon next to the clock shows whether WSA runs (grey while it is off or partially
  running, an amber dot while it starts or turns off). Its menu starts, restarts and turns off
  WSA and opens your Android apps, Files, Android settings and WSA settings; About shows the
  Android, kernel and root versions and copies the build info for bug reports. "Keyboard and
  mouse in games" turns wsapad on and off in the background, and "Set up game controls…" opens
  its window; closing that window leaves the keys on, and the first time a hint says that F8 in
  the game turns them on. It starts at sign-in, and Run.bat starts it after the install. If
  Windows puts the new icon under the ^ arrow, the tray says so once. It speaks every language
  of WSA (55), right to left in Arabic, Persian and Hebrew.
- wsapad has a window: pick a game profile, choose whether Android sees an Xbox, PlayStation
  (DualSense) or Switch Pro controller, and click a button, stick or trigger on the picture of
  that controller to bind keys and mouse buttons to it. Mouse sensitivity, dead zone, curve,
  walk speed and key combos (e.g. LB+Y) are set there too; changes apply at once and are saved
  to `%APPDATA%\wsapad\wsapad.ini`. Games show the button glyphs of the chosen controller,
  and so does the window, which draws each controller with its real shape and buttons: Xbox
  Series, DualSense and Switch 2 Pro.
- wsapad also plays games that know only touch, like BlueStacks does: keys put fingers on the
  game's screen (taps, repeated taps, a WASD joystick, a camera turned by the mouse, aimed
  skills with cancel, swipes, zoom, scroll and sequences), each key either a controller button
  or a touch control. In the window's "Touch" view you place them on a picture of the game
  and drag them around; the mouse can also stay a free cursor whose clicks touch the game. The
  keys show on the game while the mapping is on. Keymaps of BlueStacks 5 (`.cfg`) that you
  have import in one click; a profile can be picked by the game's Android package, and
  `pad = none` leaves out the controller for touch games.

### Changed

- `Run.bat -KeepData` over 20261102.2135 removes the registration of WSA and registers it again,
  because the package has a new app (the tray). Your apps and data stay, but the Android apps
  leave the Start menu until WSA starts the next time.
- Android now pauses apps you leave in the background, usually a minute or two after you minimize
  them, and they continue at once when you come back. The freezer that does this could not work
  in WSA before.
- The root variants come with the newest versions: SukiSU-Ultra v4.3.0-r2, SuSFS v2.4.0,
  KernelSU 32718 (manager v3.3.1-38) and KernelSU-Next 33486 (manager v3.4.2-131). Magisk v31.1,
  Kitsune Mask and Magisk Alpha have no newer release.

### Improved

- Games that set no frame rate of their own answer much faster: from the start of a frame to the
  screen, 50 ms became 11-14 ms in Subway Surfers, 14-18 ms in Hill Climb Racing 3 and 18 ms in
  Angry Birds 2 on a 280 Hz monitor, and 41 ms became 18-25 ms on a 144 Hz monitor, at the same
  frame rate. Fewer than two frames in a hundred land one refresh (3.6 ms) off the even rhythm.
  Other windows keep the full refresh rate of the monitor while such a game is on screen, instead
  of dropping to 56 Hz with it.
- Games that draw in a loop of their own react to a click in 15-17 ms instead of 65 ms: the next
  frame now starts at the display's rhythm, as in other apps, so frames no longer pile up waiting
  for the screen. Right after a game starts or stutters, frames that piled up go out within a
  quarter of a second, where they used to add 12 ms to every click for up to 17 seconds.

### Fixed

- Minimized apps and games stay as you left them. WSA's memory cleanup closed them within 10
  minutes, or at once when you started another app from Windows, and they started anew when you
  brought the window back. Now it only clears apps whose windows you closed.
- WSA no longer gets stuck when it shuts down after a session with games. The virtual machine stayed
  behind (`vmmemWSA`), WSA would not start again and only a Windows restart helped. It happened in
  about half of the test shutdowns after games, and in none of 12 with the fix.
- Every start of WSA showed "Configured virtual_keyboard, layout set to English (US)", whatever
  keyboard Windows uses. The message is gone; you still type with your Windows layout.

## [20261102.2135] - 2026-11-03

First build on GitHub. What it contains is in the [README](README.md#whats-inside).

[Unreleased]: https://github.com/Ognisty321/WSA-AOSP/releases
[20261112.1628]: https://github.com/Ognisty321/WSA-AOSP/releases/tag/20261112.1628
[20261111.0023]: https://github.com/Ognisty321/WSA-AOSP/releases/tag/20261111.0023
[20261108.1547-r2]: https://github.com/Ognisty321/WSA-AOSP/releases/tag/20261108.1547-r2
[20261102.2135]: https://github.com/Ognisty321/WSA-AOSP/releases/tag/20261102.2135
