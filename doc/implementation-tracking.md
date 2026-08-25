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

### Goal: a new project reaches a working, tested skeleton without re-solving the toolchain

**Outcome:** copying this template and running `/scaffold` produces a project that builds, tests and
deploys, with its `tech-spec.md` and `environment.md` written as the setup happens rather than
reconstructed afterwards.

**Try it:** copy the template to a throwaway sibling directory, run `/scaffold`, answer the
interview. Afterwards `npm test` and `npm run test:e2e` pass, `git log` shows the scaffold commit,
and once pushed the live URL shows a build identifier matching `main`. Then delete the directory and
confirm this repository is untouched.

**Covers:** none. This goal is toolchain, not product behaviour, so it implements no `DS-` or `IS-`
identifier. The **Covers** field not fitting a meta-goal is a small format finding, to be promoted
if it recurs.

**Signed off** 2026-08-25. Input given at the gate: the first recipe deploys to **GitHub Pages** —
runnable today with no account to provision and no billing, with the Firebase shape arriving as
recipe two, promoted from a real project rather than pre-built.

- [x] `.dev-template` marker at the repository root → the file itself, plus a **Starting a project**
      section in `README.md`. The guard needed somewhere to send you, or refusing is just a wall
- [x] `doc/tech-spec.md` and `doc/environment.md` in v2 shape → both documents, and the
      supporting-documents backlog item narrowed to what is left of it. `environment.md` discharges
      the SSH-and-WSL forward obligation, and gained a fourth silent failure found while writing it:
      heredocs and apostrophes nested inside a shell command string break with an error pointing
      somewhere unrelated
- [x] `.claude/skills/scaffold/SKILL.md` — the interview, recipe selection, the guard, git with
      a verified remote, and deploy-on-day-one. *Nothing new to promote:* every rule in it was
      already settled in `workflow.md` or `environment.md`, which is what a skill carrying
      procedure rather than rationale should look like
- [x] `recipes/vite-ts/` with `RECIPE.md` → the recipe carries its own reasoning, which is what a
      recipe is for. Verified by scaffolding it: install clean, `typecheck` clean, 3 unit tests and
      6 end-to-end tests passing at both viewports. The vitest/vite config split is deliberate and
      load-bearing — unit tests must see the identifier *absent* so the `unknown` fallback is the
      path under test rather than dead code
- [x] Deploy pipeline: typecheck, unit and end-to-end tests all run **before** anything publishes,
      so a red build cannot reach the live site. Verified locally that the Pages subpath build is
      right — assets resolve under `/{repo}/`, which is the failure that 404s every script on a
      site that otherwise looks fine — and that the identifier is injected into the bundle
- [x] Git: `init`, the recipe's `.gitignore`, first commit, then the guided-and-verified remote →
      the skill half landed with item 3, the `.gitignore` with the recipe
- [x] `environment.md` written *as the console steps are done* → the rule landed inside
      `/scaffold` with item 3, rather than needing a change of its own. **Planning overlap worth
      noting:** items 3, 6 and 7 all describe one artifact, so the checklist counted the skill
      three times. Item 6 is genuinely still open, but only for the `.gitignore` the recipe
      carries — its skill half is written
- [x] Verified by scaffolding it for real → the recipe, `/scaffold`, `workflow.md`. Locally:
      install, typecheck, 3 unit tests, 6 end-to-end tests at both viewports, the Pages subpath
      build. Then on a real repository: CI green through `npm ci`, typecheck, unit tests and
      end-to-end, and the live page reading `95af1e7` — matching `origin/main` exactly. The deploy
      **confirmed rather than assumed**, which is the entire reason the identifier exists
- [x] **Ad hoc:** three defects found by the first real CI runs, none of them visible to any local
      test → the recipe and `/scaffold`. No committed lockfile, so `setup-node` failed on its
      dependency cache before a single test ran; `checkout` and `setup-node` at v4 being forced onto
      Node 24 by the runner; and `configure-pages`'s `enablement: true` failing with *Resource not
      accessible by integration*, which reverted a change made an hour earlier that had been
      reasoned about rather than observed. That reason is written **inside the workflow file**, so
      the next person reading the action's documentation does not re-add it in good faith
- [x] **Ad hoc:** `.nvmrc` moved off the end-of-life Node 20 to 24 LTS → the recipe and `RECIPE.md`,
      which records that each machine needs `nvm install 24` first. Verified by the pipeline itself
      rather than a manual retest, since CI installs from `.nvmrc`
- [x] **Ad hoc:** *suspect the scratch script before the system* → `workflow.md` step 5. A
      verification script here reported a deploy mismatch that did not exist, because it searched
      for a string the bundle only assembles at runtime
- [x] **Promoted earlier from the same goal:** `environment.md` silent failure 5 — a variable
      crossing the WSL boundary arriving empty, turning `cp -r $R/.` into `cp -r /.`

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
| The loop runs itself — a goal goes from backlog to merged without anyone remembering the workflow | 2026-08-25 | `workflow.md` (spec input asked at the planning gate · git performed on instruction, never on the skill's own judgement · promotion refused rather than prompted · WIP overrides recorded · `main` as integration branch, long-running branches rejected with reasons · §Work found mid-goal · landing needs its own branch) · `.claude/skills/plan-goal` and `.claude/skills/build-stage` (the loop itself; refusals that name the next command; in-progress distinguished from done-but-not-landed) · **Backlog** (SSH-not-HTTPS and git-inside-WSL, as a forward obligation on the `environment.md` goal) |
