# NaviSarthi

Private Android navigation companion built on [Northstar](https://github.com/adityadasika21/NorthStar).

Targets Android 14+ and the existing Royal Enfield Tripper TFT connection. Intended motorcycle coverage: 2025+ Guerrilla 450, Himalayan 450, and Bear 650; each model/firmware requires hardware qualification. No console software changes or installation.

## Current status

Version 0.2.0-dev01 adds stale-location rejection, monotonic position/video timing, encoder recovery and foreground-lock fixes on top of the 0.1.0 branding foundation. Debug build and JVM regression tests pass; screen-off and decoder performance still require bike qualification. Multi-stop navigation is planned for 0.3.0. Upstream APK update checks are disabled. NaviSarthi installs separately as `com.navisarthi.app`; existing Northstar data is not automatically migrated. Optional Firebase configuration must belong to this new application ID.

## Build

On Windows run `./gradlew.bat :app:assembleDebug`. Android SDK and a compatible JDK are required. Optional Firebase and release signing use your own configuration; do not distribute upstream credentials.

## Roadmap

See [version plans](<version plans/README.md>) for requirements, version milestones, and acceptance criteria. Plans are updated with every feature addition.

## Attribution

Derived from Northstar, copyright 2026 Aditya Dasika, under Apache-2.0. See LICENSE and NOTICE for preserved upstream and third-party attribution. Independent project, not affiliated with Royal Enfield or Google.
