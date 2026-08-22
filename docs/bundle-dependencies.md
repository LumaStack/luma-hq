---
type: document
title: Bundle dependencies
description: How a bundle depending on another bundle should work — flat resolution, one version per project, and why context rather than convenience decides it.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-21T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Bundle dependencies

**Draft. Nothing here is settled.** This is a design worked out in one sitting
and written down so it can be argued with rather than re-derived. It is not in
`DECISIONS.md` on purpose: if it were, its claims would read as positions the
organization has taken, and they are not yet.

**What it would change if adopted.** Bundles are currently self-contained and
depend on nothing, and that is the stated reason there is no solver, no version
negotiation, and no dependency graph. This proposes that bundles may depend on
other bundles.

**The reason is narrow and does not generalize.** For **prose**, the alternative
to a dependency is duplication — and two copies of a policy drift, then
contradict each other, with nothing recording which is current. That is worse
than the resolution problem. For code the same trade goes the other way, which is
why this is not an argument that bundles should behave like packages.

## Flat, one version, no nesting

**Context is a single namespace with no scoping mechanism, and that decides the
whole design.** A package manager can nest because two versions of a library are
two objects with separate call sites. Two versions of a policy in one context
window are contradictory instructions to one reader. There is no `require` scope
for prose, so nesting is not merely undesirable — it has no meaning.

So: **one version of any bundle in a project, ever.** Dependencies sit side by
side. Nothing contains anything.

**Type definitions make this sharper rather than softer.** If two bundles want
different field sets for the same type, records have already been written to disk
against one of them. Prose read under the wrong version can at least be reasoned
about; a record written against a schema that no longer exists is silently wrong
and cannot be failed out of after the fact.

## A dependency is transitive adoption, and nothing more

With no nesting, no scoping and one version, *A depends on B* is operationally
identical to **adopting A also adopts B**. There is no second mechanism, no
separate dependency store, no link step.

The manifest records, per entry, whether it is there because someone asked for it
or because something they asked for required it. That flag exists for one
purpose: **removal**. Drop A and B goes with it, unless B was asked for directly
or something else still requires it.

## Resolution takes the current version, and fails only across majors

**Current, always.** An old policy is a superseded policy — somebody edited it
because the previous rules were wrong, so deliberately resolving backwards means
running rules that were replaced on purpose. The conservatism that makes
minimal-version selection right for code is wrong here, because for code an old
version still works and for policy it does not.

**A major difference is the one hard failure.** A bundle states the major it is
compatible with; two requirements naming different majors cannot both be
satisfied, and no arrangement rescues it, because nothing can nest. There is no
resolution step beyond this: no candidate selection, therefore no backtracking,
therefore no solver.

## A constraint may be as tight as its author wants; the norm is loose

**Anyone may express a dependency however they need to** — from "any version of
this major" down to an exact patch. What differs is the **default**, and what a
tight constraint costs whoever else is nearby.

**The norm is the whole major line, and defaults to it.** Depending on `2` means
accepting every `2.x.x` — every minor and every patch, without review. A bundle
author states the loosest thing they can tolerate, and that is almost always the
major line. Minor and patch carry little information for prose — a policy has no
interface, so an addition can change behaviour more than a restructure — and
pretending otherwise buys precision that is not real.

**Two different needs wear the same syntax, and they must not be confused.**

| | what it claims | who is affected |
|---|---|---|
| **a bundle constraining its dependency** | *I am compatible with this* — a correctness claim | everyone who adopts it |
| **a project pinning what it adopted** | *I do not move without review* — a change-control policy | only that project |

**The second is the compliance case, and it is already largely satisfied by
vendoring.** Content is committed, so nothing moves until somebody runs an update
and commits the result. A compliance team is protected by the architecture before
any pin is written. What a pin adds is refusal — *do not even offer me a newer
one, and fail if something tries to pull one in* — which is a legitimate thing to
want and costs nobody else anything, because it lives in that project's own
manifest.

**The first is where tightness has a price.** A bundle pinning its dependency
narrowly constrains every other bundle that shares that dependency. This would be
permitted, with the catalog entitled to reject a bundle whose constraint cannot
coexist with what it already holds. **A tight constraint is a claim its author has
to be willing to defend**, because the cost of it falls on people who never chose
it.

**Different teams should reach different answers, and that is correct rather than
a failure of standardization.** An engineering team wants the newest policy,
because for prose the newest is usually the best and often the only one worth
caring about. A legal or compliance team wants nothing to change without review.
Both are right about their own situation, and the constraint syntax is where that
difference gets stated instead of argued.

### A narrow constraint has to say why

**A bundle constraining more narrowly than a major line states a reason, and
publication rejects it otherwise.** One field, one sentence, and only on the
narrow case.

The rule exists because **the cost of a tight constraint falls on strangers.** An
author pinning to a patch is spending other people's flexibility: every bundle
sharing that dependency is now constrained by a decision none of them made, and
the person who eventually hits the publication failure has to work out whose
caution caused it. Nothing about a version expression announces that it was
deliberate rather than copied from an example, and a default nobody notices
violating is not a default.

This is the same move as **reporting what a tag change costs**, and as **making an
exemption a sentence rather than a pattern**: it does not forbid the thing, it
makes doing it quietly impossible. *"Pinned to `2.1.4`: our regulator requires
policy changes to be reviewed before adoption"* is a sentence somebody stands
behind. `2.1.4` on its own is indistinguishable from carelessness.

**It is asked of bundles, not of projects.** A project pinning its own adoption
affects nobody else and owes nobody an explanation; the asymmetry follows the
table above and is the whole point of separating the two cases.

**The catalog would report its own tightness.** A catalog doctor listing every
constraint narrower than a major line with its stated reason. Individual pins are
defensible and a catalog drifting toward tightness is a problem nobody would
otherwise see, because each one was reasonable on its own.

**What this does not solve:** a required reason catches carelessness, not bad
judgement. Somebody who believes their pin is necessary writes a sentence and
publishes. What stops that is a reviewer disagreeing with it, which is a
catalog-editor job rather than a mechanism.

## The conflict check runs at publication *and* at installation

**Both, and they are not the same check.**

**At publication**, because that is where the only person who can fix it is
standing. A catalog is curated rather than open, so it can refuse a bundle whose
requirements conflict with what it already holds. The author is present, owns the
bundle, and can change it. Letting the conflict through means it surfaces later in
front of an adopter who owns neither bundle and whose only recourse is forking —
which is a worse position than a package-manager user is in, since they at least
can nest.

**At installation**, because publication-time consistency is a property of *one
catalog at one moment*, and a project is not that. A project adopts across an
organization's catalog and the universal one, from forks, and from its own local
bundles — combinations no single publisher ever saw. Catalogs are forkable and
vendored copies are editable. **A check that runs only where you control both ends
stops running the moment somebody forks**, which is the same fail-open shape this
design rejects everywhere else.

So publication catches it early and cheaply; installation catches what
publication structurally cannot see. Neither makes the other redundant.

## What enters context is unchanged by who pulled it in

**A depending bundle does not decide what loads.** *Preload is declared by whoever
holds the knowledge* — a settled decision in [DECISIONS.md](DECISIONS.md) — already
covers this and needs no amendment: the bundle author declares document-level
`preload`, and the adopter decides bundle-level `preload_default` outright. A
dependency changes what is *available*; it does not change what is *present*.

**Adoption decides availability. Routing decides presence.** That is what keeps
dependencies affordable — a bundle adopted but not relevant to the work in hand
costs disk rather than context. The fixed cost of an adoption is only its
genuinely unconditional content, and a bundle with a large unconditional
footprint has a defect that is visible as a number.

**Adoption would report its transitive context cost before it is accepted.**
*"Adopting A also brings B and C; four documents become preloaded."* No package
manager reports this because no package manager has a budget to spend. Without it,
a dependency chain is a permanent context tax that nobody chose and nobody sees.
`preload` is declarative, so the number is computable before anything is loaded.

## Compatibility is recorded, not predicted

A version range is a claim about content that does not exist yet, made by
somebody who cannot know it. Instead, a bundle records what it was **verified
against** — a fact about work someone did. Running a different version than the
recorded one produces a notice rather than a failure, silenceable permanently by
anyone who checks the combination and records it.

**This matters more for prose than it would for code**, because semantic
versioning describes an interface and a policy has no interface — its content is
its effect. A minor addition can change behaviour more than a major restructure
does. So compatibility accumulates because people confirm it, not because a
number implied it.

**Cross-bundle links would be checked at resolve time.** If a document in A links
to a document in B that is not present at the resolved version, that is a finding.
For code a broken assumption fails a test; for prose nothing happens at all — an
agent follows a dangling link, finds nothing, and proceeds confidently. This check
is the substitute for a compile step, and without it a recorded compatibility
claim is a promise nobody verifies.

## If this were built

- Resolution order: collect requirements transitively; fail if constraints cannot
  be satisfied jointly, naming every requirer and what each asked for; otherwise
  take the current version satisfying all of them. In the ordinary case every
  constraint is a major line and this is one lookup.
- Default a dependency to the major line. A bundle constraining more narrowly
  carries a stated reason, and publication rejects it without one. Projects owe no
  reason for their own pins.
- Run that check at publication and again at installation, with the same code and
  different inputs.
- Report the transitive set and its unconditional context cost before writing
  anything.
- Record in the manifest, per entry: version, and asked-for versus required-by.
- `lifecycle_status` already carries readiness. Experimental is not a version
  question and must not become one.

## Alternatives considered

**Forbidding constraints narrower than a major.** It would make the deadlock
between two tightly-pinned bundles structurally impossible, at the cost of the
compliance case — an organization that must not accept an unreviewed policy change
would have no way to say so. The deadlock it prevents is caught at publication
anyway, where somebody can act on it. *Revisit if tight constraints in published
bundles become common enough that publication failures block ordinary work.*

**A second kind of dependency** distinguishing *I need its content* from *I accept
its rules*. It dissolves on inspection: obligation is declared by a catalog's
mandate, not by a dependency, and what loads is decided by preload. A dependency
that also bound you would be an obligation wearing a different name. *Revisit if a
real bundle pair needs a distinction these two mechanisms cannot express.*

**A depending bundle raising a specific document of its dependency into context.**
It reaches into another bundle's internals by path, which breaks silently on
rename. When it bites, the honest diagnosis is usually that the dependency's own
preload is wrong — a fix that helps every adopter rather than one. *Revisit when a
real pair demonstrates otherwise.*

## Open, and unresolved here

- **Whether the catalog now needs a version of its own.** *Bundles are versioned;
  the catalog is not* says a catalog version starts meaning something once bundles
  gain dependencies, because entries would then need co-guarantees. This proposal
  gives them exactly that. The old argument no longer defends the "no" side, and
  nothing yet argues the "yes" side.
- **Cycles.** A depends on B depends on A. Harmless for content, pathological for
  anyone reasoning about it, and presumably detected and rejected. Unspecified
  because none can exist yet.
- **Cross-catalog conflict.** Curation makes a catalog internally consistent, but a
  project adopting from two catalogs can still reach a combination neither
  publisher saw. No escape hatch is proposed. If it turns out to be common, the
  answer is arbitration between catalogs, which does not exist.
- **Budget enforcement.** Reporting the context cost is proposed; refusing an
  adoption that exceeds a ceiling is not. That becomes worth having only once
  bundles routinely carry large unconditional content.

## Where the reasoning behind this lives

Much of the argument that produced this was worked out in conversation and
survives only in commit messages on `luma-leader`, `luma-catalog`,
`luma-knowledge-format` and `luma-foreman` dated 2026-08-21 and 2026-08-22 — in
particular why minimal-version selection is wrong for policy, why semantic
versioning carries little signal for prose, and the route from *do we need
versions at all* to *versions are good enough*. **That is a known weakness of this
document**, recorded so it can be fixed rather than rediscovered.
