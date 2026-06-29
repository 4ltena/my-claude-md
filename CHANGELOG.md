# 変更履歴

このファイルは [Keep a Changelog](https://keepachangelog.com/ja/1.1.0/) に従う。バージョン（タグ）はこのリポジトリ固有で、日付ベースの `YYYY-MMDD-NNNN` 形式を用いる（`NNNN` はその日の更新通番、当日1回目が `0001`）。最新版を上に置く。

## [2026-0629-0002] — 2026-06-29

Windows でも同じ仕組みで同期できるようにし、タグの採番方式を日付ベースに切り替えた。

### 追加

- Windows 版の監視・同期。PowerShell の `FileSystemWatcher` を `watch.ps1` で常駐させ、`install.ps1` でタスクスケジューラにログオン起動として登録する。`sync.ps1` は macOS 版の `sync.sh` と同等に commit / push / タグ採番を行う。
- 日付ベースのタグ採番 `YYYY-MMDD-NNNN`（その日の更新通番）。push 成功のたびに当日のタグ数から次番号を採番・push する。

### 変更

- バージョン体系を SemVer から日付ベースのタグへ変更した。`README.md` の platform バッジを macOS と Windows の2つにした。

## [2026-0629-0001] — 2026-06-29

グローバル `CLAUDE.md` を公開ミラーする仕組みの初版。macOS の launchd による監視と自動 push を導入し、リポジトリに `CLAUDE.md` / `README.md` / `CHANGELOG.md` を用意した。

### 追加

- ユーザーレベルのグローバル `CLAUDE.md` をリポジトリに取り込み、公開対象とした。
- `CLAUDE.md` の内容と自動同期の仕組みを日本語でまとめた `README.md`。
- macOS の launchd (`WatchPaths`) で `~/.claude/CLAUDE.md` の変更を検知し、差分があれば `main` へ自動 commit / push する同期スクリプト（リポジトリ外に配置）。

### 備考

- 自動ミラー用途に限り、`CLAUDE.md` の「push 前に必ず承認」「`main` への直 push 禁止」ルールを上書きしている。
- `CLAUDE.md` を更新する際は、個人のパス構成や名前など公開に適さない情報を新たに加えないよう注意する。
