# Beyond the Format: The Operational Layer OKF Leaves Out

*The hard part of an AI knowledge base isn't writing it down — it's keeping it true.*

*by [Your Name] — [your site / contact]*

---

## A format is a snapshot. Knowledge is a process.

In June 2026, Google Cloud published the **Open Knowledge Format (OKF)** — a deliberately small, open specification for writing down what an organization knows as plain Markdown files with a little YAML frontmatter, so AI agents can read it directly. It's a genuinely good standard: vendor-neutral, Apache-2.0, no runtime, no SDK. And it formalizes a pattern a lot of us had already been building toward. Andrej Karpathy sketched the "LLM wiki" idea; a small community shipped implementations through the spring of 2026; OKF gave the result a name and a contract.

But there's a quiet assumption baked into every "just put your knowledge in OKF" walkthrough: that the writing-down is the hard part.

It isn't. The hard part is everything that happens *after* the first commit.

A format is a snapshot. Knowledge is a process. The day you finish authoring your OKF bundle is the day it starts to rot — a price changes, a runbook goes stale, a new decision quietly contradicts an old one, and nobody updates the file. Six weeks later your beautiful, standards-compliant knowledge base is confidently feeding your AI agents things that are no longer true.

OKF gives your knowledge a **body**. This piece is about the thing it deliberately doesn't specify: the **metabolism**.

## What OKF is — and what it intentionally isn't

It's worth being precise, because the gap is by design, not by oversight.

OKF specifies a *format*: a directory of Markdown concept files, one required frontmatter field (`type`), a handful of optional ones (`title`, `description`, `resource`, `tags`, `timestamp`), a couple of reserved files (`index.md`, `log.md`), and plain Markdown links to wire concepts into a graph. That's essentially it. The minimalism is a feature — a standard that's easy to adopt and impossible to lock you in is exactly what a standard should be.

But notice what a format *can't* tell you:

- How a new piece of knowledge **gets in**.
- How it's **classified** and where it belongs.
- How it gets **linked** to everything related.
- How scattered notes get **synthesized** into a coherent page.
- How you know a claim is **still true** — or where it came from.
- What happens when two files **contradict** each other.
- Who does all of this, **forever**, without getting bored.

A format describes the resting state of knowledge. It says nothing about the lifecycle that produces and maintains that state. Call that lifecycle the **operational layer**, and it's where every real knowledge base lives or dies.

## The operational layer, primitive by primitive

You don't need my code to understand the shape of the problem. Here are the primitives a living OKF knowledge base needs — the map, not the cookbook.

**1. Capture.** If adding knowledge has any friction, it won't happen. You need a zero-effort drop point — a URL, a voice memo, a pasted thread — that the system, not the human, turns into a properly-formatted concept file. The moment capture becomes a chore, the base goes stale at the source.

**2. Classification.** A captured fragment has to become a *typed* concept and land in the right place. This is exactly OKF's mandatory `type` field — but the spec only says it must exist, not how to assign it. Deciding "this is a `decision`, it belongs under Operations, here are its tags" is judgment work, and judgment work at volume is what LLMs are for.

**3. Cross-linking.** OKF's graph is built from Markdown links, and a graph with no edges is just a pile. The compounding value of a knowledge base — the reason it beats search — comes from the links being *already there* before you ask. Maintaining those links as the base grows is relentless, boring, and never finished. Perfect for automation; miserable for a human.

**4. Synthesis.** Raw notes aren't answers. The leap Karpathy named is *persistent compilation*: an LLM that reads many sources and writes a single coherent, cross-referenced page — and rewrites it when the sources change. This is what makes a wiki *compound* instead of merely accumulate. It's also the step most "just use OKF" advice skips entirely.

**5. Provenance and citation.** A claim you can't trace is a claim you can't trust — and an agent that can't cite its sources is one you can't deploy in anything that matters. Every synthesized statement should link back to the concept it came from. This isn't decoration; it's the load-bearing wall under everything else, because…

**6. Semantic maintenance ("lint").** …it's what lets the system catch its own decay. Structural checks (broken links, missing fields) are table stakes. The real work is *semantic*: flagging two files that contradict each other, claims a newer source has superseded, concepts mentioned everywhere but documented nowhere, topics with suspiciously thin coverage. This is the immune system of a knowledge base, and a format has no opinion about it whatsoever.

**7. Scheduling.** Here's the one that separates a demo from a system. Every primitive above can be run by a human "when they get to it." They won't. The entire premise — Karpathy's original insight — is that **LLMs don't get bored.** Capture, linking, synthesis, and lint have to run *unattended, on a schedule*, in the small hours, with no human in the loop. A knowledge base maintained by good intentions is already dead; it just doesn't know it yet.

**8. Reversibility.** Anything automated and mutating must be safe. Every change committed to version control, every run reversible, every mutation reviewable. Automation without an undo button is how you wake up to a knowledge base that confidently rewrote itself wrong.

Eight primitives. OKF specifies the artifacts that *one* of them (classification) produces. The other seven are wide open — which is precisely why owning them is worth something.

## Why "just export to OKF" isn't enough

The seductive shortcut is to treat OKF as a destination: run a script, dump your existing docs into compliant files, declare victory. That gets you a *snapshot* — and snapshots are exactly the thing that rots.

It also misunderstands why this pattern beats retrieval-augmented generation (RAG) in the first place. RAG rediscovers knowledge on every query, from scratch, with no memory that it ever reasoned about this before. The LLM-wiki bet — the bet OKF encodes — is the opposite: **compile once, compound forever.** The cross-references are already there. The contradictions have already been flagged. The synthesis already happened last night while you slept.

But that bet only pays out if something is *doing the compiling, continuously.* A static OKF bundle has the body of a compounding knowledge base with none of the metabolism. It looks identical on day one and diverges from reality a little more every day after.

The format is necessary. It is nowhere near sufficient.

## Standing on shoulders (and saying so)

None of the primitives above are mine, and pretending otherwise would be both dishonest and unnecessary. Karpathy articulated the LLM-wiki concept. A community of implementers worked out conventions — index files, append-only logs, source/concept separation, citation discipline, draft-and-review gates — months before any of it had a Google logo on it. OKF then did the genuinely valuable work of turning those conventions into a portable, vendor-neutral contract.

What I'll claim is narrower and, I think, more useful: I've been **running** this — the full operational layer, scheduled and unattended, on a real knowledge base — since before the standard existed. Building the format is a weekend. Building the metabolism that keeps it alive, and trusting it enough to let it rewrite your knowledge while you sleep, is the part that takes real engineering and real mileage. That's the part most "OKF adoption" stories quietly leave out, and it's the part that actually determines whether your AI agents are working from the truth.

## The takeaway

If you're adopting OKF, adopt it with eyes open:

- The format is the easy 20%. The operational layer is the 80% that decides whether it works.
- A knowledge base is not a document you write; it's a system you run.
- Automate the boring, relentless maintenance — capture, linking, synthesis, lint — or accept that your base will quietly drift out of date and take your agents' credibility with it.
- Keep humans where humans are good (curating, asking, deciding) and put machines where machines are good (the bookkeeping nobody sustains).

OKF gave organizational knowledge a standard, portable body. The interesting work — the work worth hiring for — is giving it a metabolism.

---

### About

*[Your Name] builds the automation layer that keeps OKF knowledge bases alive — capture, classification, cross-linking, synthesis, and semantic maintenance, running unattended. [Reach out / your site] if you're adopting OKF and want a system, not a snapshot.*

*This article is released under [CC BY 4.0]. Discussion and issues welcome.*
