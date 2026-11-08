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

## [20261108.1547] - 2026-11-08

### Added

- WSA Tray: an icon next to the clock shows whether WSA runs (grey while it is off, an amber dot
  while it starts or turns off). Its menu starts, restarts and turns off WSA and opens your
  Android apps, Files, Android settings and WSA settings; About shows the Android, kernel and
  root versions and copies the build info for bug reports. "Keyboard and mouse in games" turns
  wsapad on and off in the background, and "Set up game controls…" opens its window; closing
  that window leaves the keys on, and the first time a hint says that F8 in the game turns
  them on. It starts at sign-in, and Run.bat
  starts it after the install. If Windows puts the new icon under the ^ arrow, the tray says so
  once. It speaks every language of WSA (55), right to left in Arabic, Persian and Hebrew.
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
[20261108.1547]: https://github.com/Ognisty321/WSA-AOSP/releases/tag/20261108.1547
[20261102.2135]: https://github.com/Ognisty321/WSA-AOSP/releases/tag/20261102.2135
