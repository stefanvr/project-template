# {Project} — Technical specification

**Owns.** What this is built with, and which trade-offs were accepted to build it that way —
including **this project's own architectural rules**.

**Not here.** What the product does ([domain-spec.md](domain-spec.md)) · how it is operated
([implementation-spec.md](implementation-spec.md)) · what has to be true of the machine
([environment.md](environment.md)) · **the universal shape of the code**
([design-guide.md](design-guide.md)).

**Rule of thumb.** A choice belongs here if a different team could have decided differently and
still built the same product. If changing it would change what the product *does*, it is a domain
decision, not a technical one.

**Where this ends and design-guide begins.** design-guide holds what would still be true on a
completely different project — *the domain layer imports no infrastructure*. This document holds
**this project's application of that**, and its own rules besides — *months are integers 1–12
internally, with exactly one mapping table*. When a rule names a technology, a store or a domain
concept, it is this project's, and it goes here.

**Every choice names what it beat.** A technology listed without its rejected alternatives is
folklore — nobody can tell whether it was chosen or merely reached for, and nobody can revisit it
without redoing the whole analysis. One clause per rejection is enough.

Written by `/scaffold` for the initial stack, and extended per goal as further choices are made.

---

## The stack

| Concern | Chosen | Rejected, and why |
|---|---|---|
| Language | | |
| Build | | |
| Unit tests | | |
| End-to-end tests | | |
| Hosting | | |
| CI | | |

## Tooling

The commands, named the same in every project so muscle memory transfers.

- **Dev:** `npm run dev` — {what it does}
- **Tests:** `npm test` — {runner, and what it covers}
- **E2E:** `npm run test:e2e` — {tool, and how thin it is deliberately kept}
- **Coverage:** `npm run spec:coverage` — rules with no test, and tests citing rules that no longer
  exist

## Architecture

This project's own rules — each stated as something a module could violate, so it can be checked
rather than admired. The universal ones live in [design-guide.md](design-guide.md); these are what
*this* project adds or how it applies them.

- **{Rule}.** {What it forbids, and the consequence of ignoring it.}

Two shapes worth including if they apply, because both were violated in earlier builds before being
written down:

- **Where a boundary sits**, when the domain touches something external — which module owns the
  interface, and what is allowed past it.
- **How something is represented internally versus for display** — the internal type, the display
  form, and the single place that converts between them. A display string compared for logic is a
  bug that hides for months.

---

## Decisions

The valuable part of this document. One entry per decision that was not obvious.

### {Decision}

**Chosen:** {what}

**Why:** {the reasoning, in this project's actual constraints}

**Rejected:** {alternative} — {why it lost}

**Accepted risk:** {what could go wrong, and why that is tolerable for now}

> Recording the rejected option matters as much as the chosen one. Without it the same alternative
> gets re-proposed every few months and re-litigated from scratch.

### Resolving what domain-spec left open

A particular kind of decision belongs here: one the domain specification deliberately **did not**
make. Domain rules often constrain an outcome without dictating the mechanism — *"positions are
addressed by row and column"* says nothing about the internal maths; *"results are reproducible"*
says nothing about which algorithm.

When implementation forces that choice, resolve it here and say plainly that the domain spec left it
open. Otherwise it is settled implicitly by the first module that needs it, and every later module
quietly copies whatever that one did.

**Domain-spec says:** {the constraint it does impose}

**It does not specify:** {the gap}

**Resolved as:** {the choice, and where its single source of truth lives}

---

## Testing strategy

State where the **bulk** of coverage lives and why, so nobody has to guess whether a new test
belongs in the fast layer or the slow one. How tests are *organised and named* is
[design-guide.md](design-guide.md)'s; which layer carries the weight is a technical choice and
belongs here.

- **{Fast layer}** carries the bulk: {what makes it cheap here}.
- **{Slow / end-to-end layer}** is deliberately thin: {what it is reserved for}.

A split that has worked twice: test the logic through its own interface where it is cheap and
deterministic, and reserve browser or integration tests for the wiring the cheap layer structurally
cannot see. Keeping the slow layer thin is a decision that has to be defended repeatedly, because
every new feature suggests one more end-to-end test.

---

## Accepted risks

Things known to be limits, taken on deliberately. Each says what it costs, when it would bite, and
what would make fixing it worthwhile — so that hitting it later is a recognised event rather than a
surprise.

- **{Risk}.** {What it costs} · {when it bites} · {what would justify changing course}.

## Future direction

Choices made *now* specifically to keep a later option open, and what they are protecting. This is
the section that stops someone helpfully collapsing a seam that exists on purpose.

- **{Seam}.** Looks redundant today because {reason}. It exists so that {future capability} stays
  possible without a rewrite.

**Explicitly out of scope:** {list, so that *not built yet* is never mistaken for *overlooked*}.
