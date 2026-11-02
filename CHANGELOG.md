<!--
How to keep this file:
- Every change users notice goes under "Unreleased" as it lands, in plain words: what changed
  for them, with the number that shows it when there is one.
- At a release, rename "Unreleased" to the build number and date, add an empty "Unreleased"
  above it and a link line at the bottom. The section is the body of the GitHub release:
    awk '/^## \[BUILD\]/{f=1;next} /^(## )?\[/{f=0} f' CHANGELOG.md > notes.md
    gh release create BUILD --prerelease --notes-file notes.md \
      WSA-AOSP17_BUILD_*.zip WSA-AOSP17_BUILD_*_GPL-sources.tar.xz WSA-AOSP17_BUILD_SHA256SUMS.txt
- Headings: Added, Changed, Improved, Fixed, Security. Leave out the empty ones.
-->

# Changelog

What changed from one build to the next. The build number is the date and time of the Android
build (`YYYYMMDD.HHMM`); a `-rN` suffix marks the same Android image with new Windows host files.

## [Unreleased]

First build on GitHub. What it contains is in the [README](README.md#whats-inside).

[Unreleased]: https://github.com/Ognisty321/WSA-AOSP/releases
