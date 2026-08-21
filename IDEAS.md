# Ideas

Wanted capabilities, captured so they survive. Nothing here is designed, ordered, or committed to — this is a holding pen. Expect it to be replaced by `.luma/backlog/` once the repository is ready for it.

## Migrated 2026-08-21

**This file has been migrated to individual idea files** and is kept only until its deletion is confirmed. Fourteen entries: eleven became twelve files, three were pruned. Every entry below carries a marker saying which.

### Migrated

| # | Title | Landed | Modifications | Metadata |
|---|---|---|---|---|
| 1 | Derive project goals rather than restate them | `luma-hq` · derive-project-goals | none | someday · project |
| 2 | Detect boundary collisions before they are built | `luma-hq` · detect-boundary-collisions | none | someday · project |
| 3 | Decide what to build next, and say why | `luma-hq` · decide-what-to-build-next | none | someday · project |
| 4 | Hold organization-level learnings | `luma-hq` · hold-organization-level-learnings | none | someday · project |
| 7 | Open: how anything here reaches an agent working elsewhere | `luma-foreman` · knowledge-reaching-agents-elsewhere | retitled · note added | someday · project |
| 8 | Language selection | `luma-hq` · technology-stack-selection | retitled · radar context added | someday · project |
| 9 | Branding | internal hq · branding-workflow | retitled | someday · **organization** |
| 11 | Organization Personas | `luma-hq` · persona-templates | retitled · clarification added | someday · project |
| 12 | Repo maturity standard | `luma-foreman` · declared-maturity-and-behaviour | retitled · 3 notes added | someday · project |
| 13 | Draft: departments, and how not to foreclose them | `luma-hq` · departments | retitled | someday · project |
| 14 | Survey to track organization division | `luma-hq` · organization-division-survey | **split 1 of 2** | someday · project |
| 14 | *(same entry)* | `luma-hq` · tools-that-run-on-a-schedule | **split 2 of 2** | someday · project |
| — | A hook that stops the internal hq leaking | `luma-foreman` · hook-against-leaking-internal-hq | **new capture** | **next** · project |

### Pruned

| # | Title | Why |
|---|---|---|
| 5 | Competitive analysis | A topic, not an idea — names a subject without saying what would be built. Its justification was also gone: no longer this repository's responsibility, and the no-naming exemption it referenced had moved to the organization's internal headquarters |
| 6 | One directory for everything a project knows about itself | Already happened. Settled in `DECISIONS.md`; a design record rather than a wanted capability |
| 10 | Strategry selection | Already happened. Every line built or settled — what it calls a *strategy* is now a *bundle* |

`luma-hq` 9 · `luma-foreman` 3 · internal hq 1 · pruned 3

**One thing is unhomed and must be settled before this file is deleted.** The *still standing / overturned* summary inside entry 6 exists nowhere else. It belongs in `DECISIONS.md` beside the decision it annotates, not in an idea file.

## Derive project goals rather than restate them

> *Migrated to `.luma/backlog/ideas/derive-project-goals.md`.*

Each project records its own goals in its `.luma/backlog/`. This repository should read them rather than keep a second copy that drifts.

## Detect boundary collisions before they are built

> *Migrated to `.luma/backlog/ideas/detect-boundary-collisions.md`.*

Two projects growing toward the same capability is the failure this repository exists to catch early. What the signal looks like is unknown.

## Decide what to build next, and say why

> *Migrated to `.luma/backlog/ideas/decide-what-to-build-next.md`.*

Which repository the organization needs, in what order, and the reasoning — recorded well enough that the answer can be argued with later.

## Hold organization-level learnings

> *Migrated to `.luma/backlog/ideas/hold-organization-level-learnings.md`.*

What has been discovered once and should not be rediscovered. `DECISIONS.md` is the current form; whether learnings and decisions are the same shape is unsettled.

## Competitive analysis

> *Dropped 2026-08-20 — not migrated.* A topic heading rather than an idea: it names a subject without saying what would be built, what form the analysis takes, or what problem it solves. Its justification is also gone — competitive analysis is no longer this repository's responsibility, and the no-naming exemption this entry referenced now lives in the organization's own headquarters, resting on internal audience rather than on private visibility. If a real want appears later, it will be a better idea than this one.

What else exists and where it is strong. Exempt from the no-naming rule while this repository stays private.

## One directory for everything a project knows about itself

> *Dropped 2026-08-20 — not migrated.* Already happened. This is a settled design conversation rather than a wanted capability, and an idea is *something worth doing that nobody is doing yet*. The decision, its reasoning, the deferred alternatives and the re-open triggers are all in `DECISIONS.md`. Filing a hundred lines of superseded reasoning as an idea would launder it into the list people are told holds live wants.
>
> **Not yet carried anywhere: the "still standing / overturned" summary in the banner below.** It is a good short account of which parts of the reasoning survived, and it exists only here. It would belong in `DECISIONS.md` beside the decision it annotates, never in an idea file. **Decide that before step 9 deletes this file** — until then nothing is lost.

> **Settled 2026-08-18 — see [The project store is `.luma/`](DECISIONS.md#the-project-store-is-luma).** The store is `.luma/{backlog,policy,records}`: one root rather than two supported layouts, vendor-named rather than generic, and `policy` rather than `standards`.
>
> The reasoning below is kept because most of it survived, and the parts that did not are instructive. **Still standing:** the unifying property is the reader; tiers are cut by lifecycle rather than by topic; `records` beat `history`; generated material is a projection and never the source. **Overturned:** the flat-versus-nested choice, which existed to lower adoption cost and dissolves when the tiers are created together; the naming survey, answered by owning a name rather than finding an unattractive one; and `standards`, which was needed at the organization level and means *baseline default* as readily as *norm we uphold*.

Every project should have an obvious home for what it intends, what it requires, and what has happened to it. Today `.backlog/` holds intent and nothing holds the rest — decisions and their reasoning, logs, audits, guardrails, workflows, generalized skills, conventions, glossary, harness configuration. That material is currently scattered, homeless, or trapped in vendor directories.

### What it is

The unifying property is the reader. Nothing in it is read by a compiler, a bundler, or the runtime — it is read by an agent trying to act correctly in this project. Humans consume it too, but generally *through* an agent rather than by browsing. That ordering is the test for what belongs, and it means the contents are shaped for retrieval and parsing rather than for reading start to finish.

It is the durable half of the answer to the question parked below. A committed directory is inert, but it is portable, survives the machine, and travels with a clone. It does not solve liveness; it makes agent memory something a repository carries rather than something one laptop holds.

### Shape

Three tiers, split by lifecycle rather than by topic:

- **backlog** — what we intend. Churns; items are created and destroyed.
- **policy** — what is in force. Live, constantly edited.
- **records** — what happened, and why. Append-only, dated, never edited.

Both layouts are supported, because the preference is genuinely split — some people want three self-describing directories at the root, others want one and will resent three. Flat is the default, and nesting is opt-in; when nested, `.hq/` is the default root.

The default is held loosely. It rests on flat being cheaper to adopt and on each tier name reading correctly on its own, not on a settled view that three root entries are better than one.

```
.backlog/         .hq/
.standards/         backlog/
.records/           standards/
                    records/
 flat (siblings)   nested (default root .hq/)
```

**Flat** costs nothing to adopt: `.backlog/` never moves, and each tier can be added independently. **Nested** gives the convention somewhere to describe itself, keeps one namespace to defend rather than three, and holds one line of root clutter instead of three.

### Locations: two shapes, and only two, for the first version

Exactly two layouts are recognized. Nothing else is supported and nothing is configurable.

A reader determines which is in use by looking: if `.hq/` is present, nested; otherwise flat. That is the entire resolution mechanism, and the simplicity is the point — with two known shapes there is no manifest, no binding syntax, and no constant that has to be findable before anything else can be found. Fully configurable paths would have made that bootstrap the hardest problem in the design; fixing the shapes deletes it.

The nested layout doubles as the brownfield escape hatch. A project that already owns `standards/` or `records/` for something unrelated can nest instead, and `.hq/standards/` collides with nothing.

**Sharp edge, unresolved:** a project whose `.backlog/` is owned by another tool cannot nest cleanly. It would end up with that tool's `.backlog/` and a separate `.hq/backlog/` meaning the same thing in two places.

**Deferred: fully configurable paths.** The tiers would become *roles* and directories mere *bindings* to them, so adoption never requires moving a file — the right design for brownfield adoption at scale. It costs a resolution mechanism plus the one bootstrap constant that must be findable without configuration, and it risks dissolving a convention into configuration, since a path nobody can rely on is not much of a convention. *Re-open if real brownfield projects cannot adopt under the two fixed shapes.* If the cost stays high, two shapes is the permanent answer, not a stepping stone toward configurability.

Resolution belongs to foreman either way, and stays local: nothing reads this repository at runtime, so the boundary holds.

### What goes in it

**Records** — decisions with reasoning, deferred alternatives, and re-open triggers; audits (foreman's dated run output); logs of what was done, when, and by which agent or human; learnings discovered once and never to be rediscovered; incidents and postmortems; migrations and the rationale at the time; provenance of vendored code and licensing origins; measurements and benchmark baselines, dated so drift is visible.

**What is in force** — guardrails and invariants; workflows and repeatable procedures; generalized vendor-neutral skills; conventions of style, naming, and terminology; project boundaries derived from this repository but mirrored locally so they read offline; standards conformance and granted exemptions with reasons; architecture as current shape and invariants, not history; orientation, the "start here" an arriving agent reads first; verification policy and the definition of done; escalation rules for when an agent must stop and ask a human; glossary; data handling and secrets policy; harness configuration in vendor-neutral form.

**Explicitly out** — source, build output, published documentation for consumers, secrets, dependencies.

Foreman writes audits here, which makes the layout a boundary surface rather than a folder — settled in this repository, honored in foreman, without foreman ever reading this repository at runtime.

### Generated projections

Anything a specific tool expects lives wherever that tool looks, is generated from the canonical store, and is disposable: `.agents/`, `AGENTS.md`, `CLAUDE.md`, and whatever replaces them. Nothing generated is ever the source.

This is also the defense against namespace reservation. `AGENTS.md` is already becoming a de facto specification and `.agents/` is the obvious next claim; once a specification owns a path, it defines what files there mean and validators reject the rest. As a generated projection, conformance becomes a build target and the records never move.

### Naming

Only needed for the nested layout, where the default is `.hq/`: a headquarters holds plans, standing orders, and records without stretching, and the metaphor is fractal, so the organization has one and each project has one at a different scope. Considered and beaten — `.hub/`, `.core/`, `.work/`. Each leaks slightly: a hub is a pass-through when this is a store, `core` reads as essential source in a codebase, and `work` reads as scratch or staging.

Criteria that emerged, worth keeping independently of the outcome — these may be decision-shaped rather than idea-shaped:

- **Guessable cold.** A stranger who has never been told must form roughly the right expectation on sight. This is what set aside `.practice/` (reads as rehearsal or soccer practice) and `.canon/` (reads as derivative fan writing).
- **Unclaimable.** Collision risk tracks the industry's live vocabulary. A name nobody would want to standardize on cannot be reserved out from under you. This is what set aside `.agents/`, `.context/`, and `.memory/`.
- **Survives the vocabulary cycle.** The records must outlive the words currently fashionable for describing them. The pressure recurses inward: *skills* and *guardrails* are the same vintage as *agents*.
- **Names something that keeps acting**, per the existing naming decision.
- **Does not feel like filing.** `.office/` was set aside on affect alone — a name that makes the work feel administrative is a name that discourages the work.
- **Collisions only matter at the root.** Inside the directory the namespace is ours, so subdirectory names should be picked on fit and legibility alone.

Set aside, with reasons that would have to change to revive them: `docs/` (outward-facing by convention and contested by documentation tooling); `.meta/` (guessable and honest, but names a category rather than a job — re-open if no candidate beats it on that single defect); `.build/` (one dot from build output, which is generated and ignored by default — the opposite of durable committed context); `.workspace/` (claimed with real semantics by several toolchains); `.view/`, `.dashboard/`, `.panel/` (name a read-only display when this is a store); `.overseer/`, `.creator/`, `.maker/` (name a person, the one thing not in the directory).

### The tier names

**`standards/`** for what is in force. Chosen on legibility: guessable cold by anyone, and already this organization's own word for this exact material. Two things to weigh, neither disqualifying — `luma-hq` uses *standards* for organization-level rules, so a project-local `standards/` may blur inherited from local, which is arguably correct since a project's standards genuinely are the inherited ones plus its own; and like every prescriptive name it strains slightly on glossary, architecture, and orientation, which are reference rather than instruction.

**`records/`** for what happened, kept over `history/`. The switch would have been for legibility, but *records* is exactly as plain, so there is nothing to buy the costs with. History is a narrative of what is over; records are documents kept because they remain useful, and a decision with a re-open trigger is a live instrument that happens to be dated. The verb is also already in this organization's prose — a path not taken is *recorded*, not historicized. Practical kicker: the VS Code Local History extension writes into `.history/` at workspace root, which is a live collision under the flat layout.

Set aside, with the reasons that would have to change to revive them:

- **`history/`** — the closest contender of any name considered, and the last one standing against `records/`. Equally guessable, equally plain, and it covers every item in the tier. It lost on two counts: the connotation that history is what is over, when these documents stay in force; and the `.history/` collision with the VS Code Local History extension. Supporting both layouts makes the second count decisive — a name must be safe flat as well as nested, and `history/` is not. *Re-open only if the flat layout is dropped.*
- **`bedrock/`** — what the project is built on and does not move; holds the tier's descriptive contents as naturally as its prescriptive ones, and is unpretentious in the register of `foreman`. Lost only to legibility. *Re-open if `standards/` proves to blur badly against the organization-level meaning.*
- **`orders/`** — the most coherent system, but only under `.hq/`. Standing orders are precisely the permanent instructions in force until changed, and a headquarters issuing orders and keeping records holds together end to end. Its coherence is its defect: orphaned under any other root, it couples the tier name to a root decision that is not final. *Re-open if the root settles as `.hq/`.*
- **`evergreen/`** — covers the contents honestly, but is a modifier rather than a thing, and content marketing owns the phrase.
- **`always/`**, **`givens/`** — the cleanest semantics of any candidates (*not yet true / always true / was true*), set aside because neither is guessable cold. *Re-open if guessability stops being the governing criterion.*

The general rule worth keeping: **do not couple two open questions.** A name that stands alone is worth more than a name that is better only if a separate decision goes a particular way. It is the reason `orders/` lost despite being the best fit on the merits.

### Durability constraints

Records outliving the tool means readable without the tool: plain text, no harness-specific schema, no frontmatter only one vendor parses. Anything requiring a single tool to interpret is a projection, not a record.

### Open questions

- Whether generated material (indexes, caches, rolled-up digests) gets a fourth ignored tier or lives with its reader.
- Whether **logs** and **audits** are one thing wearing two names.
- What a project does when another tool hardcodes `.backlog/` and it wants the nested layout. Costs nothing under the flat default; has no clean answer under nesting.
- Whether flat should be the default at all. Held loosely on adoption cost, not conviction.
- Whether offering `.hq/` as the nested default extends the initials carve-out far enough to need recording as its own decision.

## Open: how anything here reaches an agent working elsewhere

> *Migrated to `luma-foreman/.luma/backlog/ideas/knowledge-reaching-agents-elsewhere.md`.* Moved out of this repository: the question is how knowledge reaches an agent working elsewhere, and elsewhere is where hq structurally is not.

A committed file is durable but inert. Machine-local agent memory was live but fragile and keyed to one directory on one machine. Neither is sufficient alone, and this trade is unresolved — it is the same question parked in `luma-foreman/examples/`.

## Language selection

> *Migrated to `.luma/backlog/ideas/technology-stack-selection.md`.*

I want a workflow for selecting technology stack for a new project in my organization.

## Branding

> *Migrated to the organization's internal headquarters, at `.luma/backlog/ideas/branding-workflow.md`.* Moved out of this repository: it is for branding one organization's own projects, not a reusable capability.

I want a workflow for helping me brand projects.

## Strategry selection

> *Dropped 2026-08-21 — not migrated.* Already happened. Every part of this is built or settled: the three buckets are the universal, organization and project catalogs; *copy or symlink* was settled as copy-then-adopt, with adoption by reference deferred because it breaks self-containment; foreman is the applier; the two-way street is the promotion chain, project → organization → universal, with no level skipped; version pinning stops a project's strategy changing underneath it; `obligation` with most-restrictive-wins is the forced-down case; and the *nice to have* adoption window became the `by:` date. See `DECISIONS.md`.
>
> **What this entry calls a *strategy* is now called a *bundle*.** Noted because the entry reads as unbuilt to anyone searching for the old word.
>
> **One line not carried:** *"your own version of luma-hq will help you establish them"* — an hq actively helping develop strategies rather than only holding them. That overlaps *Decide what to build next* and is not built. Dropped with the rest by decision rather than by oversight.

I want to have a catalog of strategies to apply to each project.  And for each project I can choose from the catalog (either via copy or maybe symlink, this mechanism needs to get fleshed out) to apply them to my given project by using a command found in luma-foreman.   The `luma-hq` project will help us define the strageties we can select from for our org.   `luma-hq` will use it's breadth of organization knowledge to develop the strategies and then it will promote them to luma-foreman where they become available for selection.

 But strategies do not HAVE to come from luma-hq, they can start bluegrass style withing a given project - where a strategy is either unique to a project, or it is trialed in a project and then eventually gets circulated up through luma-hq where it can make it's way to other projects if it's useful enough.  It's a two way street.  Althought luma-hq will have the context of all projects and foreman will have the context of a single project.  So most strategies will typically get handed down from luma-hq.

There will be three buckets of stategies:
- Universal stategies, provided by luma organization for all other organizations to select from (not just for the luma organization)
- Organization strategies, provided by your organization - specific to your organization, these will go under your own git repo and your own version of luma-hq will help you establish them
- Project strategies, provided by a given project and not shared enough to get promoted to organization or universal strategies

Then each project will somehow select from these buckets what strategies get applied.  And we need to select in a way where as strategies change the project doesn't get it's strategy changed from underneath it.  For example if a universal strategy XYZ goes from v1.0.0 to v2.0.0 then the project should not automatically adopt v2.0.0 but it should be made aware that new stratgies are available.

And either this project luma-foreman and/or luma-hq should be able to force new strategies down for critical stuff where projects are not allowed to adopt on their own schedule.  It's mandated and immediate.  

A nice to have is when new strategies are published they should also publish some kind of date that let's projects know what kind of timeline they have to reasonabily adopt them before they "fall out of compliance".

## Organization Personas

> *Migrated to `.luma/backlog/ideas/persona-templates.md`.*

Include templates for organization personas.  Consider also supporting user personas as this level, or maybe at the foreman level, I'm not sure yet.

## Repo maturity standard

> *Migrated to `luma-foreman/.luma/backlog/ideas/declared-maturity-and-behaviour.md`.* Moved out of this repository: the unbuilt part is behaviour keyed to maturity, which belongs where the reading and asserting happens.

A repo or LKF document can declare it's maturity.  Repos can declare in README.md, CLAUDE.md, or maybe one or two other places.  We need to provide a list of maturities.  Maybe we already have them and can use what's already provided by LKF.

And then a when a repo has a declared maturity we can behave in different ways.  When something is brand new we should stop saying things like, this is already established so we have to follow this or that rule.  And when it's very stable and established we have to treat most things like they are law.  

We should prefer maturity at the document level, but when repos are brand new we should accept it at the repo level as well so it's flexible.
## Draft: departments, and how not to foreclose them

> *Migrated to `.luma/backlog/ideas/departments.md`.*

**Draft — not settled, not reviewed.** Recorded so the options survive, not to pick one.

Some organizations have departments, and nothing here decides what that means. An earlier pass assumed a department runs its own hq and catalog, which was already too prescriptive — that is one shape among several, and the least tested.

**Shapes worth keeping on the table:**

- **A department is an organization.** Its own hq and catalog, with `upstream` chaining universal ← company ← department. Needs nothing new on the catalog side. The gap would be the hq, which has no upstream and no promotion path, so company-wide knowledge would be duplicated and drift.
- **A department is a tag on content.** One hq, one catalog, and department becomes a dimension of the material inside them. Tags already exist as a mechanism, projects already declare them, and this adds no repositories at all. Probably the cheaper shape, and it is not obvious what it fails at.
- **Something else entirely.** Neither has met a real organization.

**What is already true — as fact, not as an answer.** A catalog's `upstream` is single-valued and acyclic, so a three-link chain works today with no change. Obligations resolve most-restrictive-wins, which is associative and therefore composes over any chain length. Neither was built for departments; both happen to accommodate one shape of them.

**Constraints that keep every shape open**, worth holding regardless of which wins:

- `upstream` stays single-valued and acyclic.
- Obligations stay most-restrictive-wins.
- Origin stays derived from location rather than declared.

**Unchecked:** starter `extends` / `adds` / `excludes` was reasoned about across two catalogs. Over three links, a department-level `excludes` against a universally-defined starter may be order-dependent, and order-dependence was only ever ruled out for the two-catalog case. Worth verifying before a third link exists under any shape.

**Why not now:** nothing has adopted anything. One real organization with real departments will settle this faster than further reasoning will.

## Survey to track organization division (and scheduled tooling)

> *Migrated to two files — split at migration.* The survey is `.luma/backlog/ideas/organization-division-survey.md`; the scheduled-execution half is `.luma/backlog/ideas/tools-that-run-on-a-schedule.md`. Each is independently buildable, and each cross-references the other.

Ask how an organization is divided.
Capture the answers, so agents have enough context to help make organizational decisions.
We need to strike a balance between being useful/high impact and not something that will easily rot.
We need a ceremony once per quarter that updates this, or better yet the foreman or hq sweeper should run every day/week and look up what happens on a schedule and make sure we're doing it.   
We should capture the expected cadence, the actual cadance, and how important the thing is so we can figure out how much rot is acceptable, and so we can figure out how to divide our focus in the most useful way.  This needs to be easy to tune as needs and wants change over time.  It should be fluid.
This basically introduces a new concept that some portion of our tools need to be able to run on a schedule, so they can rover, sweep, audit, and govern policy and also eliminate rot.
