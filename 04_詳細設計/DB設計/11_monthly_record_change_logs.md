# monthly_record_change_logs テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 月次データの差分をカラム単位で記録し、提出時の変更箇所可視化に利用する。

## 変更ログの基本方針（手入力）
- 手入力（`monthly_records.input_method='manual'`）では、必要に応じて項目単位のメモを `change_memo` として残せる。
- 値の変更が無くても「メモだけ」を残す運用を許容する。
  - この場合、`before_value` と `after_value` は同値（文字列化）で保存する。
- 新規登録（前版が存在しない）でも記録可能とし、その場合の `before_value` は NULL（または空）として扱う。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | monthly_record_change_logs |  |
| テーブル論理名 | 月次差分ログ |  |
| ドメイン/コンテキスト | Submission |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | ログID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | record_id | 月次レコードID | BIGINT UNSIGNED |  | ○ |  | `monthly_records.id` |  |
| 3 | field_name | 項目名 | VARCHAR(100) |  | ○ |  | 変更対象カラム名 |  |
| 4 | before_value | 変更前 | TEXT |  |  |  | 型は文字列化して保存 |  |
| 5 | after_value | 変更後 | TEXT |  |  |  | 同上 |  |
| 6 | change_memo | 変更メモ | TEXT |  |  |  | 手入力時のみ |  |
| 7 | changed_by_type | 変更者種別 | ENUM('admin','producer','applicant') |  | ○ |  |  |  |
| 8 | changed_by_id | 変更者ID | BIGINT UNSIGNED |  | ○ |  | 種別に応じて参照 |  |
| 9 | changed_at | 変更日時 | TIMESTAMP(0) |  | ○ | CURRENT_TIMESTAMP |  |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| インデックス | change_logs_record_idx | record_id, changed_at |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | change_logs_record_fk | monthly_records.id 参照 |  |
| リレーション | Change-Exporter | monthly_record_change_logs.record_id ⇔ agency_exports.source_record_id | 任意（facility単位の月次スナップショットに紐付ける場合のみ） |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
