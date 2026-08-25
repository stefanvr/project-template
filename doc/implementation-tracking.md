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

### Goal: a new project reaches a working, tested skeleton without re-solving the toolchain

**Discussion.** Promoted ahead of `spec:coverage` at that goal's own planning gate. The template
currently cannot start a project at all, which makes this the thinnest slice that reaches end to
end — and it creates the `package.json` the coverage script would have needed anyway, so building
it first removes a retrofit rather than adding one.

**It never runs in this repository.** A project is created by copying the template and scaffolding
*inside the copy*; scaffolding here would turn the template into an application. A `.dev-template`
marker file at the root makes that mechanical rather than a thing to remember: `/scaffold` refuses
to run beside it, and copying a new project deletes it. Verification uses a throwaway sibling
directory, deleted afterwards.

**Recipes, not a starter directory.** A recipe is a directory inside the skill holding a proven
setup; the skill picks one and adapts it, or goes freehand when none fits. Promoting a setup that
worked is then adding a folder, which is what makes this cheaper than v1's single `starter/`.

Two decisions carried in from the `spec:coverage` planning conversation, because this goal creates
the file they land in: the coverage script ships as `tools/spec-coverage.mjs` in the template, and
`/scaffold` adds only the npm script and the per-project paths.

**Open questions** — asked because the repository cannot answer them:

- **Which recipes ship first?** The two real projects point at two: a static or SPA site on GitHub
  Pages with `node:test` and Playwright, and a Vite/TypeScript app on Firebase Hosting with Vitest,
  Playwright and Firestore. Both, one, or neither to begin with?
- **Does `/scaffold` create the git repository and its remote?** We just lost time to a workflow
  that said *push before review* while no remote existed. Creating one needs `gh`, which is not
  installed on either side of this machine.
- **Does the skeleton deploy on day one?** Garden's first stage deployed before there was anything
  worth deploying, and it flushed out two failures — a service account with hosting rights only,
  and an unregistered default bucket — that were invisible to every local test. It also makes this
  goal much larger, and needs cloud accounts to exist.

**Proposal.** *Not written yet — waiting on the questions above.*

**Sign-off:** ☐


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

- [ ] **A rule with no test, and a test citing a rule that no longer exists, are both findable by
      running a command.** `tools/spec-coverage.mjs`, reading `DS-`/`IS-` identifiers.
      - **Settled at its planning gate, before it was deferred:** the tool is **runner-agnostic** —
        it reads identifiers out of test *names* and never parses a runner's output, because Q&C
        uses `node:test`, Garden uses Vitest, and both use Playwright. It **ships as a file in the
        template**, with `/scaffold` adding only the npm script and the per-project paths, since
        the logic is identical everywhere and only the paths are not.
      - **Carries a forward obligation from the journeys goal:** a journey whose named surfaces are
        never exercised together by any end-to-end test must be reported. That is the only coverage
        a journey has, since journeys deliberately carry no identifiers.
- [ ] **The documents can be audited against the code in one pass.** `/sanity-check`, which runs the
      coverage script and adds the judgement calls it cannot make.
      - **Carries a forward obligation from the domain-spec goal:** it must report **ceremonial
        stickies** — an event nothing subscribes to, a command whose only actor is "the system" with
        no policy triggering it, an event named noun-plus-*Updated*. These are the three mechanical
        symptoms of event storming being forced onto something that is not a workflow, which is the
        failure mode most likely to bite. Defined in [domain-spec.md](domain-spec.md)'s *When it
        does not fit*; nothing enforces it yet.
      - **And one from the journeys goal:** the backbone diagram and the journey steps must agree in
        both directions.
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
| The loop runs itself — a goal goes from backlog to merged without anyone remembering the workflow | 2026-08-25 | `workflow.md` (spec input asked at the planning gate · git performed on instruction, never on the skill's own judgement · promotion refused rather than prompted · WIP overrides recorded · `main` as integration branch, long-running branches rejected with reasons · §Work found mid-goal · landing needs its own branch) · `.claude/skills/plan-goal` and `.claude/skills/build-stage` (the loop itself; refusals that name the next command; in-progress distinguished from done-but-not-landed) · **Backlog** (SSH-not-HTTPS and git-inside-WSL, as a forward obligation on the `environment.md` goal) |
