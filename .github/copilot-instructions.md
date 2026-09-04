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

Coding-Assistant: GitHub Copilot
LLM-Model: <exact model name, e.g. Claude Opus 5>
```

Rules:

- Subject line uses [Conventional Commits](https://www.conventionalcommits.org/) types: `feat`, `fix`, `docs`, `refactor`, `build`, `chore`, `test`, `style`, `perf`, `ci`. Scope is optional but preferred.
- The `Coding-Assistant` and `LLM-Model` trailers are required on every commit for AI transparency. Set `LLM-Model` to the model that actually produced the change.
- Pass multi-line messages with repeated `-m` flags so the trailers land in the commit footer.
- A `prepare-commit-msg` hook in `.githooks/` backfills these trailers if they are missing, but always write them explicitly rather than relying on the fallback values.

Enable the hook once per clone:

```pwsh
git config core.hooksPath .githooks
git config commit.codingAssistant "GitHub Copilot"
git config commit.llmModel "Claude Opus 5"
```

## Before committing

Run `git log -5` when unfamiliar with the repo to confirm current message conventions before writing a commit.
