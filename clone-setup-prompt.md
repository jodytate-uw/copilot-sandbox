# Configuring git trailers in a fresh clone

Git will not activate a repository's hooks automatically when you clone — `core.hooksPath` is
local config, and letting a repo set it on clone would allow arbitrary code execution. So each
clone needs a one-time setup step.

Paste the prompt below into GitHub Copilot Chat after cloning this repo.

## The prompt

```text
Configure this clone so every commit I make carries the repository's AI-transparency trailers.

Required commit message format:

    type(scope): short imperative summary

    - Bullet describing what changed and why

    GitHub-Copilot: true
    LLM-Model: <the model that actually produced the change>

`GitHub-Copilot` is present only when Copilot contributed to the change. A hand-written commit
carries neither trailer; never write `GitHub-Copilot: false`.

Do the following:

1. Confirm `.githooks/prepare-commit-msg` exists. If it is missing, create it as a POSIX shell
   script that exits without changing anything when `$2` is `merge` or `squash`, or when the
   `commit.githubCopilot` config key is not truthy. Otherwise it appends a `GitHub-Copilot: true`
   trailer and an `LLM-Model` trailer to the commit message file whenever either one is absent,
   taking the model from `commit.llmModel` and falling back to `Unspecified`. Use
   `git interpret-trailers --in-place` to append.
2. Run these commands, substituting the model you are currently running as:

       git config core.hooksPath .githooks
       git config commit.githubCopilot true
       git config commit.llmModel "<your model name>"

3. Verify the setup by checking that `git config --get core.hooksPath` returns `.githooks`, and
   that the hook file is executable.
4. Report which values you set. Do not create a test commit unless I ask.

From now on, write the trailers explicitly in every commit message you make in this repo rather
than relying on the hook to backfill them. Check `git log -5` before your first commit to confirm
the convention has not changed.
```

## What it configures

| Setting | Value |
| --- | --- |
| `core.hooksPath` | `.githooks` |
| `commit.githubCopilot` | `true` |
| `commit.llmModel` | the model in use, e.g. `Claude Opus 5` |

The hook is a safety net, not the primary mechanism. The authoritative description of the
convention lives in [.github/copilot-instructions.md](.github/copilot-instructions.md), which
Copilot reads automatically in any clone.
