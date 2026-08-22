---
type: document
title: Bundle adoption
description: How a bundle gets into a project — whether its content lives in the repository or outside it, how a change to it gets reviewed, and where the two kinds of bundle sit.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Bundle adoption

**Draft. Nothing here is settled.** Companion to
[bundle-dependencies.md](bundle-dependencies.md) and
[bundle-versioning.md](bundle-versioning.md).

**The question:** when a project adopts a bundle, does the content land in the
repository, or does the repository record a version and resolve the content
somewhere else?

Two things are actually at stake, and they are worth separating because only one
of them is hard:

- **where the bytes live** — a storage question, and the less interesting one
- **how a change to a bundle gets reviewed** — a governance question, and the one
  that decides it

## The argument that decides it

**You cannot delegate this review to a version number.**

A lockfile is a design that says *the version is a sufficient summary of the
change*. That works for code, and it works for a specific reason: a library has an
interface. If the signature held, the caller still works, and a test suite checks
the rest. The version stands in for the diff because something else is verifying
the diff.

**Prose has no interface. Its content is its effect** — see
[bundle-versioning.md](bundle-versioning.md), where the same property forces every
version tier to be defined in outcomes rather than signatures. A bundle bumping
from `1.2` to `1.4` may have added a rule that changes what every agent in the
project does, and there is no signature to check and no test to run.

So the only honest review of a policy change is **reading the change**. And a
review of text requires the text to be present at the moment of review.

That is the argument. Everything below is either a consequence of it or a cost it
imposes.

### Two arguments that are weaker than they look

**"It works without a network."** Ruby needs the network for `bundle install`,
Node needs it for `npm ci`, and nobody treats that as a crisis — caches stay warm,
continuous integration caches them, containers bake them in. The residual point is
small and real: an agent session has no install step, so content that is not
already present has to be fetched mid-session or made a setup prerequisite. Both
are ordinary. **This is a minor benefit, not a reason.**

**"It proves the content is pristine."** A checksum proves that, and a checksum is
required under either design. Committing the content adds a second, weaker signal
— a git diff, which anybody can approve without reading. **The hash is what
proves; the diff is what informs.**

## But a diff is only oversight while somebody reads it

**This is the real cost, and it is the one that kills vendoring in practice.**

Adopt a dozen bundles and every update is a two-hundred-file diff. It gets
approved in four seconds. That is **worse than having no diff at all**, because it
looks like review happened and it did not — and once people are numb to a class of
change, they are numb to the one that mattered.

So the benefit and the cost are the same mechanism, and the design has to earn the
benefit rather than assume it.

**Keeping the diff readable is a requirement, not a hope.**

- **Content diffs collapse by default.** Mark the vendored tree as generated — a
  `.gitattributes` entry is enough on most forges — so a reviewer sees a
  proportionate change rather than a wall.
- **The manifest line is the reviewable unit.** `luma/git-secrets 1.2 → 1.4`, next
  to the bundle's own changelog, is what somebody reads.
- **The text stays one click away.** Present when it matters, out of the way when
  it does not.

The result is one meaningful line with the evidence attached, rather than a
hundred files with the meaning buried.

## Where the two kinds of bundle sit

**A project holds two kinds of bundle and they need different treatment:**

| | how it arrived | may it be edited? |
|---|---|---|
| **vendored** | copied from a catalog | **no** — replaced wholesale; an edit is drift |
| **authored** | written here | **yes** — it is the project's own content |

**They should be visibly separate.** Today `.luma/bundles/` is described as holding
*"adopted, or written here"* with no division shown, which leaves the only
separation as a publisher namespace — making somebody else's read-only content a
sibling of your own editable content, one path segment apart.

```
.luma/bundles/
  adopted.toml
  vendored/
    luma/git-secrets/
  authored/
    deploy/
```

`vendored` and `authored` name **how the content arrived**, which is the property
that decides whether you may touch it. It also avoids overloading *adopted*, since
an authored bundle still gets a manifest entry.

**The split belongs inside `bundles/` rather than beside it.** Both kinds are *in
force*, which is the same lifecycle, and the `.luma/` tiers are cut by lifecycle
on purpose. A separate top-level tier for ownership would mean two questions
deciding one location.

### Layout reduces accidents; detection is the guarantee

**Do not rely on the directory split.** It makes the mistake unlikely and obvious,
and a careless path still lands anywhere.

**What enforces it is drift detection** — comparing the checksum against the
vendored content, and reporting an edited copy as a finding. With the escape hatch
that makes it humane: an edited vendored bundle is either reverted, or **moved into
`authored/` and owned**. Somebody who needed to change it gets a legitimate route
rather than a rule to break.

## The fetch cache is a third place, and not a third mode

**Two different things sound alike and only one is contentious.**

A machine-wide store so ten repositories do not re-download the same bundle is
uncontroversial, orthogonal to everything above, and should exist. Replacing
committed in-force content with a resolved-at-use-time copy is the question the
next section answers. **This section is about the first.**

The cache lives outside every repository — `~/.cache/luma/bundles/` — which
resolves the worry that an agent browsing a project might read content that is not
in force. There is nothing to find, because it is not in the tree. No permission
rule needed.

### It holds many versions, and that is correct

**The one-version rule is per project, not per machine.** A project holds one
version of a bundle because its content is that project's instructions and two
would contradict each other. A cache holds no instructions — it is a download
store — so several versions of the same bundle coexisting there is right rather
than merely tolerated. Different projects legitimately want different versions.

```
~/.cache/luma/bundles/<catalog>/<bundle>/<version>/
~/.cache/luma/bundles/<catalog>/git-secrets/1.2.0/
~/.cache/luma/bundles/<catalog>/git-secrets/1.2.4/
```

**What goes in `<catalog>` is not settled** — see the next section. What holds
regardless is that the version is its own path segment, so two versions coexist
without either knowing about the other.

**No `v` prefix.** The manifest holds `1.2.0`, and the path should be
constructible from it without a reader remembering to prepend a letter.

**Nothing needs a separate level for the organization.** Two catalogs belonging to
the same organization are two catalogs — there is no case where the organization
disambiguates something the catalog does not. *An earlier draft asserted an
organization has exactly one catalog. That is not true and should not be assumed:
different disclosure levels and different governance are both ordinary reasons to
run several.*

### The path depends on an undecided question

The example above uses `<namespace>` as if that were settled. **It is not** — what
a bundle's canonical reference looks like is an open question with real options,
laid out in [bundle-identity.md](bundle-identity.md).

**The cache path is downstream of that decision.** If a reference is the catalog's
location, the cache key is that location and nothing more is needed. If a reference
is a short name — declared by a catalog, or aliased by a project — then names are
not globally unique, one machine can see two catalogs claiming `acme`, and the
cache needs the resolved URL alongside each entry so a name match with a source
mismatch is treated as a miss rather than a hit.

**What holds either way** is that the cache stores several versions side by side,
lives outside every repository, and is keyed on something that cannot collide. What
that something is written as is decided in the other document.

### Pruning is mark-and-sweep, and it is safe because a wrong sweep costs a download

The root set already has a home: `~/.config/luma/luma-foreman/projects/`, keyed by
repository root. **Consult every project on the machine, mark what each is using,
remove what nothing marked.**

**The property that makes this simple is that eviction is always recoverable.**
Every failure mode is benign:

| what goes wrong | consequence |
|---|---|
| a project foreman never registered is not consulted | its bundles are swept, and re-fetched next time |
| a stale entry for a deleted project still marks | over-marks, under-prunes, harmless |
| a project root is unreachable — unmounted drive | skip and warn, possibly over-prune |

Compare a runtime's garbage collector, where a wrong sweep is a crash. **Here the
worst case is latency**, and a mistaken sweep cannot break a project even in
principle, because the in-force content is committed in the repository. The cache
only ever accelerates a fetch.

Three properties worth having:

- **Explicit, never automatic.** A background process deleting things is
  surprising, and nothing about small prose bundles creates the pressure that would
  justify it. A command, with a dry run.
- **It reports what it consulted, not only what it removed.** *"Consulted 12
  projects, 3 unreachable; 47 cached, 9 unreferenced."* The unreachable count is
  the important one — it says the sweep was partial, which is the only thing that
  should make anyone distrust the result.
- **A lock or a grace period**, so a project adopting mid-sweep does not have its
  fresh download removed between fetch and use.

### The test that keeps it a cache

**Deleting `~/.cache/luma/` must lose nothing but time.** The layout policy already
states the general form — *if deleting it loses a decision somebody made, it is not
local state* — and it is the line that would tell you if this third location ever
started quietly becoming the first.

## One mode, not two

**A second mode is tempting and I would not build it yet.** This is about where
*in-force* content lives — the fetch cache above is not in question.

Content that is *sometimes* present is a different thing to reason about than
content that is always present. Every consumer downstream — projection, checks, an
agent reading the tree — would have to handle both, and the failure surface of a
property that is meant to be invariant doubles.

**The asymmetries all point the same way.** Vendored to cached is a delete. Cached
to vendored is a migration across every repository that adopted. And the failure
modes are not comparable: vendoring fails by being annoying, a cache fails by
being empty at the moment something needed it.

**What would justify the second mode is measurement, not preference** — a real
repository where the vendored tree dwarfs the source and the noise is unfixable by
the means above. Until something measures that, the mode is a failure surface
bought against a cost nobody has hit.

## The costs, stated plainly

Choosing this way is not free, and pretending otherwise makes the choice harder to
revisit honestly.

- **Repository size grows** with every adoption, and never shrinks on its own.
- **Churn is permanent.** The mitigations above make it proportionate; they do not
  remove it.
- **The same content is duplicated across every project that adopts it**, and
  updating a policy everywhere is one change per repository rather than one
  change. That is the standard pinned-dependency tax and the standard answer —
  automated update pull requests — applies.
- **Vendored content is editable**, which a cache would discourage simply by being
  somewhere people do not go. Drift detection is the answer, and it is a detection
  rather than a prevention.

## Open

- **What marks the vendored tree as generated**, per forge, and whether that is
  written by adoption or left to the project.
- **Whether `authored/` is the right word.** It pairs with `vendored/` and reads
  well, and `own/`, `local/` and `project/` were the alternatives considered.
- **Whether an authored bundle and a vendored one share a manifest** or the
  manifest only records what came from elsewhere.
- **What updating across many repositories looks like** in practice. Named as a
  cost above and not designed here.
- **What a namespace collision should do once detected.** The cache treats it as a
  miss, which is correct and leaves both projects re-fetching over each other. The
  fix is upstream — unique namespaces, enforced somewhere — and nothing enforces
  them today.
- **Whether an adopted-but-inactive bundle can exist.** If it can, inert content
  sits in the tree and an agent can read something not in force, which is the
  problem the cache's location was chosen to avoid. The position here is that
  `bundles/` means *in force* and unwanted content is removed rather than
  disabled — but that is an assertion, not a tested one.
