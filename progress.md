# 作業進捗ログ

長時間作業時、セッションリセット前にClaude Codeがここに進捗を書き出す。
リセット後はこのファイルを読み込んで作業を再開する。

---

## プロジェクト名
KAKUZEN（派遣・従業員管理システム）DB統合作業

## 現在のフェーズ
Phase 3: 実装（DB統合作業中）

## 完了済み
- Step 1: 統合先DB（wkzbsdfgslidubqpifwa）へのテーブル作成（10テーブル）
  - departments / visa_types / emp_work_patterns / company_info / employees /
    yukyu_records / yukyu_grants / certificates / employment_contracts / dispatch_contracts
  - FK依存順（親→子）で作成、id型はBIGSERIAL統一、dispatch_contractsのみuuid
- Step 2: RLS設定（全10テーブルで有効化＋認証済みユーザー向け全権限ポリシー）
- Step 3: データ移行（7テーブル計151件、Kazuki手動実施）

## 進行中
- Step 4: index.html コード修正
  - 作業場所を `kakuzen2026/employee-ledger` リポジトリに切り替えて実施予定

## 次にやること
- Step 4: employee-ledger 側の index.html を修正
  - `EMP_URL` / `EMP_KEY` / `empDb` クライアント削除
  - `sb()` 関数を `db` クライアントのラッパーに置き換え
  - `empDb.from(...)` → `db.from(...)` に一括置換（9箇所）
  - `sb('work_patterns...')` → `sb('emp_work_patterns...')` に変更（4箇所）
- Step 5: 動作確認（コンソールエラーなし・ログイン・各機能）

## 未解決・注意事項
- 本リポジトリ（dispatch-kanri）の派遣管理側HTML（hakenn.html等）にも
  `empDb.from('employees')` / `empDb.from('dispatch_contracts')` の呼び出しあり。
  Step 4完了後、派遣管理側も同様に `db.from(...)` への置換が必要
- `db.from('work_patterns')`（派遣管理側・site_id付き）は変更しない
- `sb()` 関数はラッパーとして残す（呼び出し箇所が多いため直接置換しない）

## 直近で触ったファイル
- progress.md（進捗記録）

---

最終更新: 2026-04-20
