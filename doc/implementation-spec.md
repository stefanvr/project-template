# {Project} — Implementation specification

**Owns.** How the product is operated and presented: what someone is trying to get done, and how
each surface behaves while they do it.

**Not here.** What the product *is* and the rules it obeys
([domain-spec.md](domain-spec.md)) · technology ([tech-spec.md](tech-spec.md)) · tokens and visual
states ([style-guide.md](style-guide.md)) · which of this is in the current release
([implementation-tracking.md](implementation-tracking.md)).

**Rule of thumb.** If it would still be true with a completely different interface, it belongs in
the domain spec. If it changes when the interface changes, it belongs here.

**Identifiers.** `IS-n.n` sits on **surface behaviours only**. Journeys carry none — they are
narrative, they get reworded often, and identity on a volatile thing is how citations rot. A
journey instead *names the surfaces it passes through*, and its end-to-end test cites that set.
Identity lives on the stable thing; the volatile thing points at it, exactly as `[?Hn]` works in
the domain spec.

Written by `/story-map`. Surface sections are filled in per goal, not up front —
see [workflow.md](workflow.md).

---

## How the two parts divide

**A journey says what someone does. A surface says how it behaves.** No sentence appears in both.

The division is strict, and it is what stops this document collapsing back into one pile:

| | Journey | Surface |
|---|---|---|
| Voice | *"picks a border, taps a clump"* | *"a tap under the drag threshold selects; past it, it is a pan"* |
| Changes when | the product's purpose changes | the interface changes |
| Reads as | continuous prose | a specification |

When a surface is central to a journey it is tempting to restate its behaviour so the journey
reads well. Don't. Name what the person is doing and link; a journey that has to explain a widget
is describing the widget, and the widget already has a section.

## Backbone

Small on purpose. It orients the prose below; it is not a map of the product, and it does not try
to be exhaustive. Every step here appears as a journey step below, and no journey step is missing
from it — `/sanity-check` reports either way round.

Mermaid renders in GitHub and in most editors' preview, so the diagram stays in the document
rather than beside it as an image nobody regenerates.

```mermaid
flowchart LR
  subgraph A[Find something]
    A1[search] --> A2[read the entry]
  end
  subgraph B[Take it out]
    B1[check what you owe] --> B2[confirm]
  end
  subgraph C[Get it back]
    C1[renew] --> C2[return]
  end
  A --> B --> C
```

---

> ### ⚠️ Illustrative example — delete this whole block
>
> Continues the worked example from [domain-spec.md](domain-spec.md), so the two documents can be
> read against each other and the split between them is visible on the same content.
>
> ---
>
> # Part 1 — Journeys
>
> ## Take something out
> *Surfaces: §1 catalogue search · §2 item entry · §3 what you owe*
>
> A member arrives knowing roughly what they want and searches for it (§1). The entry tells them
> whether it can be borrowed at all and whether a copy is here now (§2) — a reference book says so
> before they walk to the shelf, not after. Borrowing is one action from the entry; where it is
> refused, the reason is the member's own position rather than a rule quoted at them (§3), because
> *"you are holding three DVDs"* is actionable and *"DVD limit exceeded"* is not.
>
> ## Get something you are waiting for
> *Surfaces: §2 item entry · §3 what you owe · §4 the hold shelf*
>
> The member reserves from the entry (§2) and is told where they are in the queue, since a queue
> position is the difference between waiting and giving up. When the item becomes collectable they
> have three days, and the countdown is visible wherever they look at their own account (§3).
> Being blocked does not hide the hold — it prevents collection, and saying so plainly is what
> stops a member assuming the reservation was lost (**DS-2.1**).
>
> ---
>
> # Part 2 — Surfaces
>
> ## 1. Catalogue search
> *Serves: find something → search*
>
> - **[IS-1.1]** A query matches title, author and series; results are ranked by availability
>   first, then relevance. An unavailable exact match still ranks above an available near miss.
> - **[IS-1.2]** Zero results offers the nearest spellings rather than an empty page, and says how
>   many results each would give — a suggestion leading to another empty page costs a second
>   round trip.
>
> ## 2. Item entry
> *Serves: find something → read the entry · take it out → confirm · get something you are waiting
> for → reserve*
>
> - **[IS-2.1]** Shows loan period, renewals allowed, and whether a copy is on the shelf now, above
>   the fold. These three decide whether the member walks to the shelf.
> - **[IS-2.2]** A non-loanable type (**DS-1.4**) shows *reference only — not borrowable* in place
>   of the borrow action, never a disabled button. A disabled control reads as a temporary state
>   and invites waiting.
> - **[IS-2.3]** Reserving shows the resulting queue position immediately, not a confirmation.
>
> ## 3. What you owe
> *Serves: take it out → check what you owe · get something you are waiting for → collect*
>
> - **[IS-3.1]** Overdue items sort first, then holds expiring soonest, then everything else. The
>   order is the priority the member should act in.
> - **[IS-3.2]** A block (**DS-1.3**) is shown as what it prevents and what clears it, never as a
>   status word. *"Returning `Dune` restores borrowing"* rather than *"account blocked"*.

---

# Part 1 — Journeys

## {Activity}
*Surfaces: §n {name} · §n {name}*

{Short narrative prose. What the person is trying to do, in order, linking to the surface that
does each part. Domain rules are cited by identifier where the journey turns on one.}

---

# Part 2 — Surfaces

## 1. {Surface}
*Serves: {journey} → {step}*

- **[IS-1.1]** {A behaviour, stated so it could be tested. Where a naive alternative exists, say
  in one clause why it was rejected — that is what stops it being "fixed" back later.}

---

# Future — how this might work

UX ideas deliberately not in this release: ways the product could look or be operated, as against
things it could *be*, which belong in [domain-spec.md](domain-spec.md)'s own Future section.
*Decided against, for now* goes to [implementation-tracking.md](implementation-tracking.md)'s
**Parked** — this section is for what has not been decided at all.
