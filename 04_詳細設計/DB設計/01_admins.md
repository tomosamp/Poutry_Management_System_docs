# admins テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- `admins` テーブルのスキーマ・制約・運用ルールを共有し、認証実装と整合させる。
- 変更時は本書とテーブル一覧を同時更新する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | admins |  |
| テーブル論理名 | 管理者 | 職長協会向け |
| ドメイン/コンテキスト | Authentication |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 管理者ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT | システム採番 |  |
| 2 | email | ログインID（メールアドレス） | VARCHAR(255) |  | ○ |  | 一意制約 |  |
| 3 | password | パスワード | VARCHAR(255) |  | ○ |  | ハッシュ済み値を保存 |  |
| 4 | last_name | 姓 | VARCHAR(50) |  | ○ |  |  |  |
| 5 | first_name | 名 | VARCHAR(50) |  | ○ |  |  |  |
| 6 | last_name_kana | セイ | VARCHAR(50) |  | ○ |  | 全角カナ |  |
| 7 | first_name_kana | メイ | VARCHAR(50) |  | ○ |  | 全角カナ |  |
| 8 | created_at | 作成日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP | Laravel標準 |  |
| 9 | updated_at | 更新日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP | on update |  |
| 10 | deleted_at | 論理削除 | TIMESTAMP(0) |  |  |  | soft delete |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ | レコード一意 |  |
| ユニーク | admins_email_unique | email | ○ | ログイン照合 |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | - | なし |  |
| リレーション | Admin-Exports | admins.id ⇔ agency_exports.exported_by_admin_id | 予定 |
| リレーション | Admin-Monthly | admins.id ⇔ monthly_records.registered_by_admin_id | 予定 |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
