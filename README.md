# Counterentry

**A knowledge structure that notices when it stops being true.**

A note said a dashboard required an admin login. The permission check in the code returned `true` for every visitor, and always had. Nothing crashed and nothing logged an error, because the code was doing exactly what it was written to do. It came to light only when the note and the code were put side by side and compared.

Counterentry is a specification for making that comparison routine. Every claim that matters is paired with a second, independent record of the same fact, and the job is to find the pairs that no longer agree. The name comes from double-entry bookkeeping, where the counter-entry is the half that makes the first one checkable.

I first published it in August 2026, for records about software. Since late August it has also run every day as the backbone of my own personal assistant: scheduled runs that read my calendar, mail and bank feed against an append-only record, and give me a short brief at the turning points of my day. Version 3 is what six weeks of that taught me.

## How it works

Every load-bearing claim carries a counter-entry: a second record of the same fact, produced independently.

| The record says | The counter-entry |
|---|---|
| "The dashboard requires an admin login." | The permission check in the code. |
| "The phone bill is paid." | The bank's posted transactions after the due date. |
| "I'll send the numbers by Friday." | A sent message before Friday. |
| "The morning run finished." | The brief it was due to write, found in the record. |

When the two agree, the claim is verified. When they disagree, the system says so plainly and a person decides what is true. When only one record exists, the claim is marked single-entry and is never presented as verified.

A few mechanisms sit around that rule:

- **A gate based on reversibility.** Facts that can be re-derived are written freely. Reversible changes are proposed. Money, anything about a person, anything irreversible and anything public wait for a human.
- **Return conditions instead of reminders.** Anything put off carries the condition that brings it back, plus a second trigger (a date, a count or a check) that fires even if nobody notices the condition.
- **A capped brief.** At most seven lines and three decisions, each stating its cost, its source and the decision it enables. A footer lists what was checked and what could not be, so a quiet system and a broken one never look the same.
- **Supersede, never delete.** Corrections are new entries that name what they replace, so the record can answer both "what is true?" and "what did we believe on 3 June?"

## What daily use has caught

Most of what the deployment has caught came from a direct comparison and a plain rule, not from the more elaborate parts of the specification. A few of the field notes:

- A dashboard that silently dropped every task and claim the scheduled runs wrote for about six days, while its money panel stayed current, so the page looked healthy.
- A scheduled job tied to a laptop, which the scheduler switched off when the laptop went to sleep. No error, no alert, and nothing said so.
- A morning brief that quoted bank figures about 21 hours old every day, because the bank feed refreshed three hours after the run.
- The assistant itself, reporting that several things had "never" been handled when the logs showed they had. Every wrong claim was an absence claim, made without searching where the evidence would be.

[FIELD-NOTES.md](FIELD-NOTES.md) has every entry since August, with dates, how each one surfaced and what it changed.

## Status

| | As of 6 October 2026 |
|---|---|
| In daily use | Since late August 2026, in one personal deployment ([DEPLOYMENT.md](DEPLOYMENT.md)) |
| Scheduled runs | Morning and evening every day, an afternoon run on weekdays, and a text-message audit twice a day |
| Field notes | 23, numbered 5 to 27 after the four receipts published in August |
| Open holes in the system's own visibility | 7 |
| Degraded | The dashboard and the audio brief (since 1 October), spoken capture from the phone (since late September), and full reads of the text-message export (since 3 October) |
| Rules with a measured precision | None yet. The one rule measured so far failed its test. |

The append-only record, the scheduled runs, the capped brief, the open-holes list and hash-verified run instructions are built. A mechanical gate, the precision ledger, premise propagation, bi-temporal storage and the dormancy machinery are specified and not built; today the gate is enforced by instructions, which the spec itself calls a preference rather than a guarantee. [Section 2 of the spec](COUNTERENTRY.md#2-status) is the one place the status is kept current, with the reason behind each gap.

## How this is made

I'm not a software engineer by training. I designed this method and run it every day on my own assistant, on my own time. Claude does much of the implementation and drafting, this repository included, under my direction: I set the rules, make the rulings, decide what ships, and answer for every claim here. The assistant's work, including its own account of what it did, gets checked by the method like everything else. [Field note 22](FIELD-NOTES.md#22-the-assistant-claimed-absences-without-searching) is a time it failed that check.

## What's in this repository

| File | Contents |
|---|---|
| [COUNTERENTRY.md](COUNTERENTRY.md) | The specification, version 3: status, vocabulary, the counter-entry rule, the six passes, invariants, receipts and prior art. |
| [FIELD-NOTES.md](FIELD-NOTES.md) | Dated findings from daily use, and what each one changed. |
| [DEPLOYMENT.md](DEPLOYMENT.md) | How my deployment is put together, including its permission model and known limits. |
| [STARTER-KIT.md](STARTER-KIT.md) | Copy-paste templates: assistant instructions, a record entry format, a brief format and a worked example. |
| [AGENTS.md](AGENTS.md) | Instructions for AI agents that read, cite or apply this repository. |
| [CHANGELOG.md](CHANGELOG.md) | What changed between versions. |
| [ESSAY.md](ESSAY.md) | The August 2026 essay that introduced the method. |

## Getting started

1. Read §6 (the counter-entry rule) and §8 (the invariants) of the spec. Most of the value is there.
2. Pick one area where being wrong costs you something: bills, a codebase, commitments to people.
3. Give your assistant the instructions in [STARTER-KIT.md](STARTER-KIT.md), and keep its records as dated entries it can only add to. Don't paste the whole spec into its instructions; [field note 16](FIELD-NOTES.md#16-instructions-too-long-to-rely-on) explains why.
4. Add a mechanism only after its absence has cost you something, and write down what it cost.

## Credit

The foundation is Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern (April 2026): compile knowledge once instead of retrieving it for every query, let the model do the bookkeeping, and keep everything in plain versioned files. Contradiction-hunting is already part of his lint operation. What Counterentry changes is what triggers a check, what the check compares against, and what governs the output.

Other people have built close relatives, several of them posted in that gist's comment thread. [Section 11 of the spec](COUNTERENTRY.md#11-prior-art) credits them, along with the older work this rests on, and records the one novelty claim I had to withdraw.

## License and citation

The text is licensed under [Creative Commons Attribution 4.0](LICENSE): use it, adapt it, build on it commercially, and credit the source.

Archived on Zenodo at [doi:10.5281/zenodo.21924843](https://doi.org/10.5281/zenodo.21924843), which always resolves to the latest archived version. Citation details are in [CITATION.cff](CITATION.cff).

Questions, disagreements and reports of similar systems are welcome. Open an issue.

Chris Pollock, October 2026.
