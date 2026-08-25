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

### Goal: a project copied from the template starts clean, and the copy sequence is stated once

**Outcome:** following the copy sequence leaves nothing of the template's own behind, and the
sequence exists in one place rather than three that can drift apart.

**Try it:** copy the template to a scratch directory, follow `.dev-template`, then grep the result
for the word *template* — nothing describing the dev template itself remains. Delete the scratch
directory afterwards.

**Covers:** none — process documents.

**Signed off** 2026-08-25, with **movements 1 and 2 collapsed**: there were no open questions, both
defects were found and decided in the same message. That collapsing is now a rule rather than a
corner cut — see the third item.

- [x] `.dev-template` now names `README.md` — every project would otherwise inherit one describing
      the template — and settles `tools/`: **keep it in full**, tests and fixtures included, because
      the tools ship into every project as code, and code that ships untested is what
      `design-guide.md` argues against. It also says what *not* to delete: `environment.md`'s silent
      failures are about the machine and stay true everywhere
- [x] The sequence is stated **once**, in `.dev-template` → `README.md` and `/scaffold` now point at
      it instead of restating it. It was written in three places, which is three places to forget a
      step — and the omission of `README.md` had already happened in all three
- [ ] The gate is proportional: a goal with no open questions is planned in one message →
      `workflow.md` and `/plan-goal`

---

## Planning
*Empty. Goals are planned one at a time, when they are about to be built.*

<details>
<summary>The loop goal, as it was planned — kept once, as the worked example of this section</summary>

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

- [ ] **The template is proven by starting a real project with it.** Trazer — a Traz/Arkanoid-type
      game — in a fresh repository copied from this template, taken to one playable slice using
      `/scaffold`, `/discover`, `/event-storm`, `/story-map`, `/plan-goal` and `/build-stage`. Every
      format failure found is recorded back here.
      - **Why Trazer rather than the alternatives:** it is the adversarial case. An arcade game is
        simulation — physics, collision, per-frame state — with barely any domain events, which is
        precisely the shape event storming fits worst. If `domain-spec.md`'s *When it does not fit*
        fallbacks hold there, they hold anywhere. It is also small, and existing input for it can be
        frozen as the first `/discover` artifact rather than manufacturing one.
      - **What it exercises that nothing has.** Four of the seven skills have never run: `/discover`,
        `/event-storm`, `/story-map` and `/scaffold`. Only `/plan-goal` and `/build-stage` have been
        used for real, and `/scaffold`'s recipe was verified by hand-copying rather than by running
        the skill. A conversion would test the document formats; only a greenfield start tests the
        skills, which are the less proven half.
      - **A conversion is not a greenfield start**, and the earlier wording of this item confused
        them. Re-deriving Garden from its own domain-spec is a greenfield start with unusually good
        input, not a rebuild — see **Parked**.
- [ ] **The documents can be audited against the code in one pass.** `/sanity-check`, which runs the
      coverage script and adds the judgement calls it cannot make.
      - **Carries a forward obligation from the domain-spec goal:** it must report **ceremonial
        stickies** — an event nothing subscribes to, a command whose only actor is "the system" with
        no policy triggering it, an event named noun-plus-*Updated*. These are the three mechanical
        symptoms of event storming being forced onto something that is not a workflow, which is the
        failure mode most likely to bite. Defined in [domain-spec.md](domain-spec.md)'s *When it
        does not fit*; nothing enforces it yet.
      - **Deliberately after the Trazer test:** run against this repository today it would report
        three uncovered stubs and stop. Its three obligations all need real specifications, real
        modules citing them and a real end-to-end suite — none of which exist here.
      - **And two from the journeys goal:** the backbone diagram and the journey steps must agree
        in both directions; and a journey whose named surfaces are never exercised together by any
        end-to-end test must be reported. The second is the only coverage a journey has, since
        journeys deliberately carry no identifiers — it needs `*Surfaces: §n …*` resolved to `IS-`
        identifiers, then an end-to-end file citing all of them together. Moved here from the
        coverage goal, which kept itself to the mechanical check.

---

## Parked

Real, decided against for now, with what would unpark it.

- [ ] **Converting the existing projects to v2.** Worth doing in the longer run, not now, and not
      uniformly:
      - **`web-garden`** is the strongest candidate and the one that delivers real value. Its current
        `domain-spec.md` is, by its own author's account, the research that should have produced a
        domain spec — so it is already a `/discover` artifact. Freeze it and re-derive rather than
        converting section by section. Unpark once Trazer has proven the template, so the value
        project is not the one finding the format's errors.
      - **`risky.turn`** shares most of its domain with Q&C, so it inherits those lessons directly.
        A good project; a weak *test*, because a domain already in your head makes the `/event-storm`
        interview a recitation rather than a discovery.
      - **`query-and-conquer` is superseded, not converted.** Its AI approach needs a complete
        overhaul, and the lessons are better spent starting `risky.turn` than retrofitting v1.
      - **`gym` is out of scope.** It predates Q&C's `build-v1` and had template v1 applied; nothing
        is gained by dragging it forward.

- [ ] **Wardley mapping as a template artifact.** Named as an inspiration but no wish attached to
      it, and it answers a question — *where is this heading strategically* — that neither of the
      two real projects has asked. Nothing in the current document set is worse for its absence.
      Unpark it when a project has an actual make-versus-buy or evolution question to settle, at
      which point it is a `doc/discovery/` artifact rather than a maintained document.

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
| A new project reaches a working, tested skeleton without re-solving the toolchain | 2026-08-25 | `.claude/skills/scaffold` (the `.dev-template` guard, the interview, install-before-commit, guided-and-verified remote and Pages, deploy-on-day-one) · `recipes/vite-ts` with `RECIPE.md` (Node 24 LTS, why no lockfile ships, `configure-pages` rejected in place with its evidence) · `doc/tech-spec.md` and `doc/environment.md` in v2 shape, the latter carrying silent failures 1–5 · `doc/workflow.md` (suspect the scratch script before the system) · `.claude/skills/plan-goal` (checklists that count one artifact twice; empty **Covers** on a toolchain goal) · `README.md` (Starting a project) |
| The loop runs itself — a goal goes from backlog to merged without anyone remembering the workflow | 2026-08-25 | `workflow.md` (spec input asked at the planning gate · git performed on instruction, never on the skill's own judgement · promotion refused rather than prompted · WIP overrides recorded · `main` as integration branch, long-running branches rejected with reasons · §Work found mid-goal · landing needs its own branch) · `.claude/skills/plan-goal` and `.claude/skills/build-stage` (the loop itself; refusals that name the next command; in-progress distinguished from done-but-not-landed) · **Backlog** (SSH-not-HTTPS and git-inside-WSL, as a forward obligation on the `environment.md` goal) |
| A rule with no test, and a test citing a rule that no longer exists, are both findable by running a command | 2026-08-25 | `tools/spec-coverage.mjs` with its fixtures and tests (runner-agnostic, reading identifiers out of test names; implementation and test coverage reported **separately** so *implemented but untested* stays visible; blockquoted example identifiers skipped; non-zero exit on dead citations only; optional `specCoverage` config with real defaults) · `recipes/vite-ts` (`spec:coverage` wired in, the absence note deleted) · `.claude/skills/plan-goal` (checklist overlap sharpened to *can these be committed separately*) · **Backlog** (journey end-to-end coverage moved to `/sanity-check` with what it needs) |
| The document set is complete, and nothing has fallen between tech-spec and design-guide | 2026-08-25 | `doc/design-guide.md` (universal shape rules, each carrying its *when not to*; the bracketed module-header citation format `spec-coverage` depends on) · `doc/style-guide.md` (v1's scaffolding, empty, deleted by a project with no visual surface) · `doc/tech-spec.md` (**Architecture** for this project's own rules, the per-decision structure, *Resolving what domain-spec left open*, **Testing strategy**, and the design-guide boundary stated in both headers) · `doc/discovery/README.md` (freeze rule, dated naming, evidence per finding) · `tools/check-links.mjs` |
