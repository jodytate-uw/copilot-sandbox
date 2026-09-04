---
applyTo: '**'
---

# Copilot instructions for copilot-sandbox

## Commit messages

Every commit MUST follow this format:

```
type(scope): short imperative summary

- Bullet describing what changed and why
- Additional bullets as needed

GitHub-Copilot: true
LLM-Model: <exact model name, e.g. Claude Opus 5>
```

Rules:

- Subject line uses [Conventional Commits](https://www.conventionalcommits.org/) types: `feat`, `fix`, `docs`, `refactor`, `build`, `chore`, `test`, `style`, `perf`, `ci`. Scope is optional but preferred.
- The `GitHub-Copilot` and `LLM-Model` trailers are required on every commit for AI transparency. `GitHub-Copilot` is `true` when Copilot contributed to the change and `false` when it did not. Set `LLM-Model` to the model that actually produced the change, or `None` for a hand-written commit.
- Pass multi-line messages with repeated `-m` flags so the trailers land in the commit footer.
- A `prepare-commit-msg` hook in `.githooks/` backfills these trailers if they are missing, but always write them explicitly rather than relying on the fallback values.

Enable the hook once per clone:

```pwsh
git config core.hooksPath .githooks
git config commit.githubCopilot true
git config commit.llmModel "Claude Opus 5"
```

## Before committing

Run `git log -5` when unfamiliar with the repo to confirm current message conventions before writing a commit.
