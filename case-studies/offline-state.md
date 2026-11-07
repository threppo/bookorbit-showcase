# Case study: offline-first state without confusing servers, accounts and devices

## The problem

A self-hosted mobile client cannot assume that its server will always be reachable. Simply caching
every response is not enough either.

BookOrbit Android deals with several kinds of state:

- library data that can safely be cached;
- mutable user state;
- pending writes;
- downloads;
- reading and playback progress;
- information belonging to a particular server and account.

If those categories are mixed together, an apparently harmless offline feature can cause more
serious problems after changing account or server.

## Desired behaviour

The app should remain useful during temporary connectivity loss. Previously loaded books, details,
authors, series, collections and dashboard content should stay visible, and downloaded media should
remain usable.

At the same time, data belonging to one BookOrbit server/account must not silently become state
belonging to another.

## Approach

Room provides local structured storage. The client maintains offline-first caches for important
browsing data while separately handling state that needs stronger ownership rules. Mutable local
state is scoped to the appropriate BookOrbit server and account.

For operations that cannot finish immediately, WorkManager supports durable background recovery
rather than assuming a network request will complete during the original UI session.

This distinction matters:

**cached information** and **pending user intent** are not the same thing.

Losing a cached author list may be inconvenient. Losing or misapplying a pending user action can
change actual application state.

## Migration and recovery

Persistent state also creates an upgrade problem. Room schema changes need explicit migrations so
an updated app does not solve a feature problem by destroying previous user data. Development work
therefore included migration and recovery checks rather than relying only on fresh installs.

## Verification

Offline and persistence work was covered by the wider JVM suite. Project documentation also records
focused work around Room migration recovery, server/account ownership, durable pending writes and
WorkManager recovery. Everyday use made failures visible in the workflows the implementation was
meant to support.

## What I learned

Offline support is not a toggle. It forces the product to answer:

- Which data is authoritative?
- Which data is merely cached?
- Who owns this state?
- What survives restart?
- What happens when the server changes?
- What happens when an operation stops halfway through?

Those questions transfer well beyond Android development.

