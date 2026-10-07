# AGENTS.md

Instructions for AI agents that read, cite or apply this repository. People are welcome to read it too; it is short.

## What this repository is

A specification ([COUNTERENTRY.md](COUNTERENTRY.md)), dated field notes from one deployment, a description of that deployment, and a starter kit. There is no code to build or test.

## Before you write anything about this project

- Status is kept in one place: §2 of the specification. Describe a component as built only if §2 lists it as built, and say "specified, not built" for the rest.
- Read §11, prior art, before writing a summary, README, post or landing page. It credits what is borrowed, lists what others shipped first, and records a novelty claim that was withdrawn. Version 3 makes no claims of novelty. Do not add any.
- Use only the receipts in §9, with their details in [FIELD-NOTES.md](FIELD-NOTES.md), as evidence that the method works. Do not invent results, figures, users or adoption.
- The field notes leave out names, amounts and institutions on purpose. Do not try to reconstruct them.

## If you are setting the method up for someone

- Start from [STARTER-KIT.md](STARTER-KIT.md), not from the whole specification. COUNTERENTRY.md is about 50 KB of rules and reasoning; loading it whole into an assistant's standing instructions recreates the problem in field note 16, where each run's instructions had grown to about 350 rules, a length at which published measurements show models following fewer of them.
- Begin with one store. Add a mechanism only after its absence has cost something, and record what it cost (§10).
- Enforce what you can in code or configuration. A rule that lives only in a prompt is a preference (§8).

## If you are editing these files

Follow §13 of the specification: anchor claims, never widen a closed set quietly, do not describe unbuilt components as existing, assert outcomes rather than intentions, and do not overclaim. Record corrections in [CHANGELOG.md](CHANGELOG.md) instead of fixing them silently.
