# danyiimp/telegram-mcp — fork workflow

Fork of [chigwell/telegram-mcp](https://github.com/chigwell/telegram-mcp).

| Branch | What it is | Rules |
|---|---|---|
| `main` | `upstream/main` + our **upstreamable** changes, one commit per change, conventional-commit titles. Default branch. | Only generic changes. No environment-specific code. Rebased onto `upstream/main`; a change that upstream merged drops out of the rebase by itself. |
| `feat/<topic>`, `fix/<topic>` | One topic, one commit, cut from `upstream/main`. Exactly what a pull request contains. | Kept until the pull request is merged or declined. |

This fork carries only upstreamable changes.

## Why this shape

- A pull request must be a clean diff against upstream, so each change lives on its own branch cut from `upstream/main`. `main` is the same commits stacked.
- Rebase, not merge, keeps one commit per change and lets `git rebase` drop a change once upstream has it. `main` is therefore rewritten on sync (`--force-with-lease`); consumers pin a tag or SHA, not a branch tip.

## Syncing with upstream

```sh
git fetch upstream
git checkout main && git rebase upstream/main
git push --force-with-lease origin main
```

When a topic branch is merged upstream, delete it; the same commit disappears from `main` on the next rebase. When a maintainer asks for changes, amend the topic commit, then bring the amended version into `main` (re-cherry-pick).
