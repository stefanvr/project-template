# Workflow

**Owns.** How work gets done: the goal loop, the review gates, branching and commit granularity,
and how documents stay true as work lands.

**Not here.** Anything about the product. If a statement would still hold on a completely
different project, it belongs here; if it would change, it belongs in one of the specifications.

> **Integration branch for this project:** `{main | long-running branch name}`
>
> One of the two. Either works; not knowing which is in use is what causes trouble.

---

## Work is goals, not tasks

A **goal** states an outcome — what becomes true for someone once it lands. Not a list of things
to build; the list is how you get there and is thrown away afterwards.

> **Goal:** the owner can stand in the garden, pick a border, tap a clump, and see what is known
> about that specific planting.

versus what a task list would have said — *render SVG layout, add hit-testing, build detail
panel*. The second is not wrong, it just cannot tell you whether it is finished, or whether it was
worth doing.

Goals live in [implementation-tracking.md](implementation-tracking.md) in four states, plus one
siding:

**Backlog** → **Planning** → **Now** → **Done**, and **Parked** off to the side.

**The WIP limit is one goal in Now.** Not a guideline. A second goal in progress means neither is
being finished, and the first thing lost is the honesty of the tracking document.

---

## Per goal

### 1. Plan it — `/plan-goal`

A goal leaves Backlog by being talked through, in four movements. This is the most valuable half
hour in the loop and the cheapest place to be wrong.

1. **Discussion.** What is this really for, what does it depend on, what does it unlock. What
   almost-identical thing already exists that this should reuse rather than repeat.
2. **Open questions.** The things that genuinely need *your* input, asked specifically. Not "how
   should this work" but "the domain spec leaves `rustperiode` unresolved — do we show unknown, or
   seed from general knowledge and mark it `seeded`?" A question you can answer in a word is a
   good question; one that requires a design session was not ready to be asked.
3. **Proposal.** The checklist, the **Try it** line, and which `DS-`/`IS-` rules the goal covers.
4. **Sign-off.** Explicit. Nothing moves to **Now** without it.

**Answer the Try it line during planning, not after.** It is the sequencing rule's only real
enforcement: a goal you cannot describe exercising depends on something that does not exist yet,
and finding that out costs a sentence now instead of a rewrite later. "The code is there" is a
failed answer.

**Plan the goal you are about to build, not the four after it.** A plan written four goals early
was written by someone who had not built the first four.

### 2. Branch

One branch per goal, off the integration branch. Keeps the goal reviewable as a unit, and
reverting it a single operation.

### 3. Specify, then stop

Write the [implementation-spec.md](implementation-spec.md) surface sections the goal needs — it is
organized by journey and surface, not by goal, so a goal typically touches several sections.

**Then stop and wait for explicit sign-off before writing implementation code.** The cheapest
review gate in the loop: a wrong assumption costs a paragraph here and a day of rework once the
code exists.

**Opt-out.** When the reviewer explicitly says to skip the wait for a given goal — *"if there are
no significant questions, spec it and start, I'll review after"* — then note any genuine open
design question *in the specification text itself*, pick the sensible default for each rather than
blocking, and proceed. Review happens against the finished result instead. Per-request, not a
change to the default.

### 4. Implement, one commit per checklist step

A separate commit per item, not one per goal. Bundling makes the diff unreviewable and bisecting
useless. Commit messages say **why** — the what is in the diff. Decisions, rejected alternatives,
and anything surprising are worth the lines.

### 5. Verify before claiming done

Run the **full** suite, not just what you touched. Report failures plainly, with output; a skipped
step is said out loud, not quietly dropped.

**Look at anything visual.** A passing test says the code ran, not that the result is right. A
colour at 18% opacity over dark terrain draws correctly and is effectively invisible — only
looking found that.

**Write throwaway verification for anything you are reasoning about rather than observing.** A
scratch script that runs the real functions and prints what actually happened — a seeded
simulation over many iterations, a screenshot, a direct check of a computed value — repeatedly
catches what careful thinking misses: off-by-one errors in hand-worked coordinates, a rule that
never fires, a fixture that is not what you meant. Delete it in the same session.

Two traps, both of which have cost real time:

- **Do not write scratch output where the dev server watches it.** A live-reloading server reloads
  the page mid-run and resets the state you were inspecting, and the failure reads as an
  application bug rather than a tooling one.
- **A test failing after a deliberate rule change may be asserting the old behaviour.** Before
  "fixing" anything, decide which of the two is right. A test written when the old rule held is
  evidence about the old rule — but it is equally possible the rule change was wrong and the test
  is saying so. Read it before touching either.

### 6. Push, review, merge

Push the branch as soon as the goal is complete — **before** review and before any merge. The work
is off your machine from that moment, and the reviewer has something to look at that is not your
working copy.

Nothing merges before review. Then merge into the integration branch and push that; because the
branch was already pushed, its history survives on the remote independently rather than only
implicitly inside the integration branch. Delete the branch once merged and confirmed.

---

## Promote before you tick

**This is the rule the tracking document depends on.** A goal's text is deleted when it lands, so
anything learned along the way must already be somewhere else.

Before checking an item off, ask what it taught, and put that where it belongs:

| What you learned | Where it goes |
|---|---|
| A rule of the product is different than written | `domain-spec.md` — fix the rule, same change as the code |
| A behaviour ended up different than specified | `implementation-spec.md` — it describes the end state |
| A technical choice, or a risk now accepted | `tech-spec.md` |
| A machine behaves in a way that surprises | `environment.md` |
| A convention proved itself | `design-guide.md` |
| Why this specific change was made | the commit message |
| Real work found, but not to be done now | **Parked**, with what would unpark it |

Then the checklist line is one line — what was done, and where the knowledge went:

```
- [x] Rules deploy on merge, Firestore only → environment.md §Firebase deploy credentials
```

Compare that with carrying ten lines of narrative in the tracking document, where it is read once
and then scrolled past forever. **The test is mechanical: delete the goal's text. If something is
now missing, promotion was not finished.**

## Defer honestly

Work found but not to be done now goes to **Parked** with three things: what is wrong, why it was
not fixed then, and what fixing it would take. *"Not a minor tweak"* is useful; *"improve X"* is
not. Anything parked because it needs a **decision** rather than work says which decision, and
what the options are. That is what makes it resumable by someone else.

Parked is a siding, not a bin. `/sanity-check` reads it.

## When a rule turns out to be wrong

Specifications get things wrong. When implementation shows a documented rule does not hold:

1. Fix the document that owns the rule, in the same change as the code.
2. Say plainly in the commit message that it is a reversal, and why the original reasoning failed.

Do not leave a document asserting something the code no longer does. A specification nobody trusts
is worse than none, because people keep half-believing it.

Watch specifically for **implementation limitations leaking into rules** — *"it works this way
because that was awkward to build"* is a bug in the document, not a design decision. This has
happened before: a transfer was specified as free because neither container tracked an action
budget, which is a limitation dictating a rule rather than the reverse. Write the rule the domain
actually wants, then record the gap if the implementation cannot meet it yet.
