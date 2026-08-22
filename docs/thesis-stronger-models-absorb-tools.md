# Thesis: what remains as models get stronger

**Models will keep getting stronger, so these tools should expect to do less.**

That is the bet, and it is deliberate rather than defensive. A tool that has to do
a great deal is compensating for a model that cannot yet do it. Every capability
that becomes native is a capability worth deleting — and a version of luma that
does less than the last one, because the model grew into it, is a success.

## Where our tools should remain the same

As models get stronger and absorb tooling, this is what our tools should 
continue to offer.

**Governance.** A thing cannot bind itself. Rules an agent can rewrite are
suggestions, and an agent asked to follow rules it holds in its own context is
being trusted rather than governed. What binds has to sit outside the thing being
bound, and stay there.

**Observability.** The actor is the worst available witness. A record an agent
writes about its own behaviour is a claim, not evidence, and it is least reliable
in exactly the cases anybody would want it — when something went wrong, when a
step was skipped, when a check did not run. Observability has to be produced by
something that is not the subject.

**Portability.** A vendor cannot be neutral about itself, and will not be. What an
organization learns has to outlive the model it was written with, the harness it
was written in, and the company that shipped both. **The durable thing has to
survive its tooling.**

**Data privacy.** A boundary enforced by the thing it constrains is not a
boundary. What leaves an organization, what reaches a vendor, what lands in a
public repository — none of that can be left to the judgement of the system doing
the reaching. It has to be structural, checkable, and refuse by default.

**Data custody.** What you do not hold, you cannot move. Export is where a
vendor's interests and yours part hardest, and the friction there is built rather
than accidental.

**Knowledge injection.** A model knows the world. It does not know *this*
organization — what was decided, what was tried, what was ruled out and why. That
gap does not close with scale, because the missing material is private by
construction and was never in the training data. **This is the one that grows.** A
stronger model with an organization's context outperforms a stronger model
without it by more, not less, as both get stronger.

**Ease of use.** As models take over the doing, a person's remaining job is
deciding. And a model can produce more in an hour than a person can read in a day.
Our tools should help us understand what matters, so we are empowered to
make better, faster, and more informed decisions.

## Where our tools could shrink over time

**Bundle management.** There is no native standard for packaging context and
moving it between projects — no registry, no manifest, no versioning, no way for a
project to say *I take that one, at that version*. So these tools do it, and a
large part of what exists here exists only because nothing else does. Packaging
and distribution gets solved once and then belongs to everybody.  If a better
solution comes along then this tool should adopt the established best practice.

**Workflow chaining.** Nothing lets one workflow call another — say that this
runs before that, that a step brings in a policy, that a tool has to be present or
the run stops. So that gets designed here too, and it should be **held loosely
enough to abandon.** Composition is basic infrastructure, and infrastructure
converges; whatever ships natively will be better integrated than anything a
sidecar can offer.

**Policy injection.** Much of what is written today exists to keep a model honest
and on track — say what you are about to do, do not decide for them, stop before
the destructive step. A stronger model needs less of that said out loud, and every
line still being said is a line somebody should ask about. **The measure is how
much has to be repeated to get correct behaviour**, and it should fall.

**Generic workflows.** A procedure that describes how any competent operator would do
something gets absorbed, because the model learns to do it. *Read the file before
editing it. Check whether one already exists.* Those stop being worth writing.
More advanced models can do more advanced workflows, more reliably.

**What survives is the particular, not the generic.** A workflow encoding *this*
organization's release process, *this* review path, *these* obligations is not
absorbed by a better model. It is too specific, even if a generic model was
able to train on it.

**That split runs through everything here: generic is absorbed, particular is
not.**

It also explains what looks like a contradiction. Policy injection shrinks while
knowledge injection grows, and they are not opposites. Policy is mostly *how to
behave* — models get better at that on their own. Knowledge is *what is true
here* — no model gets better at that without being told.

## The same split applies to the work itself

A work item used to spell out its steps for the same reason a workflow did —
nothing else could work them out. That is the generic half, and it is being
absorbed on the same schedule, leaving the outcome and the constraints on it.
That case is made separately, in
[outcomes trump specification](thesis-outcomes-trump-specs.md).

## We are sidecar tooling, and any guarantee is bounded by that

**Nothing here controls the model, the harness, or the vendor.** Guarantees made
from beside a system are weaker than guarantees made from inside it, and saying
otherwise would be the first thing to make this untrustworthy.

So the honest form is: **as much guarantee as can be made from outside, stated
plainly, with the gaps named rather than papered over.** A check that cannot run
says so. A record that might be incomplete says so. An index that saw only what
one account can see says that too. **A guarantee that overstates itself is worse
than none**, because its passing gets read as assurance.

Textual matching is not a security boundary. A committed policy does not stop
somebody editing it. **The real boundary is always something the actor cannot
reach**, and whatever provides that in a given year, this rides on top of it —
for ergonomics, and for honesty about which of the two is which.

## What would show this wrong

**If vendors ship governance that is genuinely neutral, portable and auditable**,
the portability and governance halves of this stop being ours to provide. That is
the outcome to watch for, and it would be good for everybody — including us,
because the remaining guarantees are the ones a vendor is structurally unable to
make about itself.

**If the shrinking stops**, that is the more likely failure. A tool that keeps
adding capability while models get stronger has stopped believing its own thesis
and started defending its surface area. **The test is whether any release has ever
deleted something because the model grew into it.**
