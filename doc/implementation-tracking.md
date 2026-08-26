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

*Empty. Nothing is in progress. `/plan-goal` takes the next goal from Backlog.*

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

- [ ] **The defects the Trazer run found are fixed.** Nine, all decided and none needing discussion
      — the proportional-gate case. `A2` example content outside its delete-block, so a correctly
      followed copy sequence leaves a library story map in a game project (audit `design-guide` and
      `style-guide` the same way) · `A3` the bracket requirement missing from `design-guide` §Tests,
      which measurably cost **16 rules falsely reported untested**, plus its unstated inverse —
      `spec-coverage` scans whole files, so a bracketed identifier in a stray comment inflates
      coverage, and a green report nobody trusts is worse than the red one it replaced · `A4` the
      reachability clause, the **third** recorded instance of the same `plan-goal` failure · `B1` the
      recipe's shipped tooling tests executing nowhere, contradicting `.dev-template`'s stated reason
      for shipping them · `B2` `@types/node` tracking the Node major rather than its own latest ·
      `C1`–`C3` environment additions · `E` the placeholder-surface pattern.
      - **Fix text already exists and is quotable verbatim:** A3 from Trazer's `doc/design-guide.md`
        §Tests, A4 from its `doc/workflow.md` §4, B1 as a verified three-line `package.json` change.
        Trazer's `doc/environment.md` carries C1–C3 written out, +66 lines. No skill was tweaked
        there and all three tools are byte-identical, so nothing else needs back-porting.
- [ ] **Modelling work has branch discipline.** `workflow.md` defines `goal/{slug}` for building and
      says nothing about discovery, storming or mapping — so the Trazer run did all three on `main`.
      The shape that run suggests: after scaffolding, `/discover`, `/event-storm` and `/story-map`
      share one branch, and `/plan-goal` plus `/build-stage` then run on the goal branch as now.
- [ ] **Storming and mapping interleave rather than complete.** `/event-storm` pushes to cover more
      of the domain before `/story-map` is offered anything, where the two are an iterative pair:
      storm one area, map it, build a slice, come back. An unfinished domain is not a reason to keep
      storming, and the skills should hand over early rather than treating completeness as the gate.
- [ ] **A stand-alone step for changing how something looks and feels.** Twice now — Q&C and Trazer —
      the UX arrived different from what was intended: it brought genuinely new ideas, but not the
      ones in mind, and there is no route to revisit it without replanning a goal. Needs real design
      before it can be planned: what it reads, what it produces, whether it edits
      `implementation-spec` surfaces directly or proposes changes, and how it relates to
      `style-guide`.
- [ ] **The documents can be audited against the code in one pass.** `/sanity-check`, which runs the
      coverage script and adds the judgement calls it cannot make.
      - **Carries a forward obligation from the domain-spec goal:** it must report **ceremonial
        stickies** — an event nothing subscribes to, a command whose only actor is "the system" with
        no policy triggering it, an event named noun-plus-*Updated*. These are the three mechanical
        symptoms of event storming being forced onto something that is not a workflow, which is the
        failure mode most likely to bite. Defined in [domain-spec.md](domain-spec.md)'s *When it
        does not fit*; nothing enforces it yet.
      - **A1, severity high, from the Trazer run:** it is referenced in **seven shipped files**,
        including the `CLAUDE.md` routing table that every session reads first. The template ships
        promising a command that is not there, and four forward obligations hang off it.
      - **A5 — nothing checks rules against journeys and goals.** `spec:coverage` compares declared
        rules against code and tests only. A rule carried by no journey and claimed by no goal is
        invisible to every tool, right up until the release it is missing from. Found by hand.
      - **A6 — *Release gap* is defined in the less dangerous direction.** It reads *domain areas
        with no goal*; the gap that actually bit was the inverse — a backbone activity with **no
        domain area at all**, which declares no identifiers and so cannot appear as uncovered
        anything. State both directions.
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
| A project copied from the template starts clean, and the copy sequence is stated once | 2026-08-25 | `.dev-template` — now the single source: `README.md` named in the sequence, `tools/` kept in full with its tests and fixtures, and what *not* to delete · `README.md` and `.claude/skills/scaffold` both point at it rather than restating a list that had already drifted in all three places at once · `doc/workflow.md` and `.claude/skills/plan-goal` (the planning gate is proportional — a goal with no open questions is planned in one message, but still gets its outcome, its **Try it** and its promotion) |
| The template is proven by starting a real project with it | 2026-08-26 | `doc/discovery/2026-08-26-trazer-run.md` — frozen findings from the Trazer build: 6 template defects, 3 recipe defects, 3 environment additions, **11 mechanisms confirmed working**, and 1 pattern worth adopting · the defects became the goals now in **Backlog** · **the adversarial case held**: level unlocking came out as a `DERIVATION` rather than an invented `Level unlocked` event, caught by the ceremonial-sticky test, which is the worry that started v2 |
