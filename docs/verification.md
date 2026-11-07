# Verification record

This document summarizes the strongest verification evidence retained from development of my
BookOrbit Android fork. It does not imply that every feature was exhaustively tested on every
Android version or device.

The project is no longer under active feature development and is currently used as my personal
everyday BookOrbit client.

## Latest recorded automated baseline

The latest documented full debug baseline passed:

- **252 JVM unit tests**
- **0 JVM test failures**
- Android `lintFullDebug`
- `assembleFullDebug`

The Android project targets API 36 and supports API 26+.

These are recorded development results, not checks rerun while creating this showcase repository.

## Physical-device verification

Selected reader behaviour was checked on a physical Android device. The recorded verification
included:

- opening EPUB content through the Readium path;
- direct two-finger text resizing;
- persistence of the intended reader behaviour;
- confirming that the separate native PDF path remained unchanged.

This is not exhaustive device-matrix testing.

## Areas with focused verification work

### Audiobooks

- current audiobook manifest/API behaviour;
- authenticated media assets;
- multipart playback;
- playback-state synchronization;
- bookmarks;
- downloads and offline playback.

### Persistence and offline behaviour

- Room cache behaviour and migration chains;
- server/account ownership of durable state;
- pending writes;
- WorkManager recovery.

### Reader

- Readium integration;
- local locator persistence;
- typography and bundled reader fonts;
- distinction between local Readium state and BookOrbit server progress.

### Performance and maintenance

Focused investigations were also performed for collapsed-series rendering performance, graphics
memory, cold-start network behaviour and general codebase health. These answer narrower questions
than a complete application qualification test.

## Everyday-use verification

The application became my normal Android client for my personal BookOrbit server. This provides
useful practical evidence for library browsing, book details, audiobook playback, EPUB reading,
offline/download functionality, requests and management operations.

Everyday use is not a substitute for formal testing, but it was important for finding problems that
would not necessarily appear in isolated automated checks.

## What is not claimed

This showcase does **not** claim:

- exhaustive testing across Android devices or versions;
- production support for arbitrary BookOrbit installations;
- independent manual verification of every automated test;
- that I personally authored all implementation code;
- continued active feature development.

The project was AI-assisted, with implementation work performed using Claude / Claude Code and
OpenAI Codex. My contribution centered on product direction, requirements, problem reporting,
constraints, review, hands-on use and acceptance of user-visible behaviour.

## Current status

The fork is **feature-complete for my personal use**. Active feature development stopped because
the upstream BookOrbit project moved toward official mobile clients, including an iOS application
and a planned Android application.

A serious compatibility regression affecting my own use could still justify a maintenance fix,
but no substantial new feature work is currently planned.

## Verification philosophy

Throughout the project I tried to keep these states separate:

**implemented ≠ compiled ≠ automated tests passed ≠ physically verified ≠ proven in every environment**

For portfolio purposes I would rather describe a narrower result accurately than imply stronger
verification than the available evidence supports.

