---
type: document
title: Bundle identity
description: How a bundle is named and resolved — the catalog location is the canonical reference, why the alternatives were not taken, and how a short-name layer could be added later without breaking it.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Bundle identity

**A bundle's canonical reference is its catalog's location.**

```
github.com/LumaStack/luma-catalog/git-secrets
```

Everything in [bundle-adoption.md](bundle-adoption.md) and
[bundle-dependencies.md](bundle-dependencies.md) that names a bundle rests on
this, which is why it is decided here rather than assumed inside them.

## Why, in one paragraph

**The most reliable component is the one that does not exist.** Every alternative
requires something that maps a name to a location — and that something has to
exist, be present wherever a reference is read, and agree with every other copy of
itself. Here the reference *is* the location, so there is nothing to be absent,
stale, or in disagreement.

**References get read where there is no context** — a bundle sitting in a
machine-wide cache, a line quoted into an agent's window, a commit message, a log.
Those resolve under this scheme and cannot under any other.

**And when it fails, it fails loudly.** A moved catalog resolves to nothing and
says so. The alternatives fail by resolving to something, which is the worse
outcome in a system whose failure mode is an agent following the wrong rules.

## What it costs, stated plainly

**Relocation breaks references.** Rename an organization or a repository, move
host, and every reference to that catalog's bundles must change. This is a real,
recurring pain in Go, which uses the same scheme.

Three things make it survivable here and none of them make it free:

- the graph is flat and shallow — two to four catalogs, mostly your own manifests
- forges redirect renamed repositories, so a rename degrades before it breaks
- the change is greppable and mechanical

**Verbosity.** Accepted deliberately: reliability first, maintainability second,
brevity last.

## A short-name layer may be added later, and would cost nothing to defer

A project-local alias table — `luma` meaning
`https://github.com/LumaStack/luma-catalog` — remains attractive and is **not
ruled out.** It is deferred rather than rejected.

**Deferring it is free, and the order matters.** An alias is expressible in terms
of a location; a location is not recoverable from an alias. So adding aliases later
breaks nothing, because every existing reference is already canonical and aliases
become an additional way to *type* one. Starting with aliases and wanting locations
later would mean rewriting every reference and reconstructing what each alias meant
at the time.

**One rule makes the combination safe: aliases are input only.** A person may type
`luma/git-secrets`; the manifest records the canonical form. If an alias can be
*written into* a file, the self-describing property is gone from exactly the places
it was chosen for — and the alias has stopped being sugar and become a second
scheme.

## Option C is out

A namespace declared by the catalog was considered and rejected. It needs a
name-to-location map exactly as an alias does, so a reference is still not
self-describing — and it adds a collision risk that aliases do not have, because
its names are intended to be global while nothing enforces uniqueness. **It carries
the cost of a lookup layer without the safety of locality.**

## Two decisions that look like one

**What a bundle's canonical reference is** — the string written in a manifest, in
a bundle's dependency declaration, in an adopt command, and read by an agent that
arrived with no other context.

**What the machine-wide cache keys on.**

**These are separable.** By the time anything is cached it has already been
resolved, so the cache can key on the resolved catalog URL under any answer to the
first question. Deciding the reference format does not commit you on the cache,
and the reverse is also true.

## The comparison this came from

Kept because the reasoning is worth more than the conclusion, and because the
deferred option needs its costs on record.

Before this, `luma/git-secrets` was used informally and what `luma` *meant* had
never been decided — no catalog declares a name for itself, so it was inferred by
stripping `-catalog` from a repository name.

| | **A · project alias** | **B · catalog location** | **C · declared namespace** |
|---|---|---|---|
| looks like | `luma/git-secrets` | `github.com/LumaStack/luma-catalog/git-secrets` | `luma/git-secrets` |
| resolvable on its own? | **no** — needs the project's table | **yes** — the identity is the address | **no** — `luma` names nothing findable |
| can two parties collide? | no — aliases never leave the project | no — the host guarantees it | **yes** — honour system; two organizations may both pick `acme` |
| survives a rename or host move? | **yes** — change one line | **no** — every reference breaks | **yes** — the name is independent of location |
| several catalogs per organization? | yes — two aliases | yes — two URLs | yes, if each declares a distinct name |
| length in daily use | short | long | short |
| what has to exist for it to work | a catalog table in every project | nothing | a `namespace` field, and a way to resolve name to location |

### A — project alias

Each project declares what it draws from, and references use the local name:

```toml
[catalogs]
luma = "https://github.com/LumaStack/luma-catalog"
acme = "https://github.com/AcmeCorp/acme-catalog"
```

The same shape as a git remote: `origin` is a nickname for a URL and nobody
mistakes it for an identity.

**The cost is that a reference means nothing in isolation.** An agent reading a
bundle's dependency list, or a person reading a commit message, cannot resolve
`luma/git-secrets` without finding and reading a table somewhere else.

### B — catalog location

The reference is the address, as Go does with module paths.

**The cost is relocation.** Rename the organization, rename the repository, or
move host, and every reference to every bundle in that catalog breaks. This is a
known and recurring pain in Go — `replace` directives and vanity import paths both
exist to work around it.

### C — declared namespace

A catalog declares `namespace: luma`, and references use it.

**The cost is that uniqueness is unenforced.** Nothing prevents two organizations
choosing `acme`, and nothing resolves a name to a location — so a reference is
still not self-describing, and now there is a field that can be wrong.

## The axis underneath

**Self-describing versus relocatable.** A reference can answer *where do I find
this* on its own, or it can survive the catalog moving. No option gives both.

Every ecosystem that has faced this picked one and built an escape hatch for the
other: Go picked self-describing and added `replace`; Maven picked relocatable
coordinates and separated them from repository URLs; npm picked short names and
required a registry to make them unique.

## The default-catalog shorthand is out

`git-secrets` resolving against the universal catalog, with everything else written
in full — Docker's model, where `ubuntu` means `docker.io/library/ubuntu`.

**Rejected under the same priority that chose the scheme.** It makes the *most
common* reference the one that is not self-describing, and the most common
reference is the one most likely to be read somewhere with no context. It also
creates two forms of the same kind of thing, so a reader has to know which they are
looking at before they can act on it.

## The cache key

**The resolved location, which under this scheme is the reference itself:**

```
~/.cache/luma/bundles/github.com/LumaStack/luma-catalog/git-secrets/1.2.0/
```

Unique, browsable with `ls`, long, and never typed by a person. **No collision
guard is needed** — there is nothing to disambiguate, because two catalogs cannot
share a location.

*Content addressing — keying on the checksum the manifest already carries — was the
alternative. It never collides and deduplicates identical bundles published by
different catalogs, and it makes the cache unreadable. Rejected because these are
small prose bundles and legibility is worth more than deduplication.*

## What the files look like

**Intent and resolution do not share a file.** The layout policy already gives the
reason: *a hand-edited checksum makes the check silently start passing, and that
value must never sit in a file anyone is invited to edit.*

**Intent, written by a person:**

```toml
[[want]]
bundle  = "github.com/LumaStack/luma-catalog/git-secrets"
version = "2"                    # the major line — the norm

[[want]]
bundle  = "github.com/AcmeCorp/legal-catalog/retention"
version = "1.4.2"                # narrower, so it states why
reason  = "regulator requires review before adopting policy changes"
```

**Resolution, written by the tool and never by hand:**

```toml
[[bundle]]
id       = "github.com/LumaStack/luma-catalog/git-secrets"
version  = "2.3.1"
checksum = "sha256:9f2c…"
origin   = "vendored"
wanted   = true

[[bundle]]
id       = "github.com/LumaStack/luma-catalog/luma-layout"
version  = "1.1.0"
checksum = "sha256:41ab…"
origin   = "vendored"
wanted   = false
required_by = ["github.com/LumaStack/luma-catalog/git-secrets"]
```

**Two files, and no `source` field**, because `id` is the source. Under either
deferred alternative the resolution file would carry the location anyway, in a
separate field, plus a third file mapping names to it — which is what made the
short name a layer over this scheme rather than a replacement for it.

`wanted` and `required_by` exist for removal: drop a bundle and its dependencies go
with it, unless something else still requires them or they were asked for directly.

## What would reopen this

**Evidence that catalogs move.** An organization restructuring repositories
regularly, or a planned host migration, turns a rare loud breakage into a frequent
one — and that is the single fact this rests on. None exists yet: no catalog has
been moved, renamed or forked.

**A short-name layer being wanted before it is cheap.** Deferring costs nothing
only while references stay canonical. If aliases start being written into files, the
order stops being free.
