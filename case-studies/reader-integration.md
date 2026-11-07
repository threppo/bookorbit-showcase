# Case study: integrating Readium without confusing local and server reading state

## The problem

The client needed a better EPUB reading path. Readium provides a mature foundation for rendering
and navigation, but integrating it exposed an important state problem: the Android reader and the
BookOrbit server do not represent reading position in exactly the same way.

A Readium locator contains information meaningful to the local reader. BookOrbit's API exposes its
own progress representation. Treating one as if it were simply the other would create a fragile
synchronization model.

## Desired behaviour

The reader should support EPUB opening, reliable local resume, typography controls, bundled fonts,
direct text-size adjustment, offline reading and useful server-side progress synchronization.

The user should not need to understand that several representations of “where I am in the book”
exist underneath the interface.

## Design decision

Readium locator JSON remains local Android state and is used for exact local resume. Server
synchronization continues to use the progress fields BookOrbit actually supports.

The important rule is:

**do not send local Readium state to the server while pretending it is a server-compatible location format.**

This preserves richer local information without inventing an API contract that does not exist.

## Reader experience

The resulting EPUB path includes Readium-based rendering, persisted local position, typography and
theme controls, Readerly and Literata fonts, direct pinch-to-resize text and offline behaviour. The
existing PDF path remains separate instead of forcing unrelated formats through the EPUB reader.

## Verification

Reader work received automated and physical-device verification. Recorded device checks included
direct two-finger text resizing while confirming that the native PDF path remained unchanged. The
broader historical baseline also passed 252 JVM tests, Android lint and full debug assembly.

## What I learned

Sometimes two systems represent the same user concept differently for legitimate reasons. The
correct solution is not always to normalize everything into one format.

Separating **exact local reader state** from **server-supported cross-device progress** made the
system easier to reason about and avoided a false equivalence between two different data models.

