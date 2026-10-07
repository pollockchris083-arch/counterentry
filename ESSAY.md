# A knowledge base that notices when it stops being true

> **October 2026.** This is the essay that introduced the method in August 2026, edited on 6 October 2026 for privacy and clarity. The method has moved on since, and the per-rule numbers promised at the end have not been produced yet; sections 2 and 10 of [the specification](COUNTERENTRY.md) say why. For what changed, and what six weeks of daily use turned up, see [CHANGELOG.md](CHANGELOG.md) and [FIELD-NOTES.md](FIELD-NOTES.md).

> **Correction, 14 August 2026.** The section below headed *The part I haven't found anywhere is what to do about decisions* is wrong as written. Vigil, described by its author in the comment thread of the Karpathy gist on 12 August 2026, attaches assumptions to decisions and flags the decision when new evidence moves an assumption. Section 11 of the spec now records this and the corresponding novelty claim has been withdrawn. That section is kept as written in August, apart from one example changed on 6 October 2026 for privacy.

I keep records for a handful of small software apps. Ordinary stuff: what each app does, who it's for, what was decided and why.

One of those records said a dashboard required admin access.

The code said something else. The permission check returned `true`. Every visitor, every time, since the day the line was written. There was no gate. There had never been a gate. It was fixed that afternoon.

Here's the part that got me: nothing in place could ever have caught it. It didn't crash. It didn't throw an error. It did exactly what it was written to do. Error monitoring watches for failure, and this wasn't a failure. It was a function working perfectly while also being wrong.

It only surfaced when I put two records of the same app side by side and asked one question: do you agree?

I've spent my time since building a system that asks that question every night, so nobody has to remember to. Here's what I built, what it's caught, what's wrong with it, and what I'm measuring next.

## How I got here

I was building an app of my own, on nights and weekends. The thing that kept stopping me wasn't the code. It was that every session with my AI coding agent started from zero. I'd re-explain the same constraints. I'd re-argue decisions I'd already settled and forgotten the reasons for.

So I started keeping notes the agent could read. Basically the pattern Andrej Karpathy published in April as the "LLM Wiki": keep your knowledge in plain files, let the model write and organize them, ask your questions against that record. If you haven't read his gist, go read it. It's one page and it's the best statement of the idea anyone has written. The foundations of what I'm describing are his.

Then two of my systems failed in the same season, and the failures had the same shape.

The first was the app. I asked my own notes what state something was in, got a confident answer, and built on it. And the notes described what I had intended to build, not what had actually shipped. The record agreed with itself completely and was wrong. Nothing failed, so nothing flagged it.

The second was dumber and more expensive. A vehicle service I kept meaning to get to, with nothing ever set to bring it back, turned into a four-figure repair. That's the kind of bill that quietly rearranges your next few months. My notes didn't fail there. I never wrote it down at all, because meaning to get to it felt like a plan. It wasn't a plan. It was a wish, and then not even that.

Same failure, twice: something I believed quietly stopped matching reality, and nothing was checking. Code has tests. Servers have monitoring. Money has had reconciliation since 1494. What we believe has no feedback loop at all.

That repair bought one of my rules outright: **a thing you put off without a trigger to bring it back is a thing you deleted.** Everything I park now comes back on a condition. "Next time I touch the brakes." "If a second customer asks." Never on my memory. That rule cost me more than any book I've bought.

## The gap

Karpathy's pattern includes a health check he calls lint. It looks for contradictions between pages, stale claims, orphans, and gaps a web search could fill. For a research vault, which is what the pattern was built for, that's the right check.

But it checks the notes against the notes. When your notes describe a running system that changes, a claim can be wrong the day it's written with nothing inside the vault to contradict it. My admin-gate record was never contradicted by any note I had. It was contradicted by one line of code the notes had never read.

So the change I made is small to say: **compare against something outside, and let reality moving be the trigger.**

## What I changed

**Notes about code point at the code.** A claim about software records exactly which file and lines it's describing, and what that code looked like at the time. The design: each night, the system checks what changed in the codebase and flags every note resting on changed code. Not on a review schedule. The night the code moves. I'll be precise further down about how much of that actually runs unattended today.

I should say plainly: that part isn't novel anymore. Builders responding to Karpathy's gist have shipped pieces of it. GitHub's Copilot memory team published work in January that checks memories against the codebase, tested it by deliberately planting false ones, and showed the system healing itself, with no human review at all. That's a genuinely different bet than mine, and their write-up is worth your time. Slite, on the team-wiki side, describes detecting when documentation has drifted from reality and routing every fix through human approval. That is most of my second half too. If you're weighing whether any of this is new, start with those two.

**The part I haven't found anywhere is what to do about decisions.** A decision doesn't live in any file. And here's what took me embarrassingly long to see: the notes you can check against code are mostly things you could re-learn by reading the code. The expensive notes exist nowhere else: why we ruled something out, what we traded away, what we promised. My strongest mechanism was protecting my cheapest information.

The fix: every decision records the reasons underneath it, and each reason is its own checkable note. "We ruled out separate logins because everyone shares one computer." When a reason stops being true, every decision built on it gets flagged that night.

The difference in practice: without this, the system can tell you "you ruled that out in June." With it, it can tell you "you ruled that out because everyone shared one computer, and that stopped being true in March. The reason for your decision is gone. Want to reopen it?"

The first is a record. The second is the thing I actually wanted.

## What decides what I see

Systems like this die the same way: they start talking too much, you start skimming, and six weeks later it's decoration. Clinical alert research puts numbers on it: in a study of primary-care clinicians, the likelihood of accepting a reminder dropped roughly 30% for each additional one received.

So nothing here gets to talk by default. Every checking rule starts silent. It has to be right often enough, across enough real cases, to earn a spot in the daily brief. And if it starts being wrong, it loses the spot on its own. Nobody tunes it. It tunes itself, off the one signal that costs me nothing: whether I acted on what it showed me.

That gives me two numbers for free: how often the flags were real, and how often I did something about them. If either goes flat, the system is turning into furniture, and it says so itself.

## The part nobody designs for: me disappearing

Adding to this system costs nothing: I say "add this" mid-conversation and it files a proposal. But approving takes me. So the queue grows, and the promise that the inbox "empties overnight" was false. It empties when I do. Three quiet weeks and it becomes exactly the pile it was built to prevent, now with guilt attached.

People aren't consistent, and a method that only works under maintenance is a hobby. So neglect is part of the design. On nights I'm gone, the system may only subtract: merge duplicates, close out proposals that later evidence already answered. It may never write anything new while nobody's watching, because a model rewriting my records unsupervised is the exact failure this whole thing exists to catch.

And when I come back, the backlog is sorted by what's rotting fastest, not what's oldest. A note that rests on my memory of a conversation gets harder to verify every week. A note anchored to code is as checkable in a year as it was on day one. The dying ones surface first.

## What it's like on a normal day

Last month I asked it, in plain words, whether a trip I wanted to take this year was realistic. Because my whole life lives in this structure, not just the apps, the answer came back with things I hadn't put in the question: money already spoken for, two family commitments landing the same season, and, when I asked how to close the money gap, an honest read of which of my own projects was closest to producing income. Then it went to the web for the rest.

To be clear about what that is: retrieval over context I'd already captured. None of it is the novel part of this post. A well-organized project with any capable model gets you most of it. But it's what makes the upkeep worth doing, and it's what the checking half exists to protect. An assistant with your whole context is only as good as that context is current, and context rots silently. One half makes the answers possible. The other half keeps them from being confidently wrong.

## What it's actually caught

Four things, and four is not a large number.

The admin gate above. A silent data-loss path in the same app: it had been accepting entries all day on a dead session, no error, and hours of entries could vanish. Also fixed. A belief about what set my project apart, killed by a nightly web check before I said it out loud to anyone who mattered. And a pipeline that reported success for two days while moving almost nothing: the plumbing worked perfectly; nothing was ever put in front of it.

That last one produced the rule I'd keep if I lost everything else: **a check must assert the outcome, never the intention.** "Available for processing" is a hope. "Processed and verified" is a fact.

## The case against this

I'd rather make it myself than watch someone else make it better.

**It's heavier than the pattern it extends.** Karpathy's gist is three layers and three operations; thousands of people built it in a weekend. Mine is six nightly passes and a lot of structure. And the argument that kills knowledge systems, upkeep outgrowing value, applies to mine more than his.

The bug that started all this needed almost none of the architecture: one record compared against one file. Four findings don't justify the rest, and I won't pretend they do.

**Nothing is measured yet.** The self-tuning I described has never actually run. That's what the shadow run is for.

**The loop is only partly automated.** The morning brief arrives on a scheduled task now, but the deeper passes still run when I sit down and run them: the code comparisons, the cleanup. I'd rather say that plainly than let "every night" sound more finished than it is.

**Confident nonsense is unsolved.** A record like this answers confidently whether or not the answer is really in it. My defenses are that nothing gets written without my sign-off, and that "I have nothing solid on this" is an allowed answer that never gets saved as if it were knowledge. Those are defenses, not solutions.

And every scaling objection applies, untested. Small scale, no access control on the record itself. I haven't solved those problems. I just haven't hit them yet.

## Why I think the shape matters anyway

After building most of this, I read a meta-analysis out of MIT, from Vaccaro, Almaatouq, and Malone in Nature Human Behaviour, pooling over a hundred experiments on humans and AI working together. The headline is uncomfortable: on average, the combination did worse than the better of the two alone.

The exception is the whole story, and I want to state it as they did rather than in the shape I'd like it to have: when humans outperformed AI alone, the combination produced gains; when the AI outperformed humans alone, it produced losses.

That's the finding. What follows is my reading of it, not theirs: the way a human outperforms a model is by knowing things it doesn't. Context, decisions, reasons, the stuff in no training set. And that is exactly what decays silently. My notes said what I intended, not what shipped. My side was leaking, and when it leaked far enough I wasn't collaborating anymore. I was approving.

So the honest description of this system: it's machinery for keeping the human's side true. I didn't build it because of the research. I built it because I was frustrated, and the research explained afterward what I'd been doing.

I've been calling it **Counterentry**. In double-entry bookkeeping the counter-entry is the second half of the pair, the one that makes the first checkable. Nothing here is true on one entry; every claim that matters carries a second, independent one, and the whole job is finding the pairs that stopped agreeing. The checking happens while I'm not there, and what reaches me in the morning is only what changed.

## What happens next

I'm running the checking rules silently across those apps: logging every flag, marking each one real or false, letting none of it reach me in a brief. When there's enough data to judge each rule honestly, I'll publish the per-rule numbers, whatever they say. If they're bad, that's a result too, and you'll get it uncut. No date on that, on purpose: my own system's rule is that things come back on conditions, not calendars. The condition here is enough findings to be honest.

The full spec is at https://github.com/pollockchris083-arch/counterentry if you want to build your own or take it apart. It's meant to be argued with.

The piece I'm least sure about is flagging decisions when their reasons die. One reason changing can light up a dozen decisions, and I don't know yet whether that's signal or noise. The numbers will tell me. If you've tried anything like it, I'd genuinely like to hear how it went.

---

Chris Pollock, August 2026. This grew out of needing my own notes to stop lying to me.
