# Case study: bringing audiobook support up to the current BookOrbit API

## The problem

The Android client originated from an older BookOrbit ecosystem and its audiobook assumptions no
longer matched the current server. This was not a single broken endpoint.

Audiobook playback crosses several connected concerns:

**manifest → authenticated assets → multipart media → playback state → bookmarks → downloads → offline behaviour → UI**

Updating only the player screen would not have been enough.

## What needed to work

The target workflow was straightforward:

1. Open an audiobook from the BookOrbit library.
2. Start playback.
3. Navigate between chapters.
4. Change playback speed.
5. Leave the application and return later.
6. Resume from the expected position.
7. Download the book.
8. Continue listening without network access.

The implementation had to support the current server instead of preserving assumptions from the
older Android client.

## Investigation

The work started by tracing how the current BookOrbit server exposes audiobook data. Because the
Android API models are handwritten rather than generated from an OpenAPI contract, server changes
must be reflected deliberately in the client.

The relevant layers included:

- audiobook manifest responses;
- authenticated media-asset URLs and byte-range requests;
- multipart file mapping;
- playback-state endpoints;
- bookmark endpoints;
- local download state.

AI-assisted investigation helped trace these contracts through the Android codebase and identify
where older assumptions no longer matched the server.

## Implementation direction

The resulting path uses Media3 / ExoPlayer and supports authenticated streaming, multipart playback,
chapter navigation, playback speed, sleep timer, persistent player state, bookmarks, downloads,
offline playback and synchronization with BookOrbit's playback-state API.

One design goal was to avoid treating online and offline playback as entirely separate products.
Downloaded content should continue to behave like the same audiobook, including its relevant local
state.

## Verification

The project did not treat successful compilation as sufficient verification. The audiobook work was
covered by the wider automated suite and used against the BookOrbit server I rely on personally.
Related work also investigated playback-state write frequency and synchronization behaviour.

The latest recorded project baseline passed 252 JVM unit tests, Android lint and full debug
assembly. These are historical project results; see the
[verification record](../docs/verification.md) for scope and limitations.

## What I learned

The interesting part was not making an audio file play. It was maintaining one coherent user
experience while several layers each had their own representation of the audiobook and its state.

That is a pattern I now look for when troubleshooting software:

**the visible failure may only be the last step in a much longer chain.**

