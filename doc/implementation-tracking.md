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
### Goal: the document set is complete, and nothing has fallen between tech-spec and design-guide

**Discussion.** This started as *"write the two missing documents"* and is bigger than that. Three
things fell through, not one.

`design-guide.md` and `style-guide.md` are referenced **18 times across 13 files** — including
`README.md`'s document table, `CLAUDE.md`'s decision routing, `build-stage`'s promotion table and a
module comment in the recipe's own test — and neither file exists. Every one of those links is dead,
and has been for six goals.

Worse, v2's `tech-spec.md` pushed **Architecture** out to `design-guide` and then dropped two more
sections v1 had: **Testing strategy**, and **Resolving what domain-spec left open**. That last one
is genuinely valuable and entirely absent — it is where a technical decision records that the domain
specification deliberately left a gap, which otherwise gets settled implicitly by the first module
that needs it and copied by every module after.

**The boundary, worked out from a real project rather than invented.** Garden's `tech-spec`
Architecture section mixes two kinds of rule that v2 should separate:

| Kind | Example from Garden | Home in v2 |
|---|---|---|
| Universal shape | *the domain layer never imports Firebase; persistence is reached through a repository interface* | `design-guide.md` |
| This project's rule | *months are integers 1–12 internally, with exactly one mapping table* | `tech-spec.md` |

So `design-guide` holds what would still be true on a completely different project, and `tech-spec`
holds this project's application of it. The same test the whole document set already uses.

**Settled by reading rather than asking:**

- `design-guide` ships **filled in**, like `workflow.md`, not as a stub like the two specifications.
  Its content is a standing preference, not a per-project decision — and `style-guide` shipping as a
  template confirms the contrast.
- It must specify the **citation format** for module headers. `tools/spec-coverage.mjs` reads
  `[DS-n.n]` in brackets out of source files, and nothing currently tells an author to write it that
  way — the tool has a dependency no document states.
- Testing splits across both: *which layer carries the bulk of coverage* is a technical choice
  (`tech-spec`), while *tests mirror the source layout and are named as the behaviour claimed* is a
  code convention (`design-guide`).
- v1's `code-conventions.md` content all belongs here, reframed: module-cites-its-rule, comments
  explaining why-not-the-obvious, seeded randomness with deterministic tie-breaks, dev-only
  affordances built and gated, and scratch work leaving no trace.

**Open questions** — the repository cannot answer these:

- **How hard should `design-guide` push CQRS and ports-and-adapters?** They are your standing
  preferences and they earned their place in both real projects. But this template will also be used
  for something small, where a repository interface over one JSON file is ceremony. Mandate them, or
  state them as the default with the conditions under which not doing it is correct?
- **How thin is "merely a template" for `style-guide`?** v1's was 98 lines of token tables,
  component states and fog-of-war examples. Keep that scaffolding for a project to fill in, or strip
  it to a header and a couple of prompts?

**Proposal.** *Not written yet — waiting on the questions above.*

**Sign-off:** ☐

---

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

- [ ] **The documents can be audited against the code in one pass.** `/sanity-check`, which runs the
      coverage script and adds the judgement calls it cannot make.
      - **Carries a forward obligation from the domain-spec goal:** it must report **ceremonial
        stickies** — an event nothing subscribes to, a command whose only actor is "the system" with
        no policy triggering it, an event named noun-plus-*Updated*. These are the three mechanical
        symptoms of event storming being forced onto something that is not a workflow, which is the
        failure mode most likely to bite. Defined in [domain-spec.md](domain-spec.md)'s *When it
        does not fit*; nothing enforces it yet.
      - **And two from the journeys goal:** the backbone diagram and the journey steps must agree
        in both directions; and a journey whose named surfaces are never exercised together by any
        end-to-end test must be reported. The second is the only coverage a journey has, since
        journeys deliberately carry no identifiers — it needs `*Surfaces: §n …*` resolved to `IS-`
        identifiers, then an end-to-end file citing all of them together. Moved here from the
        coverage goal, which kept itself to the mechanical check.
- [ ] **The remaining supporting documents exist in v2 shape.** `style-guide` and `design-guide`,
      each reduced to its Owns/Not here header plus what a real project fills in. `tech-spec` and
      `environment` moved into the scaffold goal, which writes into them — and the SSH-and-WSL
      forward obligation was discharged there.
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
