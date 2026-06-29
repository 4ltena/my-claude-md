# 変更履歴

このファイルは [Keep a Changelog](https://keepachangelog.com/ja/1.1.0/) と [SemVer](https://semver.org/lang/ja/) に従う。最新版を上に置く。

## [0.1.0] — 2026-06-29

グローバル `CLAUDE.md` を公開ミラーする仕組みの初版。launchd による監視と自動 push を導入し、リポジトリに `CLAUDE.md` / `README.md` / `CHANGELOG.md` を用意した。

### 追加

- ユーザーレベルのグローバル `CLAUDE.md` をリポジトリに取り込み、公開対象とした。
- `CLAUDE.md` の内容と自動同期の仕組みを日本語でまとめた `README.md`。
- macOS の launchd (`WatchPaths`) で `~/.claude/CLAUDE.md` の変更を検知し、差分があれば `main` へ自動 commit / push する同期スクリプト（リポジトリ外に配置）。

### 備考

- 自動ミラー用途に限り、`CLAUDE.md` の「push 前に必ず承認」「`main` への直 push 禁止」ルールを上書きしている。
- `CLAUDE.md` を更新する際は、個人のパス構成や名前など公開に適さない情報を新たに加えないよう注意する。
