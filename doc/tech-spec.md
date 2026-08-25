# {Project} — Technical specification

**Owns.** What this is built with, and which trade-offs were accepted to build it that way.

**Not here.** What the product does ([domain-spec.md](domain-spec.md)) · how it is operated
([implementation-spec.md](implementation-spec.md)) · what has to be true of the machine
([environment.md](environment.md)) · how the code is shaped ([design-guide.md](design-guide.md)).

**Rule of thumb.** A choice belongs here if a different team could have decided differently and
still built the same product. If changing it would change what the product *does*, it is a domain
decision, not a technical one.

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
