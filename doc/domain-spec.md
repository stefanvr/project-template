# {Project} — Domain specification

**Owns.** What this thing *is* and the rules it obeys, independent of how it is built. Written so
someone could reason about the product — or play the game, or work the process — without reading a
line of code.

**Not here.** Technology ([tech-spec.md](tech-spec.md)) · how it is presented or operated
([implementation-spec.md](implementation-spec.md)) · what it looks like
([style-guide.md](style-guide.md)) · what gets built when
([implementation-tracking.md](implementation-tracking.md)) · **the research that led here**
([discovery/](discovery/), frozen).

**Rule of thumb.** If changing it would change *what the product does*, it belongs here. If
changing it would only change *how the product is made*, it does not.

**Identifiers.** Every numbered block carries a `DS-n.n`. Tests cite them by name, and
`npm run spec:coverage` reports rules with no test and tests citing rules that no longer exist.
A live identifier is never renumbered or reused — it is retired, and the retirement says why.

**Vocabulary is shared with the code.** An event named here is the event name in the code, and a
command named here is the command name. `Planting recorded` becomes `recordPlanting` and
`PlantingRecorded`; nothing gets renamed on the way in. This is the whole return on writing a
domain specification, and it is lost the first time a translation layer appears.

Written by `/event-storm`, one area at a time.

---

## Block types

Each area is written from a closed set of blocks. The set is closed so that the question *"which
kind of thing is this?"* has an answer, and so that anything not fitting is visible rather than
disguised.

| Block | Holds | Written as |
|---|---|---|
| **FLOW** | the timeline for this area | events in the order they occur, past tense |
| **COMMAND** | something an actor asks for | actor · what must hold · what it emits |
| **EVENT** | something that became true | past tense, named as the domain names it |
| **POLICY** | an automatic reaction | when {event, or time}, then {command} |
| **RULE** | an invariant that always holds | the statement, its exception, and why the exception exists |
| **DERIVATION** | pure computation, no state change | inputs → output, with precedence stated |
| **REFERENCE** | taxonomy and per-type values | a table |
| **READ** | a question the domain must be able to answer | who asks it, and what they need to see |

### When it does not fit

**Not everything is a command and an event, and forcing it is the main way event storming goes
wrong.** Structural facts, pure calculations, reference tables and product-wide constraints are
all real domain content with no event in sight. Three of these are load-bearing in the projects
this template came from: a season resolver that computes a state from a month and never writes
anything, a movement-reach function over terrain costs, and *"the product never blocks on an
unanswered question"* — a constraint on everything, attached to nothing.

**The fallback is RULE**, and it carries an identifier like every other block, so coverage and
traceability are unaffected by which shape a thing took. DERIVATION is the fallback for anything
that computes rather than happens.

A **ceremonial sticky** is one invented to satisfy the format. Three symptoms, all mechanical
enough that `/sanity-check` reports them:

- an event nothing subscribes to, and that nothing outside its own aggregate can observe;
- a command whose only actor is "the system", with no policy triggering it;
- an event named as a noun plus *Updated* or *Changed*, which is a database row talking, not a
  domain.

When you find one, delete it and write the rule it was standing in for.

---

> ### ⚠️ Illustrative example — delete this whole block
>
> Filled in with a small worked example, because the hard thing to convey abstractly is *how much
> detail to write*. An empty stub teaches nothing about density. Read it, then delete it.
>
> ---
>
> ## 1. Lending
>
> **FLOW**
> `Item borrowed` → `Loan renewed`\* → `Item returned`
> `Item reserved` → `Reservation became collectable` → `Reservation collected` | `Reservation lapsed`
>
> **COMMAND — Borrow item**
> *Actor:* member, at the desk or a self-service terminal.
> *Requires:* the item is loanable (**DS-1.4**) and the member is within their caps (**DS-1.2**)
> and not blocked (**DS-1.3**).
> *Emits:* `Item borrowed`.
>
> **COMMAND — Renew loan**
> *Actor:* member. *Requires:* renewals remain for the item type (**REFERENCE** below), and the
> item is not reserved by someone else. *Emits:* `Loan renewed`.
>
> **POLICY**
> When `Item returned` **and** a reservation queue exists for it → `Reservation became collectable`
> for the member at the head of the queue.
>
> **POLICY**
> When three days pass without collection → `Reservation lapsed`, then the queue is re-evaluated
> (**DS-1.5**). *Time is the trigger; no actor commands this.*
>
> **REFERENCE — item types**
>
> | Item type | Loan period | Renewals | Reservable | Notes |
> |---|---|---|---|---|
> | Book | 21 days | 2 | Yes | — |
> | Reference book | — | — | No | Never leaves the building |
> | DVD | 7 days | 0 | Yes | — |
> | E-book | 21 days | Unlimited | No | Licence-limited concurrent copies |
>
> **RULES**
>
> - **[DS-1.2]** A member may hold at most 10 items at once, of which at most 3 may be DVDs.
> - **[DS-1.3]** An overdue item blocks new loans — but **not** returns, renewals of *other*
>   items, or collecting a reservation already being held. Both halves matter: the block exists to
>   get items back, so it must never make returning one harder.
> - **[DS-1.4]** A reference book is never loanable, by any actor, under any override. It is the
>   one item type with no borrow path at all. [?H1]
> - **[DS-1.5]** A lapsed reservation passes to the next member in the queue, not back to the
>   shelf. A queue that silently empties is indistinguishable from one nobody wanted.
>
> Note what **DS-1.3** does: it names its own exceptions explicitly, and says *why*, in one clause.
> That is the density to aim for. Compare it with *"overdue items restrict borrowing"*, which reads
> fine and settles nothing.
>
> **DERIVATION — due date**
> *Inputs:* borrow date, item type, member category.
> *Output:* a due date.
> *Precedence:* member category overrides item type where they disagree; a closed day pushes the
> date forward, never back. **[DS-1.6]**
> *No event.* Nothing happens when a due date is computed — it is a function of facts already
> recorded, and recomputing it must always give the same answer.
>
> **READ — what a member owes**
> *Asked by:* the member, and by the desk before permitting a loan.
> *Needs to show:* items held, each with its due date and renewal count; anything overdue, first.
>
> ---
>
> ## 2. Interactions and edge cases
>
> Where two rules meet ambiguously, resolve it *here*, in writing, before implementation has to
> guess.
>
> - **[DS-2.1] A reserved item is returned overdue, and the next member in the queue is also
>   blocked.** The three-day hold starts anyway; being blocked prevents collection, not
>   reservation. If the hold lapses uncollected it passes on as normal (**DS-1.5**).
>   *Example:* due back Monday, returned Thursday. Member B's hold runs Thursday–Sunday. B owes a
>   fine and cannot collect. Sunday midnight it passes to member C.
> - **[DS-2.2] A member hits the 10-item cap mid-collection.** The reservation is forfeited, not
>   held — otherwise a capped member could park items indefinitely.
>
> Worked examples earn their space. One concrete *"X happens, so Y, leaving Z"* is worth several
> paragraphs of careful qualification, and it doubles as a test case.

---

## 1. {Area}

**FLOW**

**COMMAND — {name}**
*Actor:* · *Requires:* · *Emits:*

**RULES**

- **[DS-1.1]** {A rule that always holds.}
- **[DS-1.2]** {A rule with its exception named explicitly, plus why the exception exists — which
  is usually the part that stops it being "fixed" later by someone who missed it.} [?H1]

---

## Open items

The unresolved arguments — event storming's hotspots. **One list for the whole document**, so the
review queue is visible in one place; areas above point at it with `[?Hn]` rather than restating.

Each says what is currently true, what the alternative is, and what would settle it. Keeping them
here rather than in someone's head is what stops them being silently re-decided by whoever touches
that code next.

- **[H1]** {The question.} Currently {behaviour}. The alternative is {alternative}, which would
  {effect}. Settled by {what would settle it}.

An open item is closed by writing the decision into the rule it troubled and deleting the `[?Hn]`
reference. The entry moves to **Settled** with one line on what was decided, so a question does not
get re-opened by someone who was not there.

## Settled

| Was | Decided | When |
|---|---|---|
| — | | |

---

## Future — what this product might also do

Domain ideas deliberately not in this release: things the product could *be*, not ways it could
look. Each is a sentence, not a design. UX ideas go in
[implementation-spec.md](implementation-spec.md); *"decided, but not now"* goes to
[implementation-tracking.md](implementation-tracking.md)'s **Parked**.

The distinction is what stops this section becoming a wishlist: here is *not decided yet*, Parked
is *decided against, for now*.
