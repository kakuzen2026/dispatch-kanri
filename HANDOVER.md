# KAKUZEN プロジェクト引継書

## プロジェクト基本情報

- **アプリ名**: 派遣・従業員管理システム（KAKUZEN）
- **用途・目的**: 派遣スタッフおよび従業員の管理。取引先・現場・契約・請求・有給・雇用契約書・証明書発行などを一元管理する社内業務システム
- **GitHubアカウント名**: kakuzen2026
- **GitHubリポジトリ**:
  - `kakuzen2026/dispatch-kanri`（派遣管理 ※メインリポジトリ）
  - `kakuzen2026/employee-ledger`（従業員管理 ※統合前の旧リポジトリ）
- **公開URL**: https://kakuzen2026.github.io/dispatch-kanri/（GitHub Pages）

---

## 技術構成

- **DB**: Supabase（統合作業中）
  - 統合先（派遣管理DB）: `https://wkzbsdfgslidubqpifwa.supabase.co`
  - 統合元（従業員管理DB・廃止予定）: `https://jqokhqybxjkcantxsjfm.supabase.co`
- **認証方式**: Supabase Auth（メール＋パスワード）
  - ログインアカウント: `k.iwamoto.202602@gmail.com`
- **ファイル保存先**: Supabase Storage（在留カード画像・免許証画像等）
- **ホスティング**: GitHub Pages
- **実装形式**: 単一HTMLファイル（`index.html` 約5000行超）

---

## データ構造

### 統合先DB（wkzbsdfgslidubqpifwa）の主要テーブル

| テーブル名 | 役割 |
|-----------|------|
| `clients` | 取引先（派遣先企業）管理 |
| `sites` | 現場管理 |
| `contracts` | 派遣契約管理 |
| `contract_employees` | 契約に紐づく従業員 |
| `billing` | 請求管理 |
| `employee_records` | 勤怠・記録管理 |
| `work_patterns` | 勤務パターンマスター（現場用・site_id付き） |
| `settings` | システム設定 |
| `assignments` | アサイン管理 |
| `doc_templates` | 書類テンプレート |
| `employees` | 従業員情報（統合後） |
| `departments` | 部署マスター |
| `visa_types` | 在留資格マスター |
| `yukyu_records` | 有給取得記録 |
| `yukyu_grants` | 有給付与記録 |
| `certificates` | 証明書発行履歴 |
| `employment_contracts` | 雇用契約書履歴 |
| `emp_work_patterns` | 勤務パターンマスター（従業員管理用・sort_order付き） |
| `company_info` | 自社情報 |
| `dispatch_contracts` | 派遣契約同期テーブル |

---

## 主要機能

### 派遣管理側
- ダッシュボード（契約・請求サマリー）
- 取引先管理（CRUD）
- 現場管理（CRUD）
- 契約管理（契約期間・従業員アサイン・書類生成）
- 請求管理
- 記録管理
- 設定

### 従業員管理側
- 従業員一覧・詳細・CRUD
- 在留資格・ビザ期限管理
- 有給管理（取得・付与・残日数計算）
- 証明書発行（在職証明書等）
- 雇用契約書生成・履歴管理
- 健康診断記録
- 資格・免許管理
- 銀行口座情報管理
- 部署マスター管理
- 会社情報管理

---

## 現在の状態

- **完成度**: 約85%
- **状態**: 稼働中（派遣管理側） ／ 統合作業中（従業員管理側）

---

## 進行中の作業

**DB統合作業**（2つのSupabaseプロジェクトを1つに統合）

| ステップ | 内容 | 状態 |
|---------|------|------|
| Step 1 | 統合先DBにテーブル作成 | 未完了 |
| Step 2 | RLS設定 | 未完了 |
| Step 3 | データ移行（Kazuki手動実施） | 未完了 |
| Step 4 | `index.html` コード修正 | 未完了 |
| Step 5 | 動作確認 | 未完了 |

---

## 次にやること（優先順）

1. **Step 1〜2**: 統合先DB（`wkzbsdfgslidubqpifwa`）のSQL Editorで、テーブル作成・RLS設定のSQLを実行する
2. **Step 3**: 従業員管理DB（`jqokhqybxjkcantxsjfm`）から全データをエクスポートし、統合先DBにインポートする（Kazukiが手動実施）
3. **Step 4**: `index.html` のコード修正
   - `EMP_URL` / `EMP_KEY` / `empDb` クライアント削除
   - `sb()` 関数を `db` クライアントのラッパーに置き換え
   - `empDb.from(...)` → `db.from(...)` に一括置換（9箇所）
   - `sb('work_patterns...')` → `sb('emp_work_patterns...')` に変更（4箇所）
4. **Step 5**: 動作確認（コンソールエラーなし・ログイン・各機能の確認）

---

## 未解決の課題・既知のバグ

- モバイルUIのCSSセレクターバグ → **修正済み**（2026-04-20）
  - `@media(max-width:768px)` 内のボタン向けセレクター4箇所を修正
  - `html, body { overflow-x: hidden }` を追加

---

## 注意事項・独自ルール

- **`db.from('work_patterns')`（派遣管理側・site_id付き）は変更しない**
  - 変更するのは `sb('work_patterns...')` 経由のもの（従業員管理側）のみ
- **`sb()` 関数はラッパーとして残す**（呼び出し箇所が多いため直接置換しない）
- **データ移行（Step 3）はKazukiが手動で行う**
- コメントは最小限（複雑な処理のみ日本語で1行）
- LocalStorage・sessionStorage 使用禁止
- 不要な `console.log` を入れない
- 変数名・関数名は短く明確に

---

## 直近で編集したファイルと箇所

- **ファイル名**: `index.html`
- **変更内容**:
  - 行12: `html { overflow-x: hidden }` / `body { overflow-x: hidden }` を追加
  - 行167: `.topbar-actions #main .btn, #login-screen .btn` → `.topbar-actions .btn`
  - 行188: `td:last-child #main .btn, #login-screen .btn` → `td:last-child .btn`
  - 行210: `.modal-footer #main .btn, #login-screen .btn` → `.modal-footer .btn`
  - 行222: `.section-header #main .btn, #login-screen .btn` → `.section-header .btn`

---

## クライアント構成（統合前の参考情報）

```
db    = Supabase JSクライアント → 派遣管理DB（認証付き）
empDb = Supabase JSクライアント → 従業員管理DB（認証なし・廃止予定）
sb()  = fetch API直接 → 従業員管理DB（anon key・廃止予定）
```

統合後は `db` 一本に統合し、`sb()` は `db` のラッパーとして残す。
