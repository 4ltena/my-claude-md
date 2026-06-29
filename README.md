![Version](https://img.shields.io/github/v/tag/4ltena/my-claude-md?label=updated&color=blue)
![Format](https://img.shields.io/badge/format-Markdown-083fa1?logo=markdown&logoColor=white)

# my-claude-md

My claude.md

**日本語** | [English](#english)

---

## 日本語

### このリポジトリについて

[`CLAUDE.md`](./CLAUDE.md) が本体で、全プロジェクトに適用される共通指示をまとめたもの。出力言語、コミットやリリースの作法、ドキュメントの書き方などを定めている。参考や流用の用途で公開している。

公開リポジトリのため、`CLAUDE.md` には個人のパス構成や名前など公開に適さない情報を新たに加えないようにしている（同趣旨の注意書きを `CLAUDE.md` 冒頭にも記載）。

### CLAUDE.md の内容

| 節 | 概要 |
| --- | --- |
| Language | 出力は必ず日本語。コード・コマンド・パス・識別子・ログはそのまま残す。 |
| Post-coding completion output | 完了後に Summary / Changes / Verification / How to run / Notes / Next / Commit proposal の順で出力する書式。 |
| Plan execution | 承認済みプランの実装は subagent-driven を既定とする。 |
| Git / Commit and push | 英語タイトル・`type: description`・`Co-Authored-By` トレーラーなどのコミット作法、push 前チェック、削除や直 push を避ける安全規則、PR とリリースのフロー。 |
| License | 公開物は最低 MIT、商用・大規模利用は Apache-2.0。 |
| Release / Versioning | 注釈付き git タグを唯一の真実とする SemVer 運用と自動化。 |
| Documentation | 公開・非公開ドキュメントの分離、文章作法、README と CHANGELOG の構成規約。 |

全文は [`CLAUDE.md`](./CLAUDE.md)、変更履歴は [`CHANGELOG.md`](./CHANGELOG.md) を参照。

---

## <a name="english"></a>English

[日本語](#日本語) | **English**

### About this repository

The core file is [`CLAUDE.md`](./CLAUDE.md): the shared Claude Code instructions applied to all of my projects, covering output language, commit and release conventions, documentation style, and more. It is published for reference and reuse.

Because the repository is public, `CLAUDE.md` is kept free of personal paths, names, or other details that should not be published; the same note appears at the top of `CLAUDE.md`.

### What CLAUDE.md contains

| Section | Summary |
| --- | --- |
| Language | Always reply in Japanese; keep code, commands, paths, identifiers, and logs verbatim. |
| Post-coding completion output | Summary / Changes / Verification / How to run / Notes / Next / Commit proposal, in that order. |
| Plan execution | Subagent-driven by default. |
| Git / Commit and push | Commit conventions (English title, `type: description`, `Co-Authored-By` trailer), push-safety rules, and PR / release flow. |
| License | MIT at minimum, Apache-2.0 for commercial or large-scale use. |
| Release / Versioning | Annotated git tags as the single source of truth (SemVer) plus automation. |
| Documentation | Public vs. local docs, writing style, README and CHANGELOG conventions. |

See [`CLAUDE.md`](./CLAUDE.md) for the full text and [`CHANGELOG.md`](./CHANGELOG.md) for history.
