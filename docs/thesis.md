# Thesis

**Models will keep getting stronger, so these tools should keep doing less.**

That is the bet, and it is deliberate rather than defensive. A tool that has to do
a great deal is compensating for a model that cannot yet do it. Every capability
that becomes native is a capability worth deleting — and a version of luma that
does less than the last one, because the model grew into it, is a success.

**Two things shrink first, and they are the ones to watch.**

**Policy injection.** Much of what is written today exists to keep a model honest
and on track — say what you are about to do, do not decide for them, stop before
the destructive step. A stronger model needs less of that said out loud, and every
line still being said is a line somebody should ask about. **The measure is how
much has to be repeated to get correct behaviour**, and it should fall.

**Workflows.** A procedure that describes how any competent operator would do
something gets absorbed, because the model learns to do it. *Read the file before
editing it. Check whether one already exists.* Those stop being worth writing.

**What survives is the particular, not the generic.** A workflow encoding *this
organization's* release process, *this* review path, *these* obligations is not
absorbed by a better model — it was never in the training data. The same split
runs through the whole thesis: **generic procedure is absorbed; specific procedure
is not.** And it is why policy injection shrinks while knowledge injection grows —
policy is mostly *how to behave*, which models get better at unaided, and
knowledge is *what is true here*, which they cannot get better at without being
told.

**What does not shrink is the guarantee.** As models absorb the doing, the value
moves to what a model cannot promise about itself. That set does not get smaller
with scale. Some of it gets larger.

## The six that stay

**Governance.** A thing cannot bind itself. Rules an agent can rewrite are
suggestions, and an agent asked to follow rules it holds in its own context is
being trusted rather than governed. What binds has to sit outside the thing being
bound, and stay there.

**Observability.** The actor is the worst available witness. A record an agent
writes about its own behaviour is a claim, not evidence, and it is least reliable
in exactly the cases anybody would want it — when something went wrong, when a
step was skipped, when a check did not run. Observability has to be produced by
something that is not the subject.

**Ease of use.** As models take over the doing, a person's remaining job is
deciding — and deciding requires the state to be legible without reconstructing
it. This one shrinks least obviously and matters most: a guarantee nobody can
operate is not a guarantee, and every layer of ceremony spends the attention that
was supposed to go to the decision.

**Portability.** A vendor cannot be neutral about itself, and will not be. What an
organization learns has to outlive the model it was written with, the harness it
was written in, and the company that shipped both. Plain files, one format, no
runtime — because the durable thing has to survive its tooling.

**Data privacy.** A boundary enforced by the thing it constrains is not a
boundary. What leaves an organization, what reaches a vendor, what lands in a
public repository — none of that can be left to the judgement of the system doing
the reaching. It has to be structural, checkable, and refuse by default.

**Knowledge injection.** A model knows the world. It does not know *this*
organization — what was decided, what was tried, what was ruled out and why. That
gap does not close with scale, because the missing material is private by
construction and was never in the training data. **This is the one that grows.** A
stronger model with an organization's context outperforms a stronger model
without it by more, not less, as both get stronger.

## We are sidecar tooling, and the guarantee is bounded by that

**Nothing here controls the model, the harness, or the vendor.** Guarantees made
from beside a system are weaker than guarantees made from inside it, and saying
otherwise would be the first thing to make this untrustworthy.

So the honest form is: **as much guarantee as can be made from outside, stated
plainly, with the gaps named rather than papered over.** A check that cannot run
says so. A record that might be incomplete says so. An index that saw only what
one account can see says that too. **A guarantee that overstates itself is worse
than none**, because its passing gets read as assurance.

Textual matching is not a security boundary. A committed policy does not stop
somebody editing it. Sandboxing is the boundary; this rides on top for ergonomics
and honesty about what it is.

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
