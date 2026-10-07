# Field notes

Dated findings from running Counterentry every day as the record behind my own assistant ([DEPLOYMENT.md](DEPLOYMENT.md)), from August to October 2026. Each entry says what happened and what changed, and how it surfaced where that matters. Together they are the receipts behind most of version 3's changes: under the spec's own rule, a mechanism is added only after its absence has cost something.

Numbering continues from the four receipts published in August, which are in §9 of [the specification](COUNTERENTRY.md) and in [the essay](ESSAY.md).

The underlying run logs and build records are private because they contain personal data. These entries leave out names, amounts, institutions and anything that would identify a person.

---

### 5. The page nobody looked at
*23 August 2026*

**What happened.** A money page on the dashboard was published with its body text squeezed into a 34-pixel-wide column by a grid layout error. Its footer listed everything that had been checked. Rendering the page and looking at it was not on the list.

**How it surfaced.** I opened it.

**What changed.** No page or document is presented until it has been rendered in a headless browser at desktop and phone widths and inspected, including checks for horizontal overflow and for long text trapped in very narrow elements. The render is named in the footer like any other check.

**Lesson.** Render before presenting (§8). A page nobody has looked at is a single-entry claim about itself.

---

### 6. The dashboard dropped six days of work
*3 September 2026*

**What happened.** From about 28 August, the dashboard's loader accepted a task only if it already knew the task's id or the run supplied a section name, and the runs never supplied one. None of the 15 tasks and none of the 22 claims the runs had written reached the page. Money figures did arrive, so the page looked current.

**How it surfaced.** During a system audit, a real data snapshot from the record was replayed through the page in a headless browser and the output was compared with the snapshot.

**What changed.** The loader now renders everything the record carries and labels anything it had to infer. The runs gained field rules so the page never has to infer.

**Lesson.** Test a view against real output, not test fixtures. A view that is partly fresh can hide a total loss (§12).

---

### 7. The first page of a search is not the answer
*3 September 2026*

**What happened.** The dashboard showed the previous evening's data all day while the run logs said it was current. The storage search returned a partial first page of five files, ordered by relevance with one file type first, and that morning's snapshot was a different file type sitting on page two. The same audit found that the search's "contains" operator ignored punctuation and matched the start of words, so a title search for `[card:` also matched an unrelated `cardOpen()`.

**How it surfaced.** Comparing what the page showed with the newest file in storage, in my own browser.

**What changed.** Every read walks all result pages, bounds the search by time, and re-checks titles in code.

**Lesson.** A successful read is not a complete read (§8).

---

### 8. Buttons that never reached the record
*3 September 2026*

**What happened.** Done and Park on the dashboard changed only the browser's local state. Nothing reached the record, so the next run listed the same items again, and other devices never saw the change.

**What changed.** Each button now writes an append-only receipt to the record, and every device reads the receipts back when it loads.

---

### 9. One error, repeated for five days
*7 September 2026*

**What happened.** The record attributed a small recurring charge to a card the system cannot see. Every run for five days repeated the attribution.

**How it surfaced.** The evening run re-reads every money figure after the bank feed refreshes. The re-read found the charge on an account the system can see.

**What changed.** A new entry superseded the old one, and the cause was named: one record had been treated as two.

**Lesson.** Correct an error where it started, by supersession, and record why it spread.

---

### 10. An estimate the measurement contradicted
*9 September 2026*

**What happened.** Asked how long a design could run before the daily data snapshot hit a size limit, the assistant answered "comfortable for weeks". Measured against real snapshots, it was three to five days to the warning line and 16 to 29 days to the hard limit.

**How it surfaced.** My standing rule that figures are measured, not recalled. The measurement ran the same evening and the earlier answer was corrected in the record.

**What changed.** The design changed: money moved into its own document, and the dashboard now writes a fresh full snapshot itself when the running one grows too large. A tripwire was set on the warning line. In the following days the line was crossed on the first day rather than the third, and again on the third, and the tripwire caught and reported both.

**Lesson.** Measure headroom before relying on it, and put a tripwire on any limit that was estimated.

---

### 11. A confident answer from memory
*9 September 2026*

**What happened.** Asked for the most important thing that week, the assistant answered from its saved memory, and the answer was wrong.

**What changed.** Questions about priorities, deadlines or the state of anything are answered only after reading, in order, the newest record entry, the calendar for the next seven days, and the inbox. Memory and summary documents are treated as pointers to where the truth lives, never as the answer. A memory and a document summarising the same session count as one record (§6).

**Lesson.** Check, then state.

---

### 12. Dates written from memory
*9 September and 6 October 2026*

**What happened.** On 9 September the assistant wrote weekdays that did not match their dates and missed a public holiday inside a three-business-day deadline. On 6 October a run found two more errors in the system's own documents: a date labelled with the wrong weekday, and a time converted from UTC that was an hour off.

**What changed.** Every date, weekday, business-day count and time-zone conversion is computed in code from a clock read at the time. None is recalled or copied forward from an earlier document.

**Lesson.** Compute dates; never recall them (§8).

---

### 13. Closed while still open
*10–14 September 2026*

**What happened.** A task inside an unresolved matter was marked done by my own tap while the underlying problem was still open and nothing was scheduled to check on it.

**What changed.** Approved, not yet built: completing a step inside an open matter must name who holds it now and when to check, and it can never default to closed. Work held by someone else carries a `next_check` date and closes only on a record from outside (§5, §6).

**Lesson.** The counter-entry for "done" is an outcome observed outside the actor.

---

### 14. A scheduled job that switched itself off
*13 September 2026*

**What happened.** A job bound to my laptop was due at 01:50 UTC. At 01:50:57 the scheduler set it to disabled, with the reason "device absent": the laptop was asleep. There was no error and no alert: the job was switched off, and nothing said so.

**What changed.** Nothing I depend on runs as a device-bound job. The laptop pushes its exports to storage that the cloud runs already read. A sleeping laptop now produces a stale record, which the runs report, instead of a disabled job, which nothing reports.

**Lesson.** A device that sleeps must push. Never poll it.

---

### 15. Three timing defects, found by measuring
*13 September 2026*

**What happened.** The morning run read a bank feed that refreshed about three hours later, so every morning brief carried figures roughly 21 hours old. The laptop exported messages ten minutes after the run that reads them. Two jobs fired in the same minute three days a week.

**What changed.** Values carry the instrument's own timestamp. A figure read before its source refreshes is labelled as carried, with its age, and is not spoken. The evening run, which follows the refresh, is the day's money check. The other two defects went into a single schedule document that records what fires when, so ordering and clashes can be checked in one place.

**Lesson.** Schedule against the sources, not the clock (§7). For a value read from an instrument, `valid_from` is the instrument's timestamp (§5).

---

### 16. Instructions too long to rely on
*13–20 September 2026*

**What happened.** Each run's instructions had grown to roughly 350 instruction sentences, and the rule that keeps money out of the spoken brief sat 89% of the way through the text. Published measurements show instruction-following accuracy falls as the number of instructions grows, with a bias toward earlier ones (§11). By 20 September the morning run's instructions were at 96.5% of the scheduler's 65,536-byte limit.

**How it surfaced.** Counting. The build script now reports size against the limit on every build.

**What changed.** I set a standing condition: no new feature ships unless the instructions get shorter. One revision removed about thirty rules and moved the money rules to the front. The change history moved out of the instructions entirely (field note 17).

**Lesson.** Rules enforced by instructions degrade with size (§12). Keep them few, and put the ones that must not fail first.

---

### 17. One fact in three places, all three wrong
*19 September 2026*

**What happened.** The time of the afternoon run was written in three places: the change history at the top of the instructions, the step that used it, and the schedule document. After the schedule changed, all three were wrong. Duplication had not protected the fact. It had produced three stale copies.

**What changed.** Each changing fact lives in one place and is read from there. The change history moved out of the instructions, because superseded text left in a prompt gets followed. The build script now flags any changing fact stated twice, and it found two on its first run.

**Lesson.** State an invariant rule as often as you like; state a changing fact once (§8).

---

### 18. Configuration is not behaviour
*19 September 2026*

**What happened.** Twice in one week. For four days the record said an instruction change was still pending, when it had been live the whole time; nobody had read the deployed version back. Separately, the assistant read each job's list of connected tools, concluded that one job could not reach the bank feed, and told me to change the settings. That job's own log showed it had read the bank feed normally, and that a tool on its list had been unavailable.

**How it surfaced.** The run log, read before I acted. The correction reached me first.

**What changed.** "Live" means fetched back from the scheduler and matched by hash. Claims about what a job can do come from its logs.

**Lesson.** Configuration is not behaviour (§8).

---

### 19. A clock-change job that would have reported success
*19 September 2026*

**What happened.** A one-off job, written to move every run by an hour when the clocks change, was checked on 19 September, six weeks before it was due to fire. Its table of runs was already wrong on three rows and missing a fourth. Because it skipped rows it did not recognise, it would have left three runs an hour off and reported success.

**What changed.** Rebuilt, with a first step that tells it not to trust its own table.

**Lesson.** A job that skips what it does not recognise has to say what it skipped.

---

### 20. Nineteen runs of zero
*19 September 2026*

**What happened.** The afternoon run searched a mail address that no longer received anything. It returned zero results nineteen runs in a row. Zero from a dead source looks exactly like a quiet day.

**What changed.** A source that returns nothing five runs in a row now opens a hole (field note 23).

---

### 21. Mail went down, and the brief said so
*18–19 September 2026*

**What happened.** The mail connection dropped and stayed down across three consecutive runs.

**How it surfaced.** Each run reported it in the footer as a check that could not run, rather than reporting no new mail.

**What changed.** Nothing. This is the footer rule doing its job: a check that could not run never reads as a check that passed (§7). Recorded as a pass, because receipts should include the times the method worked.

---

### 22. The assistant claimed absences without searching
*20 September 2026*

**What happened.** In a working session, the assistant reported that several items had never been filed, tracked or surfaced. The run logs showed all of them handled. Every wrong claim was an absence claim, none had been checked against the place the evidence would be, and one test the assistant ran could only have confirmed its own hypothesis. One wrong finding led me to close a real, tracked item.

**What changed.** Before saying what a run did or did not do, the assistant opens that run's log and cites it. An absence claim needs the search that would have found the thing. A finding without a cited source is marked VERIFY. And the evening run now checks the assistant's session notes against the run logs and reports any disagreement, so the check no longer depends on the assistant remembering to do it.

**Lesson.** Read the record before claiming (§8).

---

### 23. Holes get their own list
*20 September 2026*

**What happened.** I put the problem plainly at the time: "I don't see it coming when something's about to break." Faults in the system's own machinery had been treated as housekeeping and closed quietly.

**What changed.** Holes became first-class records (§7): a blind channel, a check that cannot run, a source returning nothing for five runs, a fix waiting on me, or a defect the assistant has carried three sessions unfixed. Each needs a plain-language cost and a next step, and an entry without a next step is not allowed. The footer names at most three, and a hole is spoken only when it is new, when it closes, or when its next step is mine and due today. The list was in its 24th version on 6 October 2026, with seven holes open. The rule I wrote ends: "Silence is earned: the line goes quiet only when nothing is open."

---

### 24. The capture path failed before the inbox
*22 September 2026*

**What happened.** A test of spoken capture from the phone found two captures that never reached the mailbox. The failure was on the phone, upstream of anything the runs can see.

**What changed.** It went on the holes list with its next step, a setting on the phone. The runs kept checking: by 6 October they had recorded 39 consecutive empty checks of the capture mailbox, each confirmed as a real zero rather than a failed read.

**Lesson.** Test a path from its first hop. From inside, an empty mailbox cannot tell "nothing was sent" from "nothing arrived".

---

### 25. The record could not say "settled" or "unreachable"
*22 September – 6 October 2026*

**What happened.** The deployment's record gives a claim one of four statuses: agree, single, disagree, or nothing. Two cases kept coming up that none of them could express. A disagreement that had been resolved could only be written as "agree" with a note, so a reader could not tell a live disagreement from one settled weeks earlier. And a charge with a clear receipt, paid from an account the bank-feed connection cannot see, was single-entry for good: no second record could ever be obtained with the instruments available. "Single" says only one record exists. It does not say that looking for another is pointless.

**What changed.** Version 3 of the spec adds the entry status `unreachable`, and settles disagreements by superseding the losing claim rather than adding a status (§3, §6). The deployment has not adopted it yet.

**Lesson.** A claim can be double-entry in one part and single-entry in another: that a charge exists, and whose it is. Split it.

---

### 26. A watchdog that cried wolf
*23 September – 4 October 2026*

**What happened.** The watchdog that alerts me when a run's log looks missing fired early, by eight seconds, at least three times, each time a false alarm.

**What changed.** It went on the holes list with its closing condition written down in advance: a night with no false alarm. The run that wrote the condition predicted the alarm would fire again. It did not, and on 4 October the hole closed on that check rather than on anyone's impression. One quiet night is thin evidence. What the method adds is that the bar was set before the result was known, so the result could not move it. There have been no false alarms since, as of 6 October.

**Lesson.** Write the closing condition before running the test.

---

### 27. A read that succeeded and stopped
*3–6 October 2026*

**What happened.** The daily export of text messages grew past the size a single document read returns. The read reported success and simply stopped partway through the last conversation in the file, so whether a conversation was visible depended on where it happened to fall. Four exports in a row were cut the same way.

**What changed.** It is on the holes list with two priced options: split the export into one document per conversation, or order it so the quietest threads fall at the end. Neither is built.

**Lesson.** A successful read is not a complete read (§8). A run that does not check the end reports "nothing from X" when the truth is "could not look".
