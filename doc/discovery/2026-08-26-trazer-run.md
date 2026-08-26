# Trazer build — findings on dev-template v2

> **Input artifact, frozen 2026-08-26.** Not maintained. Where this disagrees with the
> specifications or the skills, they are right. Kept for the reasoning and the evidence, not the
> rules.

**Source:** the first greenfield build on template v2 — `github.com/stefanvr/trazer`, `main` at
`b5edc45`. Written by the Trazer session, which had used the template end to end.
**Prompted by:** the backlog goal *"The template is proven by starting a real project with it."*

**Why it is kept.** Every finding below carries a template path, the Trazer commit that evidences
it, and a suggested fix. Once the fixes land, the fixes themselves are the truth and this becomes
what it already is — the record of how they were found. Section D matters as much as the defects:
eleven mechanisms that did real work, which is the half a defect list never shows.

---

**Context.** First greenfield use of template v2, per its own backlog item *"The template is proven
by starting a real project with it."* Project: Trazer, a Traz/Arkanoid brick-breaker.
**Source repo:** `github.com/stefanvr/trazer` · `main` @ `b5edc45` · landing branch
`goal/land-walking-skeleton` @ `39c863a` (pushed, unmerged at time of writing).

**Skills exercised:** `/scaffold`, `/discover`, `/event-storm`, `/story-map`, `/plan-goal`,
`/build-stage` — full loop, one goal planned, built, merged, landed.
**Not exercised:** `/sanity-check` — see **A1**.

All commit references are Trazer commits; the fix targets are paths in the **template** repo.

---

## A. Template defects

### A1 — `/sanity-check` is referenced everywhere and does not exist · severity: high

`.claude/skills/` contains exactly six skills: `build-stage`, `discover`, `event-storm`,
`plan-goal`, `scaffold`, `story-map`. `sanity-check` is absent, yet referenced in **7 shipped
files**:

`CLAUDE.md` (*Which skill* table) · `README.md` (skills table) · `doc/workflow.md` ·
`doc/domain-spec.md` (*When it does not fit* — promises ceremonial-sticky reporting) ·
`doc/implementation-spec.md` (promises backbone/journey bidirectional check) ·
`doc/implementation-tracking.md` (*Release gap* — "Maintained by `/sanity-check`") ·
`.claude/skills/story-map/SKILL.md` · `.claude/skills/build-stage/SKILL.md`.

The template's own tracking listed it as an unbuilt backlog goal, so the routing table ships
promising a command that is not there. Four separate forward obligations are attached to it.

**Fix:** build it, or mark every reference as *not yet built*. The `CLAUDE.md` routing table is the
worst offender — it is the first thing every session reads.

### A2 — `implementation-spec.md` ships example content outside the delete-block · severity: medium

`.dev-template` step 4 says to delete the *"⚠️ Illustrative example — delete this whole block"*
quotes from `domain-spec.md` and `implementation-spec.md`, and *"Delete nothing else."* But
`implementation-spec.md`'s **Backbone** mermaid diagram carries library-domain example content
(`Find something → Take it out → Get it back`, `search`/`renew`/`return`) and is **not** inside a
delete-block. A correctly-followed copy sequence leaves a library story map in a game project.

**Fix:** wrap the mermaid example in the delete-block, or name it explicitly in `.dev-template`
step 4. Same audit should be run for `design-guide.md` and `style-guide.md`.
**Evidence:** survived the full copy sequence; replaced only at `5b70132`.

### A3 — `design-guide.md` §Tests omits the bracket requirement · severity: high

§*Tests mirror the source layout* says *"lead with its identifier"*. `tools/spec-coverage.mjs`
matches `/\[((?:DS|IS)-\d+\.\d+)\]/` — **brackets are mandatory**. The module-header section states
this; the tests section does not repeat it.

Consequence measured: **16 rules reported "implemented but NOT tested" while tests for them
existed**, because `describe("DS-1.9 — …")` is invisible to the tool and `describe("[DS-1.9] — …")`
is not. On a young project this is indistinguishable from genuine absence.

Second, unstated hazard: `spec-coverage.mjs` `collect()` scans **whole files**, not test names. A
bracketed identifier in a passing comment inside a test body counts as coverage it does not give.
Over-citing yields a green report nobody can trust.

**Fix:** add both clauses to `doc/design-guide.md` §Tests.
**Evidence:** `949381e` (fix + promotion). Reference wording is in Trazer's `doc/design-guide.md`.

### A4 — `plan-goal` checklist test needs a reachability clause · severity: medium

`SKILL.md` §Proposal: *"the test is **can these be committed separately**"*, with two recorded
prior failures. **This is the third.** Two checklist items — *the map screen* and *the stub level* —
compile apart but are not reachable apart: the map is only reached by clearing a level. Either
commit alone leaves an application that renders nothing, so no reviewer can exercise it.

**Fix:** extend the test in `.claude/skills/plan-goal/SKILL.md` §Proposal and `doc/workflow.md` §4
to ask about **reachability**, not just separability: *after this item and before the next, what can
someone actually do?*
**Evidence:** `5a12ef8`. Reference wording is in Trazer's `doc/workflow.md` §4.

### A5 — nothing checks rules against journeys and goals · severity: medium

`spec:coverage` compares declared rules against **code and tests** only. `DS-1.13` (abort) was
declared, carried by no journey and claimed by no goal, and was invisible to every tool. It was
found only by a manual cross-check of `DS-` citations between `domain-spec.md`,
`implementation-spec.md` and `implementation-tracking.md`.

A rule no goal claims is invisible right up until the release it is missing from.

**Fix:** add the check to `/sanity-check` (blocked on **A1**), or extend `spec-coverage.mjs` with a
`goals`/`journeys` glob.
**Evidence:** `5b70132` (found), `3e2cffb` (adopted into a goal), `f4bbf11` (built).

### A6 — *Release gap* is defined in the less dangerous direction · severity: low

`implementation-tracking.md` defines it as *domain areas with no goal*. The gap that actually bit
was the inverse: **a backbone activity with no domain area at all**. Trazer's largest activity,
*Play a level*, has no `domain-spec.md` section, so it declares zero identifiers and therefore
cannot appear as uncovered anything. A section that does not exist has no identifiers to go missing.

**Fix:** state both directions in the section header, and give the inverse check to
`/sanity-check`.
**Evidence:** `5b70132`.

---

## B. Recipe defects — `.claude/skills/scaffold/recipes/vite-ts/`

### B1 — shipped tooling tests run nowhere · severity: high

`package.json` → `"test": "vitest run"`; `vitest.config.ts` → `include: ["test/**/*.test.ts"]`.
`tools/spec-coverage.test.mjs` is Node's built-in runner (`node --test`) over `.mjs`, so it matches
neither. It executes **neither locally nor in CI**, despite `.dev-template` insisting the file ships
precisely so *"a project that adjusts the `specCoverage` globs needs the tests that catch it
breaking them."* Seven passing tests nobody runs.

**Fix (verified working):**
```json
"test": "npm run test:unit && npm run test:tools",
"test:unit": "vitest run",
"test:tools": "node --test \"tools/**/*.test.mjs\"",
```
The CI workflow already calls `npm test` and needs no change.
**Evidence:** `27cc9cf`.

### B2 — `@types/node` should track the Node major, not its own latest · severity: medium

Recipe ships `@types/node: ^22.7.0` against `.nvmrc` = `24`. `RECIPE.md` correctly tells `/scaffold`
to check current majors, which if followed naively installs `@types/node@26` — typechecking against
a runtime the project does not run.

**Fix:** add to `RECIPE.md` §Versions: *`@types/node` majors track Node majors; pin it to `.nvmrc`,
not to latest.* Bumping `.nvmrc` then means bumping this in the same commit.
**Evidence:** `c17b0ff`.

### B3 — recipe pins were 2–3 majors stale · severity: low (process worked)

Measured at scaffold time: vite `^5.4`→`8.2.2`, vitest `^2.1`→`4.1.11`, typescript `^5.6`→`7.0.2`,
playwright `^1.48`→`1.62.1`. `RECIPE.md`'s *"a recipe that pins the past is how a template starts
costing more than it saves"* did its job — flagged as **working as intended**, recorded only as a
freshness datum. TypeScript 7 (native port) typechecked clean on the recipe source, which exercises
`verbatimModuleSyntax`, `isolatedModules` and ambient `declare const`.

---

## C. Environment knowledge to back-port to `doc/environment.md`

The template ships silent failures 1–5. Three additions, all observed:

| # | Finding | Evidence |
|---|---|---|
| **C1** | **Silent failure 6.** `npm run build` outside a git repository exits 0 and produces a complete `dist/` whose build identifier reads `unknown`. The e2e suite is the only thing that catches it — which is why it must assert *not* `unknown` rather than *present*. Ordering: `git init` + first commit precede any build whose output is trusted. | `c17b0ff` |
| **C2** | **The Pages REST API is not a check.** `GET /repos/{o}/{r}/pages` returns `404` unauthenticated *even for a public repo with Pages live*. It reports *not enabled* and *no permission to ask* identically. Verify by fetching the site and comparing its SHA to `main`. | `034b90b` |
| **C3** | **`git ls-remote … \| head` then `echo $?` reports `head`'s status.** A missing repository reads as success. Redirect to a file and test the code directly. | `3b32e0c` |

**Silent failure 4 reproduced.** Apostrophes in a quoted heredoc inside a `bash -c` command string:
error was `line 127: unexpected EOF while looking for matching '`, where line 127 was the end of the
document and the cause was *project's* / *recipe's* far above. A quoted heredoc does **not** save
you — the outer `bash -c` parses first. The documented remedy (use a file-writing tool) is the only
thing that works. Recorded as additional evidence at `c17b0ff`.

**Silent failure 1 did not fire** — nvm default alias was already 24, matching `.nvmrc`.

---

## D. Confirmed working — do not change

| Mechanism | Evidence |
|---|---|
| **`.dev-template` guard** | Fired correctly on first `/scaffold`; pointed at the file rather than reciting the sequence; never offered deleting the marker as the way past. |
| **Install-before-first-commit** | Lockfile committed in the scaffold commit; CI `build` job green on run 1, every step. `c17b0ff` |
| **Deploy-on-day-one** | Run 1: `build` green, `deploy` red at `actions/deploy-pages` — the legible failure for Pages-not-enabled. Runs 2–3 green after the manual setting. `3b32e0c`, `034b90b` |
| **Build identifier** | Live site SHA `b5edc45` == `main` `b5edc45`, twice confirmed. This is the mechanism that makes a deploy *checked* rather than believed. |
| **`/discover` freeze rule** | The source draft was a specification with no derivation (one sentence of reasoning in 198 lines). Freezing it surfaced **3 author-unnoticed contradictions** and kept them out of `domain-spec.md`. `cc704bd` |
| **`domain-spec.md` *When it does not fit*** | The adversarial case held. Level unlocking was written as a **DERIVATION**, not a `Level unlocked` event — caught by the ceremonial-sticky test (*nothing subscribes to it*). `0ba964f` |
| **`/event-storm` one-area-per-session** | Produced 16 rules, 5 commands, 5 policies, 2 derivations, 1 read, 4 open items — reviewable in one sitting. `0ba964f`, `68af249` |
| **`/plan-goal` gate** | Caught a blocking domain gap (`DS-1.9` scoped *"In Journey"* with no Arcade equivalent), the user's own feature creep (mode chooser that would change nothing), and an unowned rule (`DS-1.13`). `3e2cffb` |
| **`/build-stage` promote-before-tick** | 5 checklist items → **5 promotions**, none of which would have survived the goal-text deletion. Refusal-not-prompt is doing real work. |
| **Deletion rule at landing** | Re-reading the goal text before deleting found **2 items not durable anywhere else**; both moved to Backlog first. The rule is only safe because this step is actually performed. `39c863a` |

---

## E. Pattern worth adding to `/story-map` or `implementation-spec.md`

**Split a placeholder surface into durable contract and disposable rendering.** §2 *The arena* was
specified as `IS-2.1`–`IS-2.3` (the level's boundary: reports exactly one of two outcomes; names
itself and its lives; a lost life is a continuation) plus `IS-2.4` alone marked
*"Retired when the arena is built."*

Because identifiers are never renumbered or reused, the arena goal retires **one** behaviour rather
than four, and `IS-2.1` becomes the interface the real arena implements. The naive alternative —
specifying the placeholder as four throwaway behaviours — burns four identifiers and loses the
contract.
**Evidence:** `adf6761`.

---

## F. Open

- **A1 blocks A5 and A6**, both of which are `/sanity-check` obligations.
- Trazer's `domain-spec.md` carries open items `[H2]`, `[H3]`, `[H4]`; `[H1]` settled at `68af249`.
- `doc/implementation-spec.md` *Play a level* cites almost nothing — the arena is unstormed, and the
  next backlog goal is explicitly blocked on `/event-storm`. Not a template defect; the template
  correctly surfaced it.
