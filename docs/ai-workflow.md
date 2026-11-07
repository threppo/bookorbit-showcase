# How I used AI on this project

## Honest summary

This was a personal AI-assisted software project. I did not manually build the application from
scratch or present it as commercial Android experience. I used Claude / Claude Code and OpenAI Codex
to work with an existing open-source codebase while I supplied the goals, constraints, examples,
feedback and acceptance decisions.

## Division of work

| My input and decisions | AI-assisted work | How the result was checked |
| --- | --- | --- |
| Defined milestones and explicit scope boundaries | Inspected Android/server code and proposed implementation plans | Compared proposed behaviour with current server routes and my actual workflow |
| Required offline cold-start browsing and stale-but-usable data | Added Room-backed cache flows, migrations and tests | Automated checks plus airplane-mode/everyday-use feedback recorded during development |
| Supplied screenshots and reported visible defects | Located Compose/player/state paths and generated focused fixes | Installed builds were used on my phone; incomplete behaviour was reported again |
| Required server-side providers and indexers to remain authoritative | Kept Book Requests as a client of BookOrbit server APIs | No separate Android scraping or indexer system was added |
| Asked for profiling before optimization | Collected frame, CPU, memory and trace evidence | Before/after measurements were documented rather than assuming improvement |

## A typical iteration

1. **Describe the user problem.** I supplied an observable outcome, constraints and often a
   screenshot or server endpoint.
2. **Inspect before changing.** The AI read the current code, tests and related server contracts.
3. **Make the plan reviewable.** Larger milestones produced a short plan or audit before changes.
4. **Change the smallest relevant path.** I repeatedly protected working playback, downloads,
   server behaviour and unrelated screens from unnecessary rewrites.
5. **Run checks.** AI sessions ran unit tests, lint and debug builds. Selected issues also used a
   connected phone, ADB and performance traces.
6. **Use real feedback.** I reported concrete failures such as an app that would not open, server
   actions incorrectly disabled as offline, Book Requests that could be viewed but not fulfilled,
   filename-like chapter labels and controls placed behind system navigation.
7. **Document the result and limits.** API assumptions, measurements, completed checks and
   unperformed checks were recorded separately.

## What AI was good and bad at

AI was useful for searching a large unfamiliar codebase, tracing contracts across layers, keeping
models and tests aligned, generating implementation changes and turning a reproduction into
instrumented evidence.

It was less reliable when a task was too broad or when success was inferred from compilation alone.
At different points an AI-generated iteration left the app unable to open, and another made Book
Requests visible without completing the release-selection workflow. I did not treat either result as
finished. The visible problem was reported again, the scope was narrowed and the original workflow
was retested.

## Guardrails I used

- BookOrbit remained authoritative for metadata providers, indexers, request ranking and fulfilment.
- Unsupported server behaviour was documented rather than faked in Android.
- Offline failures could not erase cached data or pretend an online write succeeded.
- Working playback, downloads and readers were not rewritten as part of unrelated UI work.
- Destructive actions remained visibly separate from reversible UI actions.
- Automated, device and historical evidence were described as different evidence levels.

## Knowing when to stop

The fork became useful enough for my own everyday needs. I paused substantial feature development
after BookOrbit moved toward official mobile clients because continuing a parallel implementation
would add maintenance work while duplicating the upstream direction.

Stopping is part of the project story: it demonstrates prioritization, not failure. The app remains
my personal client, but it is not presented as a supported commercial product.

## What I am and am not claiming

I can explain the user problems, requirements, reproduction steps, investigation evidence, expected
behaviour, verification results and known limitations. I can also explain how I used AI tools to
work through an unfamiliar product systematically.

I am not claiming that I independently authored the Kotlin implementation, that every feature was
exhaustively tested, or that this personal fork is equivalent to maintaining a commercial Android
product.

