# producer_notes テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 施設に紐づく特記事項メモを管理し、共有する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | producer_notes |  |
| テーブル論理名 | 施設メモ |  |
| ドメイン/コンテキスト | Producer |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | メモID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | facility_id | 施設ID | BIGINT UNSIGNED |  | ○ |  | `facilities.id` |  |
| 3 | author_type | 記載者種別 | ENUM('producer','admin') |  | ○ |  |  |  |
| 4 | author_id | 記載者ID | BIGINT UNSIGNED |  | ○ |  | `producers.id` or `admins.id` |  |
| 5 | note_text | メモ本文 | TEXT |  | ○ |  |  |  |
| 6 | created_at | 作成日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP |  |  |
| 7 | updated_at | 更新日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP | on update |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| インデックス | producer_notes_facility_idx | facility_id |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | producer_notes_facility_fk | facilities.id 参照 |  |
| リレーション | Note-Producer | producer_notes.author_id ⇔ producers.id (author_type=producer) |  |
| リレーション | Note-Admin | producer_notes.author_id ⇔ admins.id (author_type=admin) |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
