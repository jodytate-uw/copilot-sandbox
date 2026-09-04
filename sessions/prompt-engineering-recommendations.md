# Making Prompt Engineering Sessions More Efficient

Based on a review of Copilot sessions in this repo from August 28 through September 4, 2026 — six sessions, roughly 90 turns, spanning repo setup, an 11ty static site, an ORIS people directory and activity dashboard, and commit-convention enforcement.

## Recommendations

- **Encode conventions in the repo the moment you define them, not in prose afterward.** The commit trailer format was agreed on August 28 and recorded only in a session log. On September 4 it was missed on two commits because nothing discoverable enforced it. *Implication:* conventions become self-enforcing rather than dependent on the assistant reading the right file or you catching the lapse in review. A `.github/copilot-instructions.md` file plus a git hook costs minutes once and eliminates a recurring class of error.

- **State whether you want an answer or an action.** "Is it possible to encode these conventions?" produced both an explanation and an unrequested implementation. Prefixing with "Question only —" or "Implement:" removes the guess. *Implication:* you stop spending turns undoing work you did not ask for, and you avoid unwanted files entering the working tree.

- **Scope git commands explicitly.** "git commit and push" was interpreted as `git add -A`, sweeping a whole directory deletion and unrelated new code into one commit. "Commit only `path/to/file`" is unambiguous. *Implication:* commits stay atomic and reviewable, and history does not need repair later — which matters more once you have decided not to rewrite published commits.

- **Front-load environment prerequisites.** Roughly ten turns in the August 28 session went to Node.js PATH problems, PowerShell execution policy, and a Chocolatey detour; a later session stalled because the GitHub CLI was not installed. *Implication:* a short "check that node, npm, and gh are available before starting" instruction converts a mid-task derailment into a thirty-second precondition check, keeping the session on its actual subject.

- **Supply data the assistant cannot reach, up front.** The ORIS member list is private; two turns were spent discovering that before you provided the JSON export. *Implication:* the assistant cannot know in advance what is behind authentication. Attaching the data with the initial request removes a full discovery round-trip.

- **Decide naming and format criteria before asking for artifacts.** The log directory went `diary_entries` → `log` → `sessions`, and the timestamp format took three refinement turns. *Implication:* stating the constraints first ("sortable, no 24h time, month first") gets the right answer in one turn instead of four, and avoids renaming files already committed.

- **Put durable preferences into instruction files rather than repeating them.** "Respond with succinct answers" and "this LLM seems over-eager" were mid-session corrections that do not persist to the next session. *Implication:* preferences expressed once in `.github/copilot-instructions.md` apply to every future session in the repo automatically, so you stop re-training each conversation from scratch.

- **Keep using `user_prompts/` files, and make them more specific.** The file-based prompts in this repo are already your most effective pattern — they are reusable, reviewable, and versioned. The strongest example specified format, word count, and required sections. *Implication:* a prompt with explicit deliverable, format, and acceptance criteria produces usable output on the first pass; a vague one produces a draft you then have to steer.

- **Split sessions by topic.** The August 28 session ran 57 turns across git setup, model selection research, documentation standards, framework evaluation, and Node.js troubleshooting. *Implication:* shorter, single-purpose sessions keep relevant context dense, make the session log genuinely reviewable afterward, and reduce the chance the assistant acts on stale intent from forty turns earlier.

- **Separate research turns from build turns.** Questions like "what are best practices for commit messages" and "provide two options for static site frameworks" are decision inputs. Answering them, deciding, and *then* opening a build session prevents the assistant from starting to implement an option you have not chosen. *Implication:* fewer half-built artifacts, and the decision is recorded before code depends on it.

- **Ask for a plan before non-trivial builds.** The dashboard was built in one pass; a one-paragraph plan first would have surfaced the API rate-limit and authentication constraints before code existed. *Implication:* you catch design problems when they cost a sentence to fix rather than a file to rewrite.

- **Say what "done" looks like.** "Keep the first pass minimal — something to build on" was an effective constraint and produced an appropriately small dashboard. *Implication:* naming the intended maturity level prevents both over-engineering and an under-built result, and it is a single clause.

- **Review the working tree before ending a session.** Several artifacts — a stash, an unpushed commit, an uncommitted session entry — accumulated across this week. *Implication:* a closing `git status` and stash check means the next session starts from a known state instead of inheriting ambiguity about what was intentional.

## Summary

The pattern across these sessions is that most lost time came not from the assistant's capability but from **unstated context**: conventions held in memory rather than in files, intent that was ambiguous between question and instruction, git commands whose scope was assumed, and environment prerequisites discovered mid-task.

The recommendations reduce to three moves. **Persist what recurs** — conventions, preferences, and constraints belong in versioned instruction files and hooks, where they apply automatically. **Be explicit about scope and intent** — say whether you want an answer or an action, and name the exact paths a git command should touch. **Separate the phases** — research, decide, plan, then build, in distinct sessions.

Following them should compress the correction-and-rework turns that currently make up a meaningful share of each session, and shift your effort from steering the assistant back on course toward the experimentation this sandbox exists for. The secondary benefit is that the session logs become a cleaner record: when each session has one subject and a defined outcome, the log is a useful reference rather than a transcript.
