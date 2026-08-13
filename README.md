# COUNTERENTRY

**A knowledge structure that notices when it stops being true.**

Records about software go stale silently. A note says a dashboard requires admin
access; the permission check returns `true` for everyone and always has. Nothing
crashes. Error monitoring watches for failure, and this isn't a failure — it's a
function working perfectly while being wrong.

The only thing that catches it is putting two independent records of the same
system side by side and asking whether they agree. This is a specification for
doing that automatically.

The name is the mechanism: **every claim that matters carries a second,
independent entry, and the job is finding the pairs that stopped agreeing.**

---

## Status — read this before anything else

**What exists:** a manual practice — a person, an agent, and markdown and JSON
files. Four findings produced by it.

**What does not exist:** the scheduler, the precision ledger, counter-entry
enforcement, premise propagation, bi-temporal storage, and the dormancy
machinery. All specified. None built.

`COUNTERENTRY.md` §2 is the authoritative list and is kept honest. Nothing
generated from this spec should describe an unbuilt component as existing.

---

## What's here

| File | What it is |
|---|---|
| `COUNTERENTRY.md` | The specification. Vocabulary, passes, invariants, status, prior art. Vendor-neutral — nothing depends on a particular model or harness. |

Drop `COUNTERENTRY.md` at the root of a project. Any agent harness that reads a
project context file will pick it up. If yours expects a specific filename,
reference this one from it rather than duplicating the content, so there is only
ever one copy to keep true.

---

## Credit where it belongs

The foundation is **Andrej Karpathy's LLM Wiki** pattern (gist, 4 April 2026):
compile once rather than retrieve per query, let the model own the bookkeeping,
keep everything in plain versioned files. If you haven't read it, read it first —
it's one page and it's the best statement of the idea anyone has written.

Contradiction-hunting is not an addition here; his *lint* operation already looks
for it. What differs is what triggers a check, what it's checked against, and
what governs the output. Structurally this is his pattern with five insertions.
Remove them and the LLM Wiki is what remains.

§11 of the spec is the full prior-art accounting, including what has already been
shipped by others and what may — carefully — be claimed as new. It is written as
a guardrail against overclaiming. Read it before quoting this project anywhere.

---

## Why it's public

The method isn't ownable and isn't meant to be. It's published so it can be used,
argued with, and improved. Attribution is the only thing asked in return.

**It's meant to be argued with.** If you've built something like this — especially
the part about flagging decisions when their premises die — the author would
like to hear how it went.

---

## License

Creative Commons Attribution 4.0 International (CC BY 4.0). See `LICENSE`.

Use it, adapt it, build on it commercially. Just credit the source.

---

<!-- ─── FILL THESE IN BEFORE MAKING THE REPO PUBLIC ─── -->
<!-- 1. Replace [ARTICLE URL] below with the live post URL, then click it to confirm it loads. -->
<!-- 2. Replace [YOUR NAME] with the byline you're publishing under — it must match the article and the DOI exactly. -->
<!-- 3. Delete these comment lines. -->

The essay this came from: [ARTICLE URL]

[YOUR NAME], August 2026.
