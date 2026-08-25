# Dev template v2

A starting skeleton for a new project: a small set of documents that each own one kind of
decision, a set of skills that produce and maintain them, and a development routine that keeps
all of it honest as the build progresses.

Distilled from two real builds rather than designed in the abstract. Most of what is here exists
because its absence caused a specific problem, and the note explaining *why* a rule exists is
usually worth more than the rule.

## What changed from v1

v1 was a good set of documents with the authoring instructions written into the top of each one.
That had two consequences, both of which showed up in real use:

- **Instructions in a document get read once and then ignored.** They are furniture by the third
  session. Moved into skills, they are loaded exactly when the work happens.
- **Reasoning had nowhere to live, so it leaked into the nearest specification.** One project's
  domain spec became the research that should have produced it; the other's tracking document
  became a transcript of the discussions that defined its tasks. Both documents stopped being
  usable as the thing they claimed to be.

v2 fixes the second by giving reasoning a home with a lifecycle — discovery artifacts as frozen
inputs, and a promotion rule that moves what was learned into the document that owns it before a
task is ticked off.

## Starting a project

**Copy** this repository to a new directory — copy, do not clone; a new project does not want the
template's own history. Delete `.dev-template`, clear `doc/implementation-tracking.md` down to its
rules, delete the illustrative blocks in the two specifications, then run `/scaffold`.

`/scaffold` refuses to run beside `.dev-template`, so scaffolding the template in place — which
would turn it into an application — is impossible rather than merely discouraged.

## The documents

| Doc | Owns | Shape |
|---|---|---|
| [doc/domain-spec.md](doc/domain-spec.md) | What the thing *is* | Event flow, then commands, events and rules per aggregate |
| [doc/implementation-spec.md](doc/implementation-spec.md) | How it is operated | Journeys, which index into per-surface detail |
| [doc/tech-spec.md](doc/tech-spec.md) | What it is built with | Choices, with rejected alternatives and accepted risk |
| [doc/design-guide.md](doc/design-guide.md) | How the code is shaped | SRP, ports and adapters, CQRS, and the module-to-rule binding |
| [doc/style-guide.md](doc/style-guide.md) | How it looks | Tokens, components, visual states |
| [doc/environment.md](doc/environment.md) | The machine | How to run it, and what fails silently when it is wrong |
| [doc/workflow.md](doc/workflow.md) | How work gets done | The goal loop, the review gates, the promotion rule |
| [doc/implementation-tracking.md](doc/implementation-tracking.md) | What is being built now | Goals, in Kanban states, with a WIP limit |
| [doc/discovery/](doc/discovery/) | Where the thinking happened | Frozen input artifacts; never the source of truth |

Each document carries a two-line **Owns / Not here** header. That header is the point of the whole
set: it stops the documents collapsing into one undifferentiated pile where nothing can be found
and everything is restated three times.

## The skills

The documents describe *what is true*. The skills describe *how to get there*, and they are where
v1's preamble instructions went.

| Skill | Produces |
|---|---|
| `/discover` | A frozen input artifact in `doc/discovery/` — a brainstorm, a data analysis, a look at a previous attempt |
| `/event-storm` | `domain-spec.md`, iteratively — one area at a time, not one sitting |
| `/story-map` | `implementation-spec.md`'s journeys, and the goal backlog that follows from them |
| `/plan-goal` | One goal taken from backlog through discussion, open questions, proposal, and sign-off |
| `/build-stage` | The implementation loop for the goal in **Now**, including promotion before ticking |
| `/sanity-check` | An audit of every document against the actual application, plus rule-to-test coverage |
| `/scaffold` | The toolchain, and the `tech-spec` and `environment` entries that record it |

## The two rules that make this work

**Write standing agreements into the documents, not into a chat.** A preference expressed in
conversation is invisible to the next session, the next tool, and the next person. If it will
matter again, it belongs in whichever document owns that kind of decision — and the set above is
arranged so there is always exactly one obvious home for it. An assistant's private memory is a
fine fast-recall convenience alongside that, but must never be the only record of something a
human contributor would need.

**Nothing is ticked off until what it taught has been promoted.** Tracking text is scaffolding and
is deleted when a goal lands. That deletion is only safe because anything durable has already been
moved into the document that owns it. See [doc/workflow.md](doc/workflow.md).
