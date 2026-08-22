---
type: document
title: Bundle identity
description: How a bundle is named and resolved — the options for what a canonical reference looks like, laid out for a decision that has not been made.
lifecycle_status: draft
created: { by: human:benlinton, at: 2026-08-22T00:00:00Z }
modified: { by: agent:claude-opus-5, at: 2026-08-22T00:00:00Z }
---

# Bundle identity

**Undecided.** This document lays out options rather than arguing for one.
Everything in [bundle-adoption.md](bundle-adoption.md) and
[bundle-dependencies.md](bundle-dependencies.md) that names a bundle depends on
this answer, so it is written down separately rather than assumed inside them.

## Two decisions that look like one

**What a bundle's canonical reference is** — the string written in a manifest, in
a bundle's dependency declaration, in an adopt command, and read by an agent that
arrived with no other context.

**What the machine-wide cache keys on.**

**These are separable.** By the time anything is cached it has already been
resolved, so the cache can key on the resolved catalog URL under any answer to the
first question. Deciding the reference format does not commit you on the cache,
and the reverse is also true.

## The reference — three options

Today `luma/git-secrets` is used informally. What `luma` *is* has never been
decided: no catalog declares a name for itself, so it is inferred by stripping
`-catalog` from a repository name.

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

## A sub-option, relevant only under B

**Default-catalog shorthand.** `git-secrets` resolves against the universal
catalog; everything else is written in full. Docker's model, where `ubuntu` means
`docker.io/library/ubuntu`.

- **for** — the most common reference is short, and every non-default reference
  stays self-describing
- **against** — two forms of the same kind of thing, and a reader must know which
  one they are looking at

## The cache key — two options

**By resolved URL.** `~/.cache/luma/bundles/github.com/LumaStack/luma-catalog/git-secrets/1.2.0/`
Unique, browsable with `ls`, long, and never typed by a person.

**By content hash.** Never collides, deduplicates identical bundles published by
different catalogs, and tells you nothing when you look at it.

Under A or C the cache needs the URL regardless, to disambiguate names that are
not globally unique. Under B the URL is simply the reference again.

## What decides it

**How often will a catalog move or be renamed, against how often will a reference
be read outside the project that declared it?**

- If catalogs are stable and references travel — into an agent's context, a commit
  message, another organization's bundle — then B's cost is a rare migration and
  its benefit is continuous.
- If catalogs move while an organization finds its footing, B taxes every
  reference and A or C is cheap insurance.

**No evidence either way exists yet.** No catalog has been moved, renamed, or
forked, and nothing has been adopted.

## What is blocked until this is decided

- the cache path in [bundle-adoption.md](bundle-adoption.md), and whether it needs
  a collision guard at all
- whether a catalog needs a field naming itself
- what a bundle writes when it declares a dependency on a bundle in another
  catalog — the case where the adopting project's aliases are not available
