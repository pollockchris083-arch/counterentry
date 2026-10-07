# Changelog

## Version 3, 6 October 2026

Six weeks of daily use in my own deployment. Most changes below are tied to a dated entry in [FIELD-NOTES.md](FIELD-NOTES.md). The rest are rulings I made, as the deployment's owner, about how the system should treat me, and are marked *(owner's ruling)*.

### Scope

- The method now covers the records a person runs their life on (money, commitments, a household), not only records about software. The founding case and the core rule are unchanged.

### Specification ([COUNTERENTRY.md](COUNTERENTRY.md))

- **§2 Status, rewritten.** Scheduled runs, an append-only record, the capped brief, a dashboard, the open-holes list, hash-verified instructions and a check of the assistant's own claims now exist in one deployment, and the table says where each is degraded. A mechanical gate, the precision ledger, loops, premise propagation and the specified `store ↔ code` check do not exist yet. The version 2 table said there was no scheduler; that was true then and is not now. §2 is now the one place status is kept, and other files point to it.
- **§3 Vocabulary.** New closed set, entry status: `agree`, `disagree`, `single`, `unreachable`. A resolved disagreement is handled by supersession, not by a fifth status. Also notes that `risk` is both a node type and a state, says when `delete` applies, and says how much of the vocabulary the deployment actually uses.
- **§4 Dials.** Voice now has a floor: a store can be shown but never spoken *(owner's ruling)*. Capture appetite and voice are stated as two separate bars. The shared-store section now calls its answer to the blackboard pattern's open problem what it is: a proposal, untested.
- **§5 Properties.** New property `next_check`, for a `work` node whose `owner` is someone else. For values read from an instrument, `valid_from` is the instrument's own timestamp.
- **§6 Counter-entry rule.** Entry status defined. Compound claims are split. The records that cannot serve as a second entry are named: two records from one session, a record and its copy, an actor's own account of its work, and configuration. The counter-entry for `done` is an outcome observed outside the actor. Payments and cancellations stay single-entry until the posted history shows them.
- **§7 Passes.** Ingest splits a capture automatically and lands each piece only on approval *(owner's ruling)*. Prune: nothing leaves the record because time passed *(owner's ruling)*. The brief gains a three-decision cap *(owner's ruling)*, four questions for what earns a line *(owner's ruling)*, "nothing needs you" as a complete brief, and holes in the footer. New subsections on holes and on scheduling against sources rather than the clock. Reconcile now notes that in the reference deployment a model applies the rules.
- **§8 Invariants.** Invariant 1 is spelled out as three classes for a personal deployment *(owner's ruling)*, with faults in the system's own sight treated as holes rather than housekeeping. Invariant 3 now requires a second trigger that does not depend on anyone noticing. Six new secondary rules: read the record before claiming; configuration is not behaviour; render before presenting; state a changing fact once; compute dates; a successful read is not a complete read. Three new enforcement points (on deploy, before presenting a page, on every run), and the end-of-session point now includes the session-claims check.
- **§9 Receipts 5–27** added as a table, with full entries in the field notes. A receipt can now also record a mechanism observed working (receipt 21). The paragraph on the gap between the receipts and the architecture is kept and updated.
- **§10 Build order.** Records that the deployment did not follow the recommended order, and what that cost: there are still no per-rule precision numbers.
- **§11 Prior art.** Added Link 3.0's `lnk stale`, Gilda and Gilda (2026) on evidence decay in architecture decisions, Jaroslawicz et al. (2025) on instruction density, Gawande's checklists, and four pieces of work posted in the LLM Wiki gist thread since August (post-graph-rag, Obelyth Cortex, Cortexes, and one team's audit of whether its agent consulted its wiki at all). Added the older foundations: truth maintenance (Doyle, 1979), belief revision (Alchourrón, Gärdenfors and Makinson, 1985), bitemporal databases (Snodgrass and Ahn, 1985; SQL:2011) and W3C PROV. Replaced an unsourced range of benchmark scores with cited figures. "What may be claimed" is replaced by "What this project claims": that the combination runs every day, not that it is new.
- **§12 Failure modes.** Added: instruction-enforced rules degrade as instructions grow; an assistant's account of the system is the easiest claim to trust by mistake; views fail silently; append-only is a procedure, not a storage guarantee.
- Rewritten throughout in plainer language, and lines addressed to AI agents moved to [AGENTS.md](AGENTS.md). The structure and section numbers are unchanged.

### Other files

- New: [FIELD-NOTES.md](FIELD-NOTES.md), [DEPLOYMENT.md](DEPLOYMENT.md), [STARTER-KIT.md](STARTER-KIT.md), [AGENTS.md](AGENTS.md) and this changelog. The field notes and the deployment are written in the first person, because they are my account of my own system.
- README rewritten.
- [LICENSE](LICENSE) now holds the full legal text of CC BY 4.0 instead of a summary, so that tools, GitHub included, can recognise the license.
- [CITATION.cff](CITATION.cff) updated for version 3, with the Zenodo concept DOI as the citation DOI.

### Essay

- [ESSAY.md](ESSAY.md) gains a dated note at the top, and was edited on 6 October for privacy and clarity: a repair figure and a reference to personal finances were removed, details about where the original examples came from were made general, and "no access control" now says it refers to the record itself.

### Not yet done

- Version 3 has not been deposited on Zenodo. The all-versions DOI, [doi:10.5281/zenodo.21924843](https://doi.org/10.5281/zenodo.21924843), still resolves to version 2.
- The files archived as version 2 are the 13 August text, from before the 14 August correction below. The correction is in this repository's history and carried into version 3.

## Version 2, 13 and 14 August 2026

- First public release: the specification, the essay, the README and the license. Archived at [doi:10.5281/zenodo.21924844](https://doi.org/10.5281/zenodo.21924844).
- 14 August: withdrew the claim that premise-based decision invalidation was new, after finding Vigil, which its author had described in the LLM Wiki gist thread on 12 August. A correction notice was added to the essay and §11 was rewritten.
- 14 August: added CITATION.cff.
