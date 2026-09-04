---
applyTo: '**'
---

# Copilot instructions for copilot-sandbox

## Commit messages

Every commit MUST use a [Conventional Commits](https://www.conventionalcommits.org/) subject line.
Commits produced with GitHub Copilot MUST also carry the AI-transparency trailers:

```
type(scope): short imperative summary

- Bullet describing what changed and why
- Additional bullets as needed

GitHub-Copilot: true
LLM-Model: <exact model name, e.g. Claude Opus 5>
```

A hand-written commit made without Copilot carries no trailers at all:

```
type(scope): short imperative summary

- Bullet describing what changed and why
```

Rules:

- Subject line types: `feat`, `fix`, `docs`, `refactor`, `build`, `chore`, `test`, `style`, `perf`, `ci`. Scope is optional but preferred.
- The trailers indicate Copilot involvement by their presence. Never write `GitHub-Copilot: false` — omit both trailers instead.
- Set `LLM-Model` to the model that actually produced the change.
- Pass multi-line messages with repeated `-m` flags so the trailers land in the commit footer.
- A `prepare-commit-msg` hook in `.githooks/` adds the trailers when `commit.githubCopilot` is `true`, and does nothing otherwise. Write them explicitly anyway rather than relying on the fallback model value.

Enable the hook once per clone:

```pwsh
git config core.hooksPath .githooks
git config commit.githubCopilot true
git config commit.llmModel "Claude Opus 5"
```

Set `commit.githubCopilot` to `false` for stretches of hand-written work so the hook stays quiet.

## Before committing

Run `git log -5` when unfamiliar with the repo to confirm current message conventions before writing a commit.
