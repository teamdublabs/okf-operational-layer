# Beyond the Format: The Operational Layer OKF Leaves Out

*The hard part of an AI knowledge base isn't writing it down — it's keeping it true.*

*by Team Dub Labs — [teamdub.com](https://teamdub.com)*

---

## A format is a snapshot. Knowledge is a process.

In June 2026, Google Cloud published the **Open Knowledge Format (OKF)** — a deliberately small, open spec for writing down what an organization knows as plain Markdown files with a little YAML frontmatter, so AI agents can read it directly. It's a good standard: vendor-neutral, Apache-2.0, no runtime, no SDK. And it formalizes a pattern many of us were already building toward — Andrej Karpathy sketched the "LLM wiki" idea, a small community shipped implementations through the spring of 2026, and OKF gave the result a name and a contract.

But there's a quiet assumption in every "just put your knowledge in OKF" walkthrough: that writing it down is the hard part.

It isn't. The hard part is everything *after* the first commit.

The day you finish authoring your OKF bundle is the day it starts to rot. A price changes, a runbook goes stale, a new decision quietly contradicts an old one — and nobody updates the file. Six weeks later, your tidy, standards-compliant knowledge base is confidently feeding your agents things that are no longer true.

OKF gives your knowledge a **body**. This piece is about what it deliberately leaves out: the **metabolism**.

## What OKF is — and what it isn't

The gap is by design, so it's worth being precise.

OKF specifies a *format*: a directory of Markdown concept files, one required frontmatter field (`type`), a few optional ones (`title`, `description`, `resource`, `tags`, `timestamp`), two reserved files (`index.md`, `log.md`), and plain Markdown links to wire it all into a graph. That's essentially it — and the minimalism is the point. A standard that's easy to adopt and can't lock you in is exactly what a standard should be.

But a format can't tell you:

- How new knowledge **gets in**.
- How it's **classified** and where it belongs.
- How it gets **linked** to everything related.
- How scattered notes become a **synthesized** page.
- How you know a claim is **still true** — or where it came from.
- What happens when two files **contradict** each other.
- Who does all of this, **forever**, without getting bored.

A format describes knowledge at rest. It says nothing about the lifecycle that produces and maintains it. Call that lifecycle the **operational layer** — and it's where every real knowledge base lives or dies.

## The operational layer, primitive by primitive

You don't need to see our code to grasp the shape of the problem. Here are the primitives a *living* OKF base needs — the map, not the recipe.

**1. Capture.** If adding knowledge takes effort, it won't happen. You need a zero-friction drop point — a URL, a voice memo, a pasted thread — that the system, not the human, turns into a proper concept file. The moment capture becomes a chore, the base goes stale at the source.

**2. Classification.** A captured fragment has to become a *typed* concept in the right place. That's OKF's mandatory `type` field — but the spec only says it must exist, not how to assign it. Deciding "this is a `decision`, it belongs under Operations, here are its tags" is judgment at volume, which is exactly what LLMs are for.

**3. Cross-linking.** OKF's graph is built from links, and a graph with no edges is just a pile. The reason a knowledge base beats plain search is that the connections are *already there* before you ask. Maintaining them as the base grows is relentless and never finished — perfect for a machine, miserable for a person.

**4. Synthesis.** Raw notes aren't answers. Karpathy's key move is *persistent compilation*: an LLM that reads many sources, writes one coherent, cross-referenced page, and rewrites it when the sources change. This is what makes a wiki *compound* instead of merely pile up — and it's the step most "just use OKF" advice skips.

**5. Provenance.** A claim you can't trace is a claim you can't trust, and an agent that can't cite its sources isn't one you'll deploy anywhere that matters. Every synthesized statement should link back to where it came from. This isn't decoration — it's the load-bearing wall, because…

**6. Semantic maintenance.** …it's what lets the base catch its own decay. Structural checks — broken links, missing fields — are table stakes. The real work is *semantic*: two files that contradict each other, a claim a newer source has superseded, a concept mentioned everywhere but documented nowhere, a topic with suspiciously thin coverage. This is the immune system — and a format has no opinion about it at all.

**7. Scheduling.** This is what separates a demo from a system. Every primitive above *can* be run by a human "when they get to it." They won't. Karpathy's original insight was simple: **LLMs don't get bored.** Capture, linking, synthesis, and maintenance have to run unattended, on a schedule, in the small hours, with nobody in the loop. A knowledge base kept alive by good intentions is already dead — it just doesn't know it yet.

**8. Reversibility.** Anything automated that *changes* files has to be safe: every change committed to version control, every run reversible, every mutation reviewable. Automation without an undo button is how you wake up to a base that confidently rewrote itself wrong.

Eight primitives. OKF specifies the artifacts that *one* of them produces. The other seven are wide open — which is exactly why they're worth owning.

## Why "just export to OKF" isn't enough

The tempting shortcut is to treat OKF as a finish line: run a script, dump your docs into compliant files, declare victory. That gets you a *snapshot* — and snapshots are the thing that rots.

It also misses why this pattern beats retrieval-augmented generation (RAG) in the first place. RAG rediscovers everything on every query, from scratch, with no memory that it ever reasoned about this before. The LLM-wiki bet — the one OKF encodes — is the opposite: **compile once, compound forever.** The cross-references are already there. The contradictions have already been flagged. The synthesis already happened last night, while you slept.

But that only pays off if something is doing the compiling, *continuously*. A static OKF bundle has the body of a compounding knowledge base and none of the metabolism. It looks identical on day one — and drifts a little further from reality every day after.

The format is necessary. It's nowhere near sufficient.

## Standing on shoulders (and saying so)

None of these primitives are mine, and pretending otherwise would be both dishonest and pointless. Karpathy articulated the LLM-wiki concept. A community of implementers worked out the conventions — index files, append-only logs, source/concept separation, citation discipline, review gates — months before any of it carried a Google logo. OKF then did the real work: turning those conventions into a portable, vendor-neutral contract.

Our claim is narrower. We've been *running* this — the full operational layer, scheduled and unattended, on a real knowledge base — since before the standard existed. Building the format is a weekend. Building the metabolism that keeps it true, and trusting it enough to let it rewrite your knowledge while you sleep, takes real engineering and real mileage. That's the part most OKF adoption stories quietly skip, and the part that decides whether your agents are working from the truth.

## The takeaway

If you're adopting OKF, go in with eyes open:

- The format is the easy 20%. The operational layer is the 80% that decides whether it works.
- A knowledge base isn't a document you write; it's a system you run.
- Automate the boring, relentless maintenance — or accept that your base will quietly drift out of date and take your agents' credibility with it.
- Keep humans where humans are good (curating, asking, deciding), and machines where machines are good (the bookkeeping nobody sustains).

OKF gave organizational knowledge a standard, portable body. The work worth doing — the work worth hiring for — is giving it a metabolism.

---

### About

**Team Dub Labs** builds the automation layer that keeps OKF knowledge bases alive — capture, classification, cross-linking, synthesis, and semantic maintenance, running unattended. Adopting OKF and want a system, not a snapshot? Get in touch: [contact@teamdub.com](mailto:contact@teamdub.com) · [teamdub.com](https://teamdub.com)

*This article is released under [CC BY 4.0](LICENSE). Discussion and issues welcome.*
