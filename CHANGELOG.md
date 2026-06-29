# 変更履歴

このファイルは [Keep a Changelog](https://keepachangelog.com/ja/1.1.0/) に従い、`CLAUDE.md` の内容変更だけを記録する。バージョンは日付ベースの `YYYY-MMDD-NNNN` 形式で、`NNNN` はその日の `CLAUDE.md` 更新通番（当日1回目が `0001`）。各バージョンは `CLAUDE.md` を変更したコミットに付くリポジトリの git タグと一致する（README や CHANGELOG だけの更新ではタグもこのファイルも増えない）。最新版を上に置く。

## [2026-0629-0004] — 2026-06-29

ローカルでの破壊的操作に関する安全規則を一段細かくした。

### 追加

- ローカル破壊的操作の注意書きに、`git branch -d` はマージ済みまたは不要と確認できれば承認のうえ実行してよい旨を加えた。

## [2026-0629-0003] — 2026-06-29

冗長だった Git 関連の記述と個別の運用節を、オンデマンドで読み込むスキルへの参照に集約して再構成した。

### 変更

- 「Git / Commit and push」配下の小節（コミット作法・接頭辞・頻度・push 前チェック・gh/git 安全規則・PR フロー）を、「Git — author, commits, frequency」と「Git — safety (always-on)」の2節に圧縮した。
- Language に、人向けの日本語文では AI らしさを避ける方針と `writing-style-ja` スキルへの参照を加えた。Plan execution に `subagent-driven-development`、Verification に `verification-before-completion` への参照を補った。
- 冒頭の Public sync note を広げ、トピック別の知見はミラー対象外のローカルスキルに置く旨を明記した。

### 削除

- 本文に直書きしていた License・Release / Versioning・Documentation の詳細を外し、「Conditional playbooks (load on demand)」から `releasing` / `docs-authoring` / `writing-style-ja` スキルへの索引に置き換えた。

## [2026-0629-0002] — 2026-06-29

公開ミラーに不要な個人情報が載らないよう、作者設定の記述を一般化した。

### セキュリティ

- git の作者設定の説明から個人のメールアドレスを削除し、「メールアドレスを本ファイルにハードコードしない。マシンのグローバル設定にある作者情報を使う」という記述へ改めた。

## [2026-0629-0001] — 2026-06-29

全プロジェクト共通のユーザーレベル `CLAUDE.md` を公開ミラーの対象として収録した初版。

### 追加

- 出力言語、コーディング完了後の出力書式、プラン実行方針、コミットと push の作法・安全規則、ライセンス、バージョニング、ドキュメント作法を収めた `CLAUDE.md`。
- 冒頭に、本ファイルが公開リポジトリへ実時間でミラーされるため個人情報や機微情報を含めない旨の注意書き。
