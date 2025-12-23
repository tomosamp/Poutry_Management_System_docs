# producers テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 生産者（代表者）アカウントを管理し、施設と紐付ける。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | producers |  |
| テーブル論理名 | 生産者 |  |
| ドメイン/コンテキスト | Producer |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 生産者ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | facility_id | 施設ID | BIGINT UNSIGNED |  | ○ |  | `facilities.id` |  |
| 3 | last_name | 姓 | VARCHAR(50) |  | ○ |  |  |  |
| 4 | first_name | 名 | VARCHAR(50) |  | ○ |  |  |  |
| 5 | last_name_kana | セイ | VARCHAR(50) |  |  |  | 任意（全角カナ） |  |
| 6 | first_name_kana | メイ | VARCHAR(50) |  |  |  | 任意（全角カナ） |  |
| 7 | department | 部署名 | VARCHAR(100) |  |  |  | 任意 |  |
| 8 | email | メールアドレス | VARCHAR(100) |  |  |  |  |  |
| 9 | phone_number | 電話番号 | VARCHAR(30) |  |  |  |  |  |
| 10 | role_type | 役割区分 | ENUM('representative','applicant') |  | ○ |  | 代表/申請者 |  |
| 11 | created_at | 作成日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP |  |  |
| 12 | updated_at | 更新日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP | on update |  |
| 13 | deleted_at | 論理削除 | TIMESTAMP(0) |  |  |  |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| 外部キー | producers_facility_fk | facility_id |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | producers_facility_fk | facilities.id 参照 |  |
| リレーション | Producer-Notes | producers.id ⇔ producer_notes.author_id (author_type=producer) | 予定 |
| リレーション | Producer-Monthly | producers.id ⇔ monthly_records.registered_by_producer_id | 将来拡張 |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
