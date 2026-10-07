# Counterentry

**A reconciling knowledge structure.** Specification, version 3, 6 October 2026.

This document defines the method, records what has been built and what has not, and credits what was borrowed. Nothing in it depends on a particular model, harness or product. [DEPLOYMENT.md](DEPLOYMENT.md) describes one working deployment, and [FIELD-NOTES.md](FIELD-NOTES.md) holds the evidence behind this version's changes.

To try the method with an assistant, start from [STARTER-KIT.md](STARTER-KIT.md) rather than loading this whole document into its instructions; field note 16 explains why. Agents reading this repository should start with [AGENTS.md](AGENTS.md).

Changes from version 2 are listed in [CHANGELOG.md](CHANGELOG.md). Archived versions are at [doi:10.5281/zenodo.21924843](https://doi.org/10.5281/zenodo.21924843).

---

## 1. What this is

A structure for keeping the records that people and agents act on (what a system does, what was decided and why, what is owed and to whom, what an account currently says) whose distinguishing job is to notice when those records stop matching reality.

Most knowledge tools store information and return it when asked. This one's main job is to compare records and report where they disagree.

Versions 1 and 2 were written for records about software. Version 3 widens the scope to the records a person runs their life on, because that is where the method has been used every day since late August 2026. The core rule did not change. What did change, and why, is in the changelog and the field notes.

### The founding case

A record said a dashboard required an admin login. The permission check in the code returned `true` for every visitor and always had. Nothing crashed and nothing threw an error, so nothing was watching. Error monitoring looks for failures, and this was code doing exactly what it was written to do. It surfaced only when the record and the code were put side by side and compared.

### The gap it addresses

Code has tests. Infrastructure has drift detection, which compares declared state with actual state on a schedule. What a team, or a person, believes about its own systems has no equivalent feedback loop. It is written down once, confidently, and then quietly stops being true while decisions keep being made from it.

---

## 2. Status

This section is the one place this project's status is kept. Other files point here rather than repeating it.

### Built and in daily use

One personal deployment, running since late August 2026. Details are in [DEPLOYMENT.md](DEPLOYMENT.md).

| Component | State |
|---|---|
| Append-only record | Dated entries in cloud storage, one folder per store. Corrections are new entries that name what they supersede. Append-only is a procedure, not a property of the storage (§12). |
| Scheduled runs | A morning and an evening run every day, an afternoon run on weekdays, and a twice-daily audit of exported text messages. Each run reads the record and the instruments (calendar, mail, a bank feed, the message export) and appends a run log, a data snapshot and a brief. By late September, the runs' own logs recorded the ownership check before every write in more than 60 runs. Of the six passes (§7), the runs carry out ingest, reconcile and the brief; consolidate, outward and prune are done by hand in working sessions. Two inputs are degraded: spoken capture from the phone has failed since late September (field note 24), and reads of the text-message export have been cut short since 3 October (field note 27). |
| The brief | At most seven lines and three decisions, with a footer that states what was checked, what could not be checked, and the open holes (§7). Also delivered as generated audio, in which money is never spoken (§4). The audio reads the same daily snapshot as the dashboard and is stale for the same reason. |
| Dashboard | A single-file page that renders only from the record and covers money by default. Since 1 October 2026 it has been stale: the daily data snapshot it reads grew too large for a single write, which is an open hole. |
| Open-holes list | The system's own blind channels, failing checks and stalled fixes, each with its cost and a next step (§7). Twenty-four versions by 6 October 2026. |
| Instruction verification | The runs' instructions are kept as source files, built, deployed, then fetched back from the scheduler and compared by SHA-256. The record calls a change live only after the hashes match. |
| Assistant-claim check | The evening run compares what the assistant said in working sessions about the system's behaviour against the run logs, and reports any disagreement (§8). The run logs are written by the runs themselves, so this checks one account against a closer one, not against an outside record. |

### Specified, not built

| Component | State |
|---|---|
| Mechanical gate | Invariant 1 is enforced by instructions. Scheduled runs get no permission prompts, so nothing structural stops a run that ignores them. By §8's own standard that makes the gate a strong preference, not a guarantee. This is the most important unbuilt piece. |
| Precision ledger | Not built. The one rule measured so far, a line-movement drift check on a codebase, failed its measurement: it raised no flag on either of the two commits that rewrote the logic it was watching, because positional anchors cannot see code that was rewritten in place. |
| `store ↔ code` on change | Not built. The specified version compares content and carries a file, line range and hash on each claim. A cruder check that compares line positions was tried on a codebase; it is the rule that failed above. |
| Loops and the completion gate | Approved for the deployment in September 2026 (§5 `next_check`, §6 on "done"). Not built. |
| Entry status set | Specified in this version (§3, §6). The deployment's record still uses four older keys and cannot express `unreachable` or a settled disagreement. |
| `predicated_on` propagation | Specified, not implemented. |
| Bi-temporal storage | One timestamp per entry. Values read from an instrument also carry the instrument's own as-of time. |
| Consolidation and dormancy | Designed. A deterministic consolidation pass was approved in September 2026 and is not built. |
| Cross-party reconciliation and shared stores | Design sketch. |
| Media ingestion | Manual capture works. The snapshot rule and the one-question ritual (§6) are followed by hand; nothing enforces them. |

---

## 3. Vocabulary

These sets are closed. Contradictions can only be computed by rule if the vocabulary is fixed, and widening a set by one convenient member is how the method slides back into prose. Propose additions explicitly, as this version does, rather than adding them quietly.

**Node types (8):** `concept` · `decision` · `risk` · `work` · `artifact` · `source` · `junction` · `entity`. Anything unclear is a `concept`. Resist a ninth.

**States (10):** `done` · `active` · `next` · `later` · `idea` · `standing` · `risk` · `broken` · `gap` · `blocked`. `active` is capped at three per store. `risk` is also a node type; they are separate fields, one saying what a claim is about and the other where it stands.

**Roles the reconciler reads (6):** `complete` · `in_motion` · `owned` · `damaged` · `halted` · `absent`. States map to roles. Two of the mappings were learned by getting them wrong:

- `standing` is never `owned`. Something with no finish line cannot own a fix.
- `later` is never `owned`. A backlog cannot own a breakage.

**Edge types (11):** `relates_to` · `depends_on` · `blocks` · `part_of` · `implements` · `evidences` · `contradicts` · `derived_from` · `same_as` · `similar_to` · `predicated_on`

**Counter-entry types (6):** `code` · `check` · `attestation` · `external` · `behaviour` · `counterparty`

**Entry status (4), added in version 3:** `agree` · `disagree` · `single` · `unreachable`. Defined in §6. A resolved disagreement is not a fifth status: the losing claim is superseded, and the surviving claim records the settling evidence as its counter-entry.

**Reversibility tiers (3):** `re_derivable` · `reversible` · `irreversible_or_personal`

**Lifecycle outcomes (7):** `keep` · `update` · `merge` · `supersede` · `archive` · `retire` · `delete`. `delete` is reserved for what should never have been recorded, such as a password pasted by mistake, and only a person can choose it. Corrections supersede; they never delete (§5).

**Instruments (4):** `store ↔ store` · `store ↔ code` · `store ↔ world` · `store ↔ behaviour`

Version 3 adds one closed set (entry status) and one property (`next_check`, §5). Nothing else was widened.

Much of this vocabulary is not yet exercised. The reference deployment's record uses stores, counter-entries with a status, supersession and return conditions. Node types, roles and edge types are specified here and not yet used there.

---

## 4. Objects

**Store.** One subject. The test is whether it could be handed to someone else; if not, it is not a store. Every store carries `held_by`. Ownership is explicit, never assumed.

**Region.** A standing area inside a store, with no finish line.

**Journey.** A path with a named end state: ordered steps with real dependencies.

**Crossing.** A person, place, service or resource that spans stores. It is not filed anywhere; many things point at it. A login service that nine applications depend on is a crossing. So is a person, and a person is the crossing that eventually becomes a store, which is why records about people are written as though that person will read them.

**Node.** A claim, of one of the eight types.

**Standing instruction.** A decision made in advance about what to do if nobody responds. Because it is a decision it carries premises, and it is flagged when they move. It has the shortest volatility window in the system and expires rather than persisting. Expiring means it stops being acted on; the entry itself stays in the record.

A **value** is not an object. It is a `concept` used as a premise. There is no values layer, and there must never be a values filter: values may reorder what surfaces, never suppress it.

### Topology: personal records and shared ones

When two or more people collaborate closely, the shape is not federation. It is `held_by` doing its ordinary job. Each person keeps a personal instance, held by that person and read by nobody else. Shared work lives in stores held by the organisation, kept in git next to the code they describe, and readable and writable by every collaborator's agent through the same gates, with attribution on every edge.

Agents do not message each other. They coordinate through the shared record: one proposes, with its principal's name on the edge, and another reads the proposal on its next pass. This is the blackboard pattern (Hearsay-II, 1980), revived across LLM multi-agent work in 2025 and 2026. That revival names an open problem: how shared state is updated, authorised and audited over long collaborations. This specification proposes an answer, untested so far: the gate, the reversibility tiers, attribution and counter-entries. A direct agent-to-agent channel would be less auditable than the record, and should not be built.

Concurrent writes to a shared store surface as git conflicts. A conflict is a contradiction made visible, and a person resolves it; it is never auto-merged. The federation design stays dormant between close collaborators. It exists for outside parties with interests of their own.

### Per-store configuration: the dials

The machinery is not uniform across stores. Each store declares what applies to it, because stores differ in what exists to check them against.

- **Instruments.** Which of the four run here. A store about software gets all four. A store whose only record is the owner's own words (a van build, a relationship) gets `store ↔ store` and sometimes `store ↔ world`. Running code or behaviour instruments against it is meaningless and should not be attempted.
- **Capture appetite.** `generous` where no other record exists, because in a life store a thing not captured is gone. `stingy` where a second record already exists, because in a code store a note that restates the code is noise. The setting inverts between the two kinds of store; a single global value is wrong for half of any real life.
- **Voice.** When the store may appear in the brief unprompted: `nightly-if-changed` (code), `monthly` (finances), `only-when-asked` (most life stores) or `never-unprompted`. Stores about people default to `never-unprompted`.
- **Voice floor (added in version 3).** Spoken output is a broadcast: anyone in the room hears it. A store can be marked shown-not-spoken. In the reference deployment, money is shown on the dashboard and never spoken; the spoken brief may say that a money item needs attention and how soon, and nothing more ([DEPLOYMENT.md](DEPLOYMENT.md)).

Capture appetite and voice are separate bars, and the gap between them is deliberate: capture generously into the record, surface strictly into the brief.

A store with every dial at minimum degrades gracefully to the LLM Wiki pattern plus the snapshot rule and the one question. That is correct, because it is all such a store can support. The configuration is itself a set of claims, subject to drift and reconciled like everything else it governs.

---

## 5. Claim properties

| Property | Meaning |
|---|---|
| `reversibility` | Which of the three tiers. Decides the gate. |
| `volatility` | Expected decay by kind of fact, not by risk. Past its window a claim is unknown: not wrong, and not still true. |
| `assumes` | What a reader must already know for the claim to make sense. A claim that fails a cold read is not finished. |
| `closes_at` | If the claim describes an option that expires, when. Drives escalation. |
| `next_check` | **Added in version 3.** For a `work` node whose `owner` is someone other than the person the store is held by: when to look again. Required whenever the `owner` is a counterparty. Such work is exempt from aging and closes only on a record from outside (a reply, a receipt, an observed result). |
| `disclose_after` | An optional release condition. A sealed claim cannot be reconciled while sealed, so use this rarely and knowingly. |

`verification_decay` is derived, never stored. It is a function of the counter-entry type: `attestation` decays fast because it depends on human memory, while `code` and `check` do not decay at all. Storing it would let it drift away from the thing it is computed from, which is exactly the failure this method exists to catch.

**Two clocks, not one:**

- `valid_from` / `valid_until`: when the claim was true in the world.
- `recorded_at` / `retired_at`: when it was learned, and when it stopped being believed.

Superseded claims are invalidated, never deleted. "What did we believe on 3 June?" and "What was true on 3 June?" are different questions, and both must be answerable.

**Values read from an instrument (added in version 3).** For a balance, a status or a count read from an instrument, `valid_from` is the instrument's own timestamp, not the time the value was read. A run that reads a bank feed before the feed has refreshed must report the figure as carried, with its age, and never as current (field note 15).

---

## 6. The counter-entry rule

Every load-bearing claim carries a second, independent representation of the same fact. This is the core discipline and the reason for the name.

| Type | What it is |
|---|---|
| `code` | A file, a line range and a content hash. |
| `check` | A command that returns true or false. |
| `attestation` | A second person, or a second system of record. |
| `external` | A fetched source, hashed at fetch time. |
| `behaviour` | Observed runtime reality: a synthetic request, a log signature, a posted transaction. |
| `counterparty` | Another party's independent entry of the same claim. |

The counter-entry for a stated value is `behaviour`. What you did is the second entry for what you said mattered.

### Entry status

Every load-bearing claim has one of four statuses (§3):

- `agree`: two independent entries exist and match.
- `disagree`: they exist and do not match. The **trial balance** is the count of claims in this status. It should be zero.
- `single`: only one entry exists so far, and a second may be obtainable. If the claim matters, go and look.
- `unreachable`: no instrument available can produce a second entry. Stop looking, and say so whenever the claim is used.

A `single` or `unreachable` claim is permitted, labelled, and never used to answer a question without its label. Plenty of true things cannot be double-entered.

A disagreement is settled by superseding the losing claim (lifecycle outcome `supersede`, with `retired_at` set) and recording the settling evidence as the surviving claim's counter-entry. It does not get a status of its own; the supersession is the record that it was settled, and when.

A claim can be double-entry in one part and single-entry in another: that a charge exists, and whose charge it is. Split such a claim in two and give each part its own status (field note 25).

### What does not count as a second entry

Independence is the point, so some pairs do not qualify:

- Two records produced from the same session are one record. A memory an assistant saved and a document summarising the same conversation cannot confirm each other (adopted with field note 11).
- A record and a copy of it are one record. So are two reports built from the same feed.
- An actor's account of its own work is not the counter-entry for that work. The counter-entry for `done` is an outcome observed outside the actor: a posted transaction, a reply received, a file that exists, a deployment fetched back and matched. Completing one step inside an open matter does not close the matter; it passes it to whoever holds it next, with a `next_check` date (field note 13).
- A configuration value is not evidence of behaviour (§8).

For money specifically: a payment or a cancellation stays `single` until the account's posted history shows it, and a cancellation stays `single` until a full billing cycle passes with no charge. Pending is not posted.

### Sources from outside: articles, videos, podcasts, links

External media enters through the existing vocabulary: a `source` node, an `external` counter-entry, and `derived_from` and `evidences` edges. No new machinery is needed. Three rules govern it.

**Every external source is stored in three layers:**

1. **The snapshot.** The full text or transcript, copied locally and hashed at capture. The URL is kept as provenance, never as the copy. This inverts the references-not-copies rule at the project boundary on purpose: internal originals get edited, so copies of them go stale, but external originals disappear. Pew Research (2024) found 38% of web pages that existed in 2013 were gone, and 8% of pages from 2023 had vanished within a year. Snapshot what you do not control.
2. **What it said.** A short machine-written summary, drafted from the snapshot.
3. **Your take.** One sentence, supplied by the person at capture in answer to a single question: "What are you taking from this?" The take is the claim that enters the record, and the source hangs beneath it as evidence. It is the only layer that can link, support a decision or contradict a plan, and it is the layer the generation effect says the person will actually remember (Slamecka and Graf, 1978; a 310-experiment meta-analysis, 2020).

**The take is optional, and a skipped take is marked.** A source-only save is allowed. Asked later "what did I learn from this?", the honest answer is: "You saved it and never said what you took from it. Here is what it said, and the snapshot." The no-confident-answer invariant applies in full. The system never generates a lesson the person did not have.

**Retrieval resets disuse.** Each time a claim or source is used in an answer, its last-referenced clock resets. Media that is never used drifts toward a one-line archive proposal like everything else: archived, not deleted.

**What each medium yields.** Articles and web pages yield full text. YouTube yields a transcript through standard caption tools; visual content is carried by the person's note and a timestamp. Podcasts yield a transcript when the show publishes one, and a thinner source layer when it does not. Instagram has no reliable machine path and is captured by hand, so the person's description is the record. This layer needs no scraper, auto-ingestion pipeline, transcription farm or vector database. Intent ("add this") is the filter, and plain-file search is enough at this scale.

**Meeting transcripts are a distinct kind of source, because a meeting yields commitments rather than a take.** Most follow-through failure after a meeting is a translation failure: decisions made out loud never become explicit commitments. Transcript ingest therefore drafts a set of proposals from the existing vocabulary: `decision` nodes with their spoken reasons as `predicated_on` premises; `work` nodes, each with an `owner` (a person or an agent, and the one declared assignment in the system); open questions, as nodes in state `gap`; and deferrals with return conditions. The transcript snapshot sits beneath all of them as evidence. Owners are proposed, never assumed, because automatic speaker attribution is unreliable, and each participant approves their own lines. "What's on me from that meeting?" is then just a query over `work` nodes by owner and source.

---

## 7. The six passes

**1. Ingest.** Read raw capture, route it to a store, and draft it. Write only what the reversibility tier allows; everything else becomes a proposal. Be stingy: the approval rate is ingest's own precision metric, and if a person approves five percent of what it proposes, it is proposing too much and must tune itself down. One capture often holds several items. Splitting it is automatic; each piece lands in a store only on approval, with a suggested home and date. Anything that touches a person or a deadline is proposed, never filed silently.

**2. Consolidate.** Re-process the unreviewed set: dedupe, merge, supersede, and resolve proposals that later evidence has answered. This pass may only subtract. It never synthesises a new claim. It is the pass that runs when nobody is watching, which makes it the most dangerous part of the method, so it is the most constrained.

**3. Reconcile.** Stand outside every store and own none of them. Compare across the four instruments. Rules decide, and language only explains; in the reference deployment the rules are applied by a model following instructions, which is weaker than code applying them (§12). Never auto-merge. Rank findings by how many independent instruments agree, remembering that two observers sharing a source are one observer. That includes monitors: a watchdog that shares an account, a quota or credentials with what it watches is not independent of it.

**4. Outward.** Take open questions to the live web. Every claim carries a source URL. When a finding contradicts a store, say so plainly and never smooth it over.

**5. Prune.** Propose lifecycle transitions, one line each, approved with a single action. Nothing leaves the record because time passed. Age is reported; removal is a decision a person makes.

**6. Brief.** One short report with a hard cap: at most seven lines and at most three decisions. Every line names its cost, its source and the decision it enables. If a line changes nothing the reader would do, it is not a line. "Nothing needs you" is a complete brief.

What earns a line is decided by four questions, asked in order: Was it promised? Who is it with? Is there a clock? What does dropping it cost? Rank by cost first and clock second. Items that need the owner now go in the brief. Items with no clock go to an open list the owner visits when they choose. Everything else stays in the record.

The footer is exempt from the cap, because a quiet system and a broken system must never look the same. It states what was checked (with each instrument's as-of time), what could not be checked, and the open holes. A check that could not run is never reported as a check that passed.

### Holes

A hole is a fault in the system's own ability to see: a channel that is blind, a check that cannot run, a source that has returned nothing for five runs in a row, a fix waiting on the owner's hands or decision, or a defect the assistant has carried unfixed for three sessions. Holes are not housekeeping. Each one goes on an open-holes list with what it is in plain words, what it is costing, and the exact next step. A hole without a next step is not allowed on the list. The footer names at most three. A hole is spoken only when it is new, when it closes, or when its next step belongs to the owner and is due today. The holes line goes quiet only when nothing is open (field notes 20 and 23).

### When to run

Schedule passes around their sources and the owner's day: after the instruments they read have refreshed, and at the points where the owner changes context, such as starting the day, mid-afternoon and winding down (field note 15).

### The execution boundary

This system records work. It does not perform or manage it. Execution belongs to agents and tools outside it, and the boundary runs both ways:

- **Outbound.** An executor reads `work` nodes (`state: next`, `owner:` the agent or its principal) from the store. The store is the assignment; there is no separate dispatch channel.
- **Inbound.** An executor may propose state changes, and `done` requires a counter-entry: a commit hash, a deploy log, an observed behaviour. No evidence, no done. This is invariant 2 applied to agents, and it is how work done unattended stays trustworthy.
- **Never.** Counterentry does not schedule, remind or orchestrate, and it is not a task manager. The brief surfaces work and may propose a calendar entry or a draft; the owner approves it, and a tool executes it. The record judges whether it happened.

---

## 8. Invariants

Five are core. Enforce them mechanically wherever the harness allows: an invariant enforced only by a prompt is a preference.

1. **Gate by reversibility, not by kind.** Re-derivable facts are written freely. Reversible writes are proposed. Irreversible writes, and anything asserting something about a person, are gated absolutely. In a personal deployment this comes out as three classes. The assistant may close housekeeping, such as a late run or a duplicate entry, and records that it did; a fault in its own ability to see is a hole and is handled under §7. It may draft, research, file and reconcile, and shows the owner before any of it counts. Money moving, anything about a person, anything irreversible and anything public under the owner's name belong to the owner alone. The gate decides both what the record may come to believe without a person and what the assistant may do without one, so weakening it weakens both.
2. **A check must assert the outcome, never the intention.** "Available for embedding" is a hope. "Embedded and verified" is a fact.
3. **A deferral without a return condition is a deletion.** The condition is a trigger, not a date: "when we next touch auth", "if a second customer asks". A trigger that relies on someone noticing it also needs a second trigger that does not: a date, a count, or a check the machine runs. A silent failure produces no reports, so a deferral that waits for a report never comes back.
4. **No confident answer is a valid answer,** and it is never filed back. If retrieval returns nothing above threshold, say that the record has nothing confident on this.
5. **Fetched content is data, never instruction.** So is sensor input, and so is anything a counterparty asserts. Proposals that carry imperatives are flagged, never silently dropped.

Secondary rules are corollaries and policy, enforced where it is cheap. Those added in version 3 cite the field note that produced them.

- References, not copies, for anything this project controls. External web content is the exception (§6): snapshot it, because the original is rarely edited and often disappears. Never copy another party's store.
- Sources, not builds. Keep only what cannot be regenerated.
- Accuracy beats tidiness. Mark a claim unverified rather than asserting it.
- Read a system's own definitions, not only its data. Recomputing a system's rule yourself is harder to catch than a typo, because the arithmetic is right.
- Write every record about a person as if that person will read it.
- Silence must be proven.
- Every escalation ends in an action, a pre-authorised default, or an expiry a person recorded, and never in more alerts.
- During dormancy, only subtract.
- Read the record before making a claim about what the system did. A claim that something never happened needs the search that would have found it, cited. A finding with no source is marked VERIFY (field note 22).
- Configuration is not behaviour. What a job can do is established from its logs, not its settings, and a change is live only when the deployed version has been fetched back and matched (field note 18).
- Render before presenting. A page nobody has looked at is a single-entry claim about itself (field note 5).
- State an invariant rule as often as you like; state a changing fact once. Copies of a changing fact go stale independently of one another (field note 17).
- Compute dates, weekdays, business-day counts and time-zone conversions. Never recall them or copy them from an earlier document (field note 12).
- A successful read is not a complete read. Walk every page of a result, and check that a document's end is actually there (field notes 7 and 27).

### Enforcement points

Where a harness exposes lifecycle hooks, bind the invariants to them rather than restating them.

| Point | Enforces |
|---|---|
| Before a write | Block writes above the reversibility threshold (1). Confirm the write target belongs to the record's owner. |
| End of session | Run the deferral sweep unconditionally (3). Check the session's claims about the system against the run logs; if the session does not, the next scheduled run does. |
| Before a prompt is answered | Inject contradicting claims first. |
| On file change | Flag every claim anchored to that file. |
| On schema validation | Reject writes that carry invalid values for a closed set. |
| After a fetch | Wrap retrieved content in a data envelope (5). |
| On retrieval | Log the top relevance score of every query, to set the no-confident-answer threshold from data (4). |
| On deploy | Fetch the deployed instructions back and compare by hash. Until they match, the change is pending. |
| Before presenting a page | Render it at desktop and phone widths and inspect it. |
| On every run | Read the open-holes list first. A source that has returned nothing for five runs opens a hole. |
| On heartbeat | If a pass has not run in *n* periods, that is a finding about the system itself. |

---

## 9. Receipts

A receipt is evidence from real use: usually something lost, or nearly lost, for want of a mechanism, and occasionally a mechanism observed doing its job (receipt 21). Receipts are the evidence for the structure, and nothing else is offered as evidence.

**Published in August 2026:**

1. **An admin gate that never existed.** A record claimed a dashboard required an admin login; the permission check returned `true` for everyone, always. Found by comparing the record against the code.
2. **A silent data-loss path.** The same application accepted entries on a dead session with no signal, so hours of entries could vanish unnoticed.
3. **A belief that went stale.** A recorded belief that running while you sleep set this project apart, killed by the outward pass after Notion shipped autonomous scheduled agents in February 2026.
4. **A pipeline reporting success while carrying nothing.** The capture inbox ran clean for two days and routed almost nothing, because nothing was ever put in front of it. This produced invariant 2.

**From the personal deployment, August to October 2026.** Full entries, including how each surfaced and what it changed, are in [FIELD-NOTES.md](FIELD-NOTES.md).

| # | Date | Finding | Led to |
|---|---|---|---|
| 5 | 23 Aug | A page shipped with its text squeezed into a 34-pixel column; its footer listed every check except looking at it. | Render before presenting (§8) |
| 6 | 3 Sep | The dashboard dropped every task and claim the runs wrote for about six days while its money panel stayed current. | Test views against real output (§12) |
| 7 | 3 Sep | A search's first page hid the newest file, and its "contains" ignored punctuation and matched word prefixes. | A successful read is not a complete read (§8) |
| 8 | 3 Sep | Done and Park buttons changed the browser and never reached the record. | Actions write receipts |
| 9 | 7 Sep | One misattributed charge, repeated by every run for five days. | Supersede at the source |
| 10 | 9 Sep | "Comfortable for weeks" was three to five days to the warning line when measured. | Measure headroom; tripwires |
| 11 | 9 Sep | A priority question answered from memory, wrongly. | Check before answering; same-session records count as one (§6) |
| 12 | 9 Sep, 6 Oct | Weekdays, holidays and time-zone conversions written from memory were wrong. | Compute dates (§8) |
| 13 | 10–14 Sep | A task closed on its owner's tap while the problem was still open. | `done` needs an outside outcome; `next_check` (§5, §6) |
| 14 | 13 Sep | A job bound to a laptop disabled itself when the laptop slept, with no alert. | Devices push; they are never polled |
| 15 | 13 Sep | Every morning brief quoted bank figures about 21 hours old. | Instrument timestamps (§5); run against sources (§7) |
| 16 | 13–20 Sep | Run instructions reached about 350 instruction sentences each; the morning run's reached 96.5% of the scheduler's size limit. | Instructions must shrink before anything new ships (§12) |
| 17 | 19 Sep | One run time stated in three places; after a change, all three were wrong. | State a changing fact once (§8) |
| 18 | 19 Sep | A change reported as pending for four days had been live the whole time; a tool list misread as capability. | Configuration is not behaviour (§8) |
| 19 | 19 Sep | A clock-change job that skipped rows it did not recognise would have reported success. | Jobs report what they skipped |
| 20 | 19 Sep | A dead source returned zero for nineteen runs, indistinguishable from a quiet day. | Five zeros open a hole (§7) |
| 21 | 18–19 Sep | Mail was down for three runs and each brief said so. | The footer rule, working as designed |
| 22 | 20 Sep | The assistant reported absences it had never searched for, and a real item was closed as a result. | Read the record before claiming (§8) |
| 23 | 20 Sep | Faults in the system's own machinery were being closed quietly as housekeeping. | The open-holes list (§7) |
| 24 | 22 Sep | Spoken captures failed on the phone, before reaching anything the runs can see. | Test a path from its first hop |
| 25 | 22 Sep – 6 Oct | The record could not express a settled disagreement, or a claim no instrument can ever verify. | Entry status, including `unreachable` (§3, §6) |
| 26 | 23 Sep – 4 Oct | A watchdog raised false alarms; its hole was closed on a condition written before the test. | Write the closing condition first |
| 27 | 3–6 Oct | A document read reported success and silently stopped partway through. | Check the end of what you read (§8) |

**The gap between these and the architecture is still real, and should be stated.** Nearly every receipt since August was caught by a direct comparison and a plain rule, not by the counter-entry taxonomy, premises or a second clock. They justify the core rule, the gate, the brief's footer and the holes list. They do not yet justify the rest of the structure, and should not be presented as if they do.

---

## 10. Build order

The recommended order front-loads measurement over features.

1. **The precision ledger, in shadow.** Run the contradiction rules, log every finding, mark each one real or not, and let nothing reach a person for at least a month. Every rule starts silent and earns a place in the brief only after clearing a precision bar over a minimum number of findings; a rule that falls below the bar demotes itself. Track two numbers permanently: **flag precision** (raised, then found real) and **action rate** (reached a person, then acted on). If either goes flat, the system is turning decorative.
2. **One store, not nine.**
3. **Counter-entries on new claims only.** Do not backfill.
4. **`predicated_on` on decisions from here forward.**
5. **Dormancy, before it feels necessary.** It will be needed during a stretch when nobody is around to build it.

Everything else waits.

The governing rule for all of it is **no mechanism without a receipt**. A feature is justified the way a claim is, by a second entry: something actually lost because the mechanism was missing. Forgot why you chose the diesel heater? Then add `predicated_on` to that store. Built on a stale note after a teammate shipped? Then wire the nightly code check. Start with one store, and let the losses set the roadmap. That keeps the complexity earned, and it keeps the person using the system rather than building it, which is the failure this method is most likely to die of.

### What actually happened

The personal deployment did not follow this order. It built the scheduled runs, the append-only record and the brief first, because it had to be useful every day to survive. The precision ledger is still unbuilt, so there are no per-rule hit rates; the brief's quality is judged by the author's rulings as its only reader, and nobody has measured it. The order above stands as the recommendation, and the cost of skipping step 1 is visible in §9: version 3 rests on receipts, and there are no numbers yet.

The governing rule mostly held. Most additions in version 3 cite a receipt. The rest (the voice floor, splitting a capture and landing each piece only on approval, the three-decision cap, the four questions for the brief, the three classes in invariant 1, and the rule that nothing leaves the record because time passed) are the author's explicit rulings, as the deployment's owner, about how the system should treat him. That is a different kind of evidence, and the changelog marks them as such.

---

## 11. Prior art

This section credits what is borrowed, lists what others have already shipped, and records the novelty claim that had to be withdrawn.

### The foundation is Andrej Karpathy's

His LLM Wiki pattern (gist, 4 April 2026) supplies the base: compile once rather than retrieving for each query; let the model own the bookkeeping; keep everything in plain versioned files; let a schema file govern conventions; and file good answers back as new material.

His *lint* operation already looks for contradictions between pages, stale claims superseded by newer sources, orphans, and gaps a web search could fill. Contradiction-hunting is not an addition here. What differs is what triggers a check, what the check compares against, and what governs the output.

Structurally, this method is that pattern with five insertions: a gate, structure on every claim, three additional instruments, a governor (the precision ledger and the brief's cap, which limit what reaches a person) and a lifecycle. Remove the five and the LLM Wiki is what remains.

### The core mechanism has an academic ancestry

The field is code-comment inconsistency detection. Panthaplackel et al. (2021) detected comments made stale by a code change at the moment of the change, "just in time". That is the "flag on change" mechanism, five years earlier. The field has public benchmarks, including JITDATA and CCIBench; on CCIBench, CCISolver reports an F1 score of 89.54% (Zhong et al., 2025, arXiv:2506.20558).

One result from that field supports a design choice here. Nguyen, Bui and Nguyen (2025, arXiv:2512.19883) built a detector on the small CodeT5+ model and report that it beat fine-tuned general code models, including DeepSeek-Coder, CodeLlama and Qwen2.5-Coder, by 4.18% to 10.94% on those two benchmarks. A small model trained for this one task beat larger general ones. Both are learned models, so the result favours narrow, purpose-built checks over general fluency without testing rules against models directly. "Rules decide; language only explains" is the same preference taken one step further.

Relative to that literature: it checks comments against code one method at a time, with no governor and no lifecycle. This checks claims against four instruments across a whole project, or a person's records, with a precision ledger specified on the output but not yet built. That is the whole of the difference.

### Borrowed, not invented

| Component | Source |
|---|---|
| Bi-temporal modelling (valid time and transaction time) | A long-standing database idea (Snodgrass and Ahn, 1985), standardised in SQL:2011, and applied to agent memory by Graphiti / Zep (arXiv:2501.13956) |
| Retracting a belief when its support is withdrawn | Truth maintenance systems (Doyle, 1979). `predicated_on` propagation is a small, human-gated version of the idea. |
| Revising a set of beliefs when new evidence contradicts it | Belief revision theory (Alchourrón, Gärdenfors and Makinson, 1985) |
| Provenance on every claim | W3C PROV (2013). `derived_from` and attribution on edges are a small subset of it. |
| Project / area distinction | Tiago Forte |
| Supersede rather than overwrite | Architecture decision record practice |
| Priority by consequence and onset time; escalation of unaddressed conditions | IEC 60601-1-8, the medical alarm standard |
| Runtime assurance | The Simplex architecture and its descendants. This layer stays out of any control loop: it owns beliefs, and runtime assurance owns the safety envelope. |
| Knowing who knows what (transactive memory) | Wegner (1987). `held_by` and crossings make it explicit. |
| Programming as theory building | Naur (1985): a system's real design lives in the heads of the people who built it, which is why written records about it drift. |
| Double entry | Pacioli (1494). The principle is borrowed. This is not accounting double entry, where both entries are derived mechanically from one transaction. Write "borrowed from", never "implements". |
| Checklists at a defined pause point | Gawande, *The Checklist Manifesto* (2009); used for the read-the-record gate in §8. |

### Already shipped by others

Review-gated writes, supersede-based contradiction detection, point-in-time recall and never-retrieved detection (Link); stale-memory detection on code change (Omni-Memory); team-scale entity registries with code-stamped fields (stigmergy).

**GitHub Copilot Memory** (github.blog, January 2026) is the closest shipped system to the `store ↔ code` instrument, and the closest thing to a disproof of its novelty here. Memories are stored with citations to specific code locations and verified against the current branch when they are used, rather than curated offline. When repositories were deliberately seeded with memories that contradicted the code, agents consistently detected the contradictions and corrected the records, and the pool healed itself with no human review.

Two design choices differ, and both are bets rather than improvements. Theirs verifies lazily, at the point of use; this design verifies on change, which costs more and catches claims nobody happens to ask about. Theirs heals unattended; this design refuses to write without a person, per invariant 1. Their approach is proven at a scale this has never seen. State it that way.

**Slite** detects when documentation has drifted from reality, drafts the fix, and routes every change through human approval. Verified on 13 August 2026 against Slite's own changelog and product pages: its agent cross-references documents against each other and against activity in twenty or more connected tools, proposes fixes, and sends every change through a review queue that shows the full diff before anything is applied. It has shipped on their paid tier since 9 June 2026. The propose-don't-file posture is therefore not distinguishing, and neither is reporting a contradiction between two records.

**Vigil** (trustvigil.com, described by its author in the comment thread of the Karpathy gist on 12 August 2026) is the closest published system to the decision layer of this method. It compiles sources into atomic claims that carry provenance back to the source passage, attaches the assumptions under a decision as explicit falsifiable conditions, and flags the decision for review when new evidence moves one of them. Its stated principle is that the model proposes, the system verifies what it can, and people decide. That is invariant 1, arrived at independently.

The differences are real and narrower than this document first implied. Vigil watches external sources on a schedule and tests them against assumptions; this specification tests claims against a codebase, on commit, with a file and line range carried on the claim. Vigil is a hosted product; this is a specification, most of which §2 records as unbuilt. Neither difference makes premise-based decision invalidation new here. Vigil was posted the day before this document was first published, one scroll below the gist this section opens by crediting, and the search behind this section missed it.

### Found since version 2

**Link 3.0** (16 September 2026) added `lnk stale`, which flags memories that name files git no longer tracks and points to the successor path where git recorded a rename. A path is questioned only if it is missing now and git tracked it before. Its changelog reports zero false flags across 95 path references in the project's own documentation, detection of every deletion probed, and a check that fails if either changes. That is a shipped, measured version of this method's `store ↔ code` instrument for file paths, with the kind of per-rule precision figure §10 asks for. This project still has no precision figure of its own.

**Gilda and Gilda (2026),** "AI-Assisted Engineering Should Track the Epistemic Status and Temporal Validity of Architectural Decisions" (arXiv:2601.21116), proposes separating unverified conjecture from validated knowledge in architecture decisions, and tracking evidence decay so that stale assumptions surface before they cause failures. That is close to this method's `volatility` and `predicated_on`, stated for software architecture, and it predates version 2.

**Jaroslawicz et al. (2025),** "How Many Instructions Can LLMs Follow at Once?" (arXiv:2507.11538), measured instruction following at densities up to 500 simultaneous instructions. The best model tested reached 68% at that density, and models showed a bias toward earlier instructions. It is cited in §12 as outside support for keeping instruction-enforced rules few and putting safety rules first.

**From the same comment thread, September and October 2026.** Several people building in this space have posted work that overlaps with parts of this method. crajah's post-graph-rag resolves contradictions when a fact is written rather than in a later sweep, closes superseded facts instead of deleting them, and keeps a second clock for when the system came to believe something: supersession and bi-temporal storage, built and benchmarked. Obelyth Cortex checks every quoted passage against the cited note at a specific commit, and a save returns its commit hash, with the model told never to claim a save without one; that is a counter-entry for an assistant's claim about its own work, enforced in code. XBlueSky's Cortexes found a transcript filter silently producing unusable records for two weeks, the same class of failure as field notes 6 and 24. A team posting as orcosto-lab audited nine working sessions and found that its agent read its wiki at the start of a session but not in the middle of a task, and re-diagnosed failures that already had a written lesson; field note 22 is a version of the same failure. All of these appeared after version 2, in the thread this section opens by crediting.

### What this project claims

Not novelty. Every mechanism here has a precedent or a close relative listed above, and the search behind this section has missed a close one before. The claim is narrower: this combination (a required counter-entry on every load-bearing claim, a gate keyed to reversibility, return conditions with a second trigger, a capped brief whose footer says what it could not check, and an open-holes list) runs every day in one personal deployment, most of it since late August 2026 and the open-holes list since 20 September, and the field notes record what that turned up, including where it failed. The run logs are private, so this rests on the author's account and the field notes.

Version 2 listed several ideas as possibly new: the required counter-entry, decorrelated instruments as a ranking rule, return conditions rather than dates, the per-rule precision ledger, reversibility as the gate key, cross-party reconciliation, and a design for its own neglect. Version 3 does not repeat that list. Several of these ideas have close relatives above, and three (the precision ledger, cross-party reconciliation and dormancy) are still unbuilt.

**Withdrawn on 14 August 2026: premise-based decision invalidation.** Vigil ships it. Version 2 kept a narrower claim: coupling premise invalidation to a codebase rather than to external sources, and the dormancy lifecycle, for which the nearest published thing found was the ninety-day cycle field in SIGN. Both are still unbuilt.

"Not found elsewhere" means not located by a search the author designed. It is not proof of novelty, and this section has been wrong once already.

---

## 12. Known failure modes

Do not paper over these.

- **Complexity is the central risk.** Karpathy's pattern is three layers and three operations, simple enough that many people built their own in a weekend. Every element added here is another place to drift, and the method's own argument, that systems die when upkeep outgrows value, applies to it.
- **Rules enforced by instructions degrade as the instructions grow.** In the reference deployment, each run's instructions reached roughly 350 instruction sentences, one privacy rule sat 89% of the way through the text, and the morning run's instructions reached 96.5% of the scheduler's 65,536-byte limit (field note 16). Outside measurements point the same way (§11). The mitigation is structural: fewer rules, safety rules first, changing facts stated once, and no new feature unless the instructions get shorter.
- **Confident fabrication is unsolved.** A compiled record answers confidently whether or not the answer is in it. Invariants 1 and 4 are defences, not solutions.
- **An assistant's account of what the system did is the easiest claim to trust by mistake.** It sounds like a report, it is fluent, and it is often an inference (field note 22). The evening run now checks it against the run logs, which are the runs' own account: a closer record, not an independent one (§2).
- **Most of the remaining risk sits in claims that can only ever be single-entry:** verbal promises, judgements about people, reasons given once in a meeting. Marking a claim is not checking it.
- **Views fail silently.** A dashboard can drop what the record holds while still showing fresh numbers elsewhere (field note 6). The record is the truth; a view is only as good as its last test against real output.
- **Append-only is a procedure.** Common cloud document storage does not guarantee revision history. The record stays append-only only because every writer follows the rule; the storage does not enforce it.
- **Detection is the weak link in escalation.** A failsafe that is trusted and silently does not fire is worse than none.
- **Consolidation during dormancy cannot be audited.** It is model-driven compression running when nobody is watching.
- **Decorrelation is assumed, not measured.** Two models built on the same base model are not independent observers.
- **This project is itself an application of the method.** It accumulates decisions and it will drift, so it is reconciled against its own deployment like everything else it describes.

---

## 13. Conventions

For anyone, or anything, writing into this project.

1. **Anchor claims.** Any assertion about behaviour written into documentation or comments carries a file path and a line, or is marked unverified.
2. **Never widen a closed set quietly.** If something does not fit, say so and propose the addition explicitly, as version 3 does in §3.
3. **Do not describe unbuilt components as existing.** Check §2. Prefer "not implemented" to a stub that looks finished.
4. **Assert outcomes, not intentions,** in every test and check. Invariant 2 applies to this project's own checks before it applies to anything else.
5. **Do not overclaim.** Read §11 before writing anything that will be published.
6. **Disagree in writing.** This document is a claim like any other. It can be wrong, and it is subject to the same reconciliation as everything it describes.
