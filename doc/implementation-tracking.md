# Implementation tracking

**Owns.** What is being built now, what is agreed next, what is deliberately set aside.

**Not here.** How work gets done ([workflow.md](workflow.md)) · what the product does
([domain-spec.md](domain-spec.md)) · how it behaves ([implementation-spec.md](implementation-spec.md)).

> **Copying this template?** Clear everything below the rules and start with an empty **Backlog**.
> What is here is this repository's own build, kept as the worked example — the hard thing to
> convey abstractly is how much to write, and an empty stub teaches nothing about density.

**The deletion rule.** Everything under a goal is scaffolding. When a goal lands, its text is
deleted and only a row in **Done** remains. That is only safe because anything learned was
promoted into the document that owns it first — see workflow.md's *Promote before you tick*. The
test is mechanical: delete the text, and see whether anything is now missing.

**WIP limit: one goal in Now.** A second goal in progress means neither is being finished.

---

## Now

### Goal: the loop runs itself — a goal goes from backlog to merged without anyone remembering the workflow

**Outcome:** the next goal can be taken from Backlog to a pushed branch using only `/plan-goal` and
`/build-stage`, with `workflow.md` unopened.

**Try it:** run `/plan-goal` on the `spec:coverage` backlog item, then `/build-stage` on the
result. If either needs the workflow document open to know what happens next, the goal is not done.

**Covers:** `workflow.md`'s per-goal loop, its two gates, and the promotion rule.

**Signed off** 2026-08-25 — input given at the gate on all three open questions, which is the gate
working rather than being skipped.

- [x] Three standing decisions promoted into `workflow.md`: spec input requested at the planning
      gate, git performed rather than narrated, promotion refused rather than prompted
- [x] `/plan-goal` — the four movements, ending with the sign-off that asks for spec input.
      *Nothing to promote beyond the skill itself, except the question bar* — a question answerable
      by reading the repository is not a question for the user — which is operational and lives
      where it is used
- [x] `/build-stage` — branch, specify, stop at the second gate, implement one commit per item,
      verify, refuse to tick without a promotion target, push. *Nothing to promote;* the boundary
      it enforces (it never merges) was already written into `workflow.md`
- [x] Both skills hold the WIP limit → `workflow.md`, which now also says an override must be
      **recorded**. Writing the decline path is what surfaced that: a limit with an unrecorded
      exception is indistinguishable from no limit a week later
- [x] **Ad hoc:** branching default corrected → `workflow.md`. `main` is the integration branch and
      every goal gets a `goal/{slug}` feature branch; a long-running integration branch is now
      recorded as **rejected with its reasons**, rather than merely absent, so it cannot return as
      a fresh good idea. Found because this repository's own first twelve commits went straight to
      `main`
- [x] **Ad hoc:** the `Ad hoc:` convention itself → `workflow.md` §Work found mid-goal. Writing the
      item above is what surfaced that v2 had dropped v1's convention without ever deciding to,
      leaving no home for real work found mid-goal
- [x] **Ad hoc:** remote added and both branches pushed, so the workflow's step 6 is satisfiable at
      all → the SSH-rather-than-HTTPS finding is recorded as a forward obligation on the
      supporting-documents goal, because `environment.md` does not exist yet to receive it

---

## Planning

*Empty. Goals are planned one at a time, when they are about to be built.*

<details>
<summary>The goal now in <b>Now</b>, as it was planned — kept once, as the worked example of this section</summary>

### Goal: the loop runs itself — a goal goes from backlog to merged without anyone remembering the workflow

**Discussion.** `workflow.md` describes the loop well and is already too long to re-read every
time, which is exactly the failure v1 had with instructions at the top of documents. Two skills
carry it instead: `/plan-goal` for the four planning movements, `/build-stage` for branch, specify,
implement, verify, promote, push.

The interesting half is `/build-stage`, because it owns the promotion rule — and the promotion
rule is the load-bearing one. Everything else in the tracking format depends on nothing being
ticked before what it taught has been written down somewhere durable. A skill that merely mentions
promotion will be as ignored as a document that mentions it.

Two skills rather than one, on SRP grounds: planning and building fail differently and are picked
up at different moments. The handoff between them is the risk, so the goal's checklist has to
cover sign-off state surviving it.

**Open questions** — needed before the proposal is worth writing:

- **How hard does `/build-stage` enforce promote-before-tick?** It can prompt (*"what did this
  teach, and where does it go?"*), or it can refuse to tick an item until a promotion target is
  named — with an explicit *nothing to promote* answer available, since plenty of items teach
  nothing. Refusing is the only version that survives a tired session, and it is also the version
  that gets resented on item nine of nine.
- **Does `/build-stage` run git itself?** Branch, commit per checklist item, push before review —
  the workflow specifies all three. A skill that performs them makes the routine real; a skill that
  guides them keeps you looking at every commit. This is a standing preference worth setting once,
  and it belongs in `workflow.md` rather than in a chat.
- **Two sign-off gates or one?** Today there is one on the goal in planning and another on the
  specification before code. For a solo build with an assistant that can produce the specification
  in a minute, the second may be the same conversation twice — or it may be the one that catches
  the wrong assumption while it is still a paragraph. Q&C already found the second gate valuable
  enough to write an opt-out for, which is evidence in both directions.

**Proposal.** Two skills, plus the three standing decisions written into `workflow.md` first so the
skills point at a document that already agrees with them. Checklist and **Try it** as they now
appear in **Now**.

**Sign-off:** ☑ 2026-08-25.

</details>

---

## Backlog

Goals, not tasks. Each is one line stating an outcome; it gets its checklist when it reaches
Planning, not before.

- [ ] **A rule with no test, and a test citing a rule that no longer exists, are both findable by
      running a command.** `DS-`/`IS-` identifiers, `spec:coverage`, and `/sanity-check` on top.
      - **Carries a forward obligation from the domain-spec goal:** `/sanity-check` must report
        **ceremonial stickies** — an event nothing subscribes to, a command whose only actor is
        "the system" with no policy triggering it, an event named noun-plus-*Updated*. These are
        the three mechanical symptoms of event storming being forced onto something that is not a
        workflow, which is the failure mode most likely to bite. Defined in
        [domain-spec.md](domain-spec.md)'s *When it does not fit*; nothing enforces it yet.
      - **And two from the journeys goal:** the backbone diagram and the journey steps must agree
        in both directions, and a journey whose named surfaces are never exercised together by any
        end-to-end test must be reported. The second is the only coverage a journey has, since
        journeys deliberately carry no identifiers.
- [ ] **A new project reaches a deployed, tested skeleton without re-solving the toolchain.**
      `/scaffold` with recipes, writing `tech-spec.md` and `environment.md` as it goes. Recipes are
      directories in the skill — a proven setup is promoted by adding one.
- [ ] **The supporting documents exist in v2 shape.** `tech-spec`, `environment`, `style-guide`,
      `design-guide` — each reduced to its Owns/Not here header plus what a real project fills in.
      - **Carries a forward obligation:** `environment.md` must record that the GitHub remote is
        reached over **SSH**, not the HTTPS URL — HTTPS is not configured here, and it prompts for
        credentials no helper supplies, so it hangs rather than failing. And that git runs *inside
        WSL*, because the Windows host carries a different identity and will attribute commits to
        the wrong person without warning. Both are silent failures, which is the class of problem
        `environment.md` exists for.
- [ ] **The template is proven against a real project.** Rebuild Garden's documents in v2 shape and
      see what the format cannot express. This is the goal that finds the design errors, and it is
      the only real test of the *not everything fits a command and an event* worry.

---

## Parked

Real, decided against for now, with what would unpark it.

- [ ] **Wardley mapping as a template artifact.** Named as an inspiration but no wish attached to
      it, and it answers a question — *where is this heading strategically* — that neither of the
      two real projects has asked. Nothing in the current document set is worse for its absence.
      Unpark it when a project has an actual make-versus-buy or evolution question to settle, at
      which point it is a `doc/discovery/` artifact rather than a maintained document.
- [ ] **Generating the Done table from git history.** The rows are cheap to write by hand and the
      information is not identical — Done records *which document received the knowledge*, which
      no tag knows. Unpark it if Done ever grows past the point of being read.

---

## Release gap

Domain areas with no goal in any state. Maintained by `/sanity-check` once `domain-spec.md`
carries identifiers — until then, deliberately empty rather than guessed at.

---

## Done

| Goal | Landed | Knowledge promoted to |
|---|---|---|
| The process that builds the rest of this template exists and governs its own construction | 2026-08-25 | `workflow.md` (goal loop, four planning movements, promotion rule) · `implementation-tracking.md` (Kanban states, WIP limit, deletion rule) · `README.md`, `CLAUDE.md` (decision routing) · `code-conventions.md` renamed `design-guide.md`, reframed as design and architecture guidance |
| The domain can be captured as an event storm, iteratively, without the research leaking into it | 2026-08-25 | `domain-spec.md` (block vocabulary, `DS-n.n` identifiers, `[?Hn]` open-item references, *When it does not fit*) · `.claude/skills/event-storm` (interview style, one area per session, past-tense events sharing the code's names) · `.claude/skills/discover` (frozen input artifacts, the freeze rule) |
| The journeys through a product can be written down, and sliced into goals | 2026-08-25 | `implementation-spec.md` (strict journey/surface split, `IS-n.n` on surfaces only with journeys naming the surfaces they cross, mermaid backbone, *Future — how this might work*) · `.claude/skills/story-map` (propose-then-walk rather than interview, slices never layers, surfaces written per goal not up front) |
