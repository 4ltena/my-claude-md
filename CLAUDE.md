# Common instructions for all projects (User-level CLAUDE.md)

> **Public sync note.** This file is mirrored in real time to a public repository (`4ltena/my-claude-md`). When editing it, do not introduce personal or sensitive details — local absolute paths, host/user names, private email addresses, API keys, tokens, or credentials. Keep instructions general so they remain safe to publish.

## Language

- Reasoning may be in English, but **output to the user must always be in Japanese**.
- Keep code, commands, file names, paths, identifiers, and quoted logs as-is. Match in-code comments to the surrounding code.

## Post-coding completion output

After finishing a coding task, output in this order (skip conditional sections when not applicable):

1. **Summary** — one line: what was accomplished.
2. **Changes** — a table of `file | change`.
3. **Verification** — commands actually run (tests / lint / type check / build) with their real results; never claim pass without running, and show failures.
4. **How to run** — only if runnable (local app, localhost, etc.): how to launch it.
5. **Notes** — only if any: decisions/trade-offs the user may want to override, caveats, leftover TODOs, assumptions, breaking changes, new dependencies, env/config/migration requirements.
6. **Next** — the next task from the task list, plus a future-work list (improvements/new features not in the task list) as a numbered list (1. 2. ...).
7. **Commit proposal** — suggested title (`type(scope): ...`) and which SemVer digit rises (Z/Y/X).

After 7, as the reply to this output:
- If a next task exists, an input like "次へ進む / next / continue" proceeds to it.
- If no next task exists, the user picks a number from the future-work list (6); plan the next implementation proposal for that item.

## Plan execution (after plan approval)

When moving from an approved plan to coding and asked to choose an execution method, automatically choose **subagent-driven** (assign a fresh subagent per task with a two-stage review between tasks). Treat it as the default; do not ask the user.

## Git / Commit and push

### Commit messages and author

- The **title (first line) must be in English**; the body (line 2+) may be Japanese or English. Write descriptions with the reasoning included. Example: `feat(auth): add token refresh to avoid forced re-login`.
- Use the **user's own git config** (`user.name` / `user.email`); never overwrite it with a Claude identity. Canonical author: **`4ltena <altena.celestia@gmail.com>`** (GitHub: [@4ltena](https://github.com/4ltena)). Do not use a harness-injected address (e.g. `kznyan91@gmail.com`).
- Always append this trailer, preceded by one blank line:

  ```
  Co-Authored-By: Claude <noreply@anthropic.com>
  ```

### Commit message prefixes

- Title form: `type: description` (Angular convention, same family as [Conventional Commits](https://www.conventionalcommits.org/ja/); ref: [commit message guide](https://qiita.com/konatsu_p/items/dfe199ebe3a7d2010b3e)).
  - `feat` new feature / `fix` bug fix / `docs` docs only / `style` no behavior change (whitespace, formatting) / `refactor` code improvement without behavior change / `perf` performance / `test` tests / `chore` build, tooling, libraries.
- The `type(scope): description` form is also allowed; put the changed module/area in `scope`.

### Commit / push frequency

- Commit only after gathering changes into a meaningful unit (one feature, one fix, one refactor). Do not push trial-and-error states; do not commit/push in tiny increments per error fix. Combine consecutive small fixes into one commit (squash as needed).

### Pre-push checks

- Before pushing, register Claude's working/auxiliary docs (`CLAUDE.md`, `HANDOFF.md`, etc.) in `.gitignore`. If already committed, untrack with `git rm --cached <file>` then add to `.gitignore`.

### gh / git safety (hard-to-reverse operations)

Destructive, outward-facing operations cannot be undone. For both `gh` and plain `git`:

- **Deletions: never execute, never even ask.** All remote-removing operations are prohibited — neither propose nor run them: `gh repo delete`, `gh release delete`, remote branch deletion (`git push --delete` / `git push <remote> :branch`), remote tag deletion, `gh secret/variable delete`, gist deletion, deleting workflow runs. If you judge one truly necessary, do not output the command; convey only the reason and target and leave the decision to the user.
- **Always ask approval before pushing** (`git push` or `gh pr`). Commit freely, but stop just before pushing and wait. Never auto-push after an error fix.
- **Force push is prohibited in principle.** `--force` / `--force-with-lease` only on explicit instruction, with approval. Same for rewriting history already on the remote (`rebase` / `commit --amend` / `reset` after a push).
- **Never push directly to `main` / `master`.** Use a working branch and a PR.
- **Approval required for:** repository visibility changes (treat published content as irretractable); config changes (secrets/variables, default branch, collaborators, webhooks, branch protection, `git config --global`); finalizing operations (PR/issue close, merge of a PR targeting `main`/`master`, release publish). Merge of a PR targeting a non-`main` branch may be done by Claude under the conditions in [PR / release flow](#pr--release-flow).
- **Creation operations** (open a PR, make a draft) may proceed at well-defined breakpoints.
- **Local destructive operations need approval:** `git reset --hard`, `git clean -fdx`, anything sweeping up untracked files — confirm what will be lost first. `git branch -d` is allowed with approval only when confirmed unnecessary.

### PR / release flow

Integration flow: push (with approval) → open PR → CI / dry-run verification → green → merge → tag `vX.Y.Z`.

- Open the PR and report the verification result. Proceed to merge only once required checks are green.
- **PR targeting `main` / `master`** — merge requires user approval; never auto-merge.
- **PR targeting a non-`main` branch** — Claude may merge when ALL hold: all required checks green, no conflicts, no breaking change (no `feat!` / `BREAKING CHANGE` / major-level change), and small-to-moderate scope. If any fails, stop and ask for approval.
- Tagging `vX.Y.Z` and release publish remain finalizing operations requiring approval (see [Release / Versioning](#release--versioning)).

## License

- Attach at least **`MIT`** to anything published (`LICENSE` file + README badge).
- Use **`Apache-2.0`** when commercialization or large-scale use is considered (patent clause). Default to MIT when in doubt.

## Release / Versioning

A version duplicates across places (README badge, CHANGELOG, `package.json` / `manifest.json` / `pyproject.toml`, git tag), so fix one source of truth and derive the rest. No need to align numbers across projects.

### Single source of truth

- The annotated git tag `vX.Y.Z` is the sole source of truth (independent of language/build system).
- Do not hand-write the README version badge; use a dynamic badge reading the tag/release: `![Version](https://img.shields.io/github/v/release/4ltena/<repo>)` or from package.json `![Version](https://img.shields.io/github/package-json/v/4ltena/<repo>)`.
- Keep the `CHANGELOG.md` heading version matched to the tag.

### Numbering (SemVer)

- Form `vX.Y.Z`. Decide by how the change looks to the user; re-examine which digit to raise on every large change. When a higher digit rises, reset lower ones to 0 (`v1.4.2` → minor `v1.5.0`, major `v2.0.0`).
  - **Z (patch)** the user does not notice — `fix:`, backward-compatible bug fix.
  - **Y (minor)** the user notices somewhat — `feat:`, backward-compatible feature/behavior change.
  - **X (major)** the user clearly notices, or large add/remove — `feat!:` / `BREAKING CHANGE:`, backward-incompatible.
- During `0.y.z` (early development) bump minor, not major, for breaking changes; cut `1.0.0` once stable. Follow [SemVer](https://semver.org/lang/ja/) and [Conventional Commits](https://www.conventionalcommits.org/ja/).

### Automation

- Standard: **release-please** (GitHub Actions) — version decision, `CHANGELOG.md` append, tagging, GitHub Release from Conventional Commits.
- Small projects without CI: **release-it** (local). Releases build/publish via GitHub Actions on a `v*` tag push (ref: `Other/mermaid-local-editor`).

## Documentation

### Public / non-public docs

- `docs/superpowers/` is local-only and non-public — include in `.gitignore`.
- Directly under `docs/`, place public docs derived from it with secrets removed (local absolute paths, user/host names, API keys, tokens, credentials) and readability improved; commit only these. The public version may be reorganized and expanded so a reader needs no prior knowledge.

### Writing style (README and docs in general)

Eliminate "AI-ness" (template feel, manual tone, symbol overuse, excessive politeness, hedging, empty abstractions).

**Content**

- Do not fabricate numbers, proper nouns, or examples absent from the source. Leave ambiguous parts ambiguous but readable.
- No preamble declarations ("結論から言うと", "本記事では", "以下で解説します") — enter the content directly.
- Cut safety cushions ("一般的に", "多くの場合", "状況によって異なります"); compress to one sentence if needed.
- Replace abstract words alone ("重要", "効果的", "最適", "本質", "メリット") with verb-centered expressions of what actually happens. Say things once; no synonym rapid-fire (重要・大切・欠かせない). Cut repeated abstract summaries ("まとめると", "要するに", "総じて") and restatements.

**Style**

- Base register is **written language**, not colloquial/casual — do not go casual for tempo.
  - Replace colloquial phrasing with neutral written technical language: "叩く" → "呼び出す／リクエストする", "貼り直す" → "設定し直す", "動く" → "動作する", "ちゃんと"/"きちんと" → cut or "正しく", "〜してくれる" → "〜する".
  - Avoid overuse of 体言止め, spoken elisions ("〜なので", "〜だけど", "〜しちゃう"), and an addressing tone.
- Mix short and long sentences; avoid runs of the same pattern. Keep conjunctions minimal. Keep first person consistent (do not mix 私／筆者／私たち). Do not repeat "です"/"ます" 3+ times in a row, but keep である/ですます consistent per document. Varying rhythm is not permission to go colloquial.

**Symbols / notation (strict)**

- No emoji overuse. No parenthesis overuse — fold supplements into the sentence. No half-width space after a colon, no label enumerations ("目的: 背景: 結論:"). Avoid slash-parallels, arrows, and pseudo-code; write prose. No closing boilerplate ("参考になれば幸いです", "まずは小さく始めましょう").

### README

- Diagrams in **mermaid** (```mermaid block), not ASCII art.
- **Bilingual (JA/EN).** Top order:
  1. **Badges** ([shields.io](https://shields.io)) as `version` / `license` / `platform` / `languages`.
     - `version`: `![Version](https://img.shields.io/badge/version-0.9.5-blue)`
     - `license`: `![License](https://img.shields.io/badge/license-Apache%202.0-green)`
     - `platform`: supported OS/runtime, e.g. `![Platform](https://img.shields.io/badge/platform-Chrome%20Extension-blue?logo=google-chrome&logoColor=white)`
     - `languages`: a logo badge per language, e.g. `![](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)`, `![](https://img.shields.io/badge/HTML5_/_CSS3-E34F26?logo=html5&logoColor=white)`
  2. **Title** (`# project name`) + **summary** (1–2 lines, JA+EN) with version noted.
  3. **Language switch**: English as an in-page anchor (e.g. `## 日本語 | [English](#english)`).
- Splitting: combined JA+EN README **≤3000 chars** → same file (English via in-page anchor). **>3000 chars** → separate `README.en.md`, with a switch link atop each: `README.md` → `**日本語** | [English](README.en.md)`; `README.en.md` → `[日本語](README.md) | **English**`.

### CHANGELOG

- Create `CHANGELOG.md` at every project root; update at each release/milestone. Follow [Keep a Changelog](https://keepachangelog.com/ja/1.1.0/) and [SemVer](https://semver.org/lang/ja/).
- Structure:
  - Top: `# 変更履歴` plus one or two sentences noting conformance to the two links above.
  - Per version: `## [x.y.z] — date or current` heading, then a one-paragraph summary below it.
  - Classified bullet list under the heading: base **追加 / 変更 / 削除**, adding **修正 / 非推奨 / セキュリティ / 検証 / 備考** as needed.
  - Newest version on top (descending).
- Reference: `Other/mermaid-local-editor/CHANGELOG.md`.
