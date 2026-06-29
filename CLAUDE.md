# Common instructions for all projects (User-level CLAUDE.md)

> **Public sync note.** This file is mirrored in real time to a public repository (`4ltena/my-claude-md`). When editing it, do not introduce personal or sensitive details — local absolute paths, host/user names, private email addresses, API keys, tokens, or credentials. Keep instructions general so they remain safe to publish. Topic-specific playbooks live in local skills (`~/.claude/skills/`), which are not mirrored.

## Language

- Reasoning may be in English, but **output to the user must always be in Japanese**.
- Keep code, commands, file names, paths, identifiers, and quoted logs as-is. Match in-code comments to the surrounding code.
- When writing human-facing Japanese prose (README, docs, CHANGELOG summaries, PR/issue/commit bodies, articles), avoid AI-ness: no preamble declarations, no hedging cushions, written register over colloquial, no symbol/emoji overuse. Full checklist: the **`writing-style-ja`** skill.

## Post-coding completion output

After finishing a coding task, output in this order (skip conditional sections when not applicable):

1. **Summary** — one line: what was accomplished.
2. **Changes** — a table of `file | change`.
3. **Verification** — commands actually run (tests / lint / type check / build) with their real results; never claim pass without running, and show failures. (Overlaps with `superpowers:verification-before-completion`; don't restate the principle twice.)
4. **How to run** — only if runnable (local app, localhost, etc.): how to launch it.
5. **Notes** — only if any: decisions/trade-offs the user may want to override, caveats, leftover TODOs, assumptions, breaking changes, new dependencies, env/config/migration requirements.
6. **Next** — the next task from the task list, plus a future-work list (improvements/new features not in the task list) as a numbered list.
7. **Commit proposal** — suggested title (`type(scope): ...`) and which SemVer digit rises (Z/Y/X).

After 7: if a next task exists, "次へ進む / next / continue" proceeds to it; if not, the user picks a number from the future-work list and you plan that item.

## Plan execution (after plan approval)

When moving from an approved plan to coding and asked to choose an execution method, automatically choose **subagent-driven** (a fresh subagent per task with a two-stage review between tasks). Treat it as the default; do not ask. Procedure details: `superpowers:subagent-driven-development`.

## Git — author, commits, frequency

- Commit **title (first line) in English**; body (line 2+) may be Japanese or English, with the reasoning included. Example: `feat(auth): add token refresh to avoid forced re-login`.
- Use the **user's own git config** (`user.name` / `user.email`); never overwrite it with a Claude identity, and do not hard-code any email address in this file. The canonical author identity (GitHub [@4ltena](https://github.com/4ltena)) is already set in the machine's global git config — verify it is in effect before committing. Never substitute a harness-injected address.
- Always append this trailer, preceded by one blank line:

  ```
  Co-Authored-By: Claude <noreply@anthropic.com>
  ```

- Title prefix: `type: description` (Angular / Conventional Commits): `feat` / `fix` / `docs` / `style` / `refactor` / `perf` / `test` / `chore`. The `type(scope): description` form is also allowed.
- Commit only after gathering changes into a meaningful unit (one feature, one fix, one refactor). Do not push trial-and-error states or commit per-error; squash consecutive small fixes.
- Before pushing, ensure Claude's working/auxiliary docs (`CLAUDE.md`, `HANDOFF.md`, etc.) are in `.gitignore`; untrack with `git rm --cached <file>` if already committed.

## Git — safety (always-on)

Destructive, outward-facing operations cannot be undone. The machine-checkable subset (remote deletions, force push, push to `main`/`master`) is also enforced in `~/.claude/settings.json` (`permissions.deny`/`ask`) and a PreToolUse hook; the judgment calls below remain yours.

- **Deletions: never execute, never even ask.** All remote-removing operations are prohibited — `gh repo/release/secret/variable/gist delete`, deleting workflow runs, remote branch/tag deletion (`git push --delete` / colon refspec). If you judge one truly necessary, do not output the command; convey only the reason and target and leave the decision to the user.
- **Always ask approval before pushing** (`git push` or `gh pr`). Commit freely, but stop just before pushing. Never auto-push after an error fix.
- **Never push directly to `main` / `master`.** Use a working branch and a PR; auto-push is limited to working branches.
- **Force push is prohibited in principle.** `--force` / `--force-with-lease` only on explicit instruction, with approval. Same for rewriting pushed history (`rebase` / `commit --amend` / `reset` after a push).
- **Approval required for:** repository visibility changes; config changes (secrets/variables, default branch, collaborators, webhooks, branch protection, `git config --global`); finalizing operations (PR/issue close, merge of a `main`/`master` PR, release publish).
- **Local destructive operations need approval:** `git reset --hard`, `git clean -fdx`, sweeping up untracked files — confirm what will be lost first.
- Creation operations (open a PR, make a draft) may proceed at well-defined breakpoints. The full push→CI→merge→tag flow and merge-eligibility rules are in the `releasing` skill.

## Conditional playbooks (load on demand)

Topic knowledge that applies only sometimes lives in local skills, indexed here:

- **`releasing`** — license choice (MIT/Apache-2.0), SemVer digit rules, annotated tag as single source of truth, dynamic version badges, release-please/release-it, CHANGELOG format, and the PR→tag flow with merge eligibility.
- **`docs-authoring`** — README structure, shields.io badge order, mermaid diagrams, JA/EN bilingual layout and the 3000-char split rule, public/non-public docs derivation.
- **`writing-style-ja`** — de-AI-ify checklist for Japanese prose (the detailed form of the Language note above).
