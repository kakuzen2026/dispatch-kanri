# dispatch-kanri プロジェクト設定



## テンプレート変数の展開値



| 変数 | 値 |

|---|---|

| `{{PROJECT_NAME}}` | KAKUZEN 派遣・従業員管理システム |

| `{{CLIENT_NAME}}` | 株式会社KAKUZEN |

| `{{DB_STACK}}` | Supabase |

| `{{REPO_NAME}}` | dispatch-kanri |

| `{{GHPAGES_URL}}` | https://kakuzen2026.github.io/dispatch-kanri/ |

| `{{FIREBASE_PROJECT_ID}}` | (Supabase使用のため該当なし) |

| `{{SUPABASE_PROJECT_REF}}` | wkzbsdfgslidubqpifwa |

| `{{MONTHLY_PLAN}}` | (記入) |



## プロジェクト固有情報



### 用途

派遣スタッフ・従業員の一元管理(取引先・現場・契約・請求・有給・書類発行)



### 現状

- 約85%完成

- 派遣管理側は稼働中

- 従業員管理側はDB統合作業中



### 技術スタック

- **DB・認証**: Supabase (統合先: `wkzbsdfgslidubqpifwa.supabase.co`、Supabase Auth)

- **ファイル保存**: Supabase Storage(在留カード画像・免許証等)

- **ホスティング**: GitHub Pages

- **実装**: 単一HTMLファイル(`index.html` 約5000行)



## プロジェクト固有のルール・メモ



### 重要: リポジトリ構成

- **dispatch-kanri**(このリポジトリ): メインのリポジトリ、GitHub Pages公開URL元

- **employee-ledger**(別リポジトリ): 実装本体



### dispatch-kanri/index.htmlの役割

- `dispatch-kanri/index.html` は `employee-ledger` へのリダイレクトのみ

- 実装本体は `employee-ledger` リポジトリ側



### GitHubアカウント

- 所有: `kakuzen2026` アカウント

- 実運用: 岩本和貴本人

- push時は `kakuzen2026` アカウントの認証が必要



### 関連アプリ

- KAKUZEN従業員台帳アプリ(`employee-ledger` リポジトリ): 実装本体を共有

- 両アプリでSupabaseプロジェクトを統合利用



## 更新履歴



| 日付 | 内容 |

|---|---|

| 2026-04-23 | 新雛形CLAUDE.md適用、CLAUDE.local.md初版作成(別チャット版の内容を統合) |

