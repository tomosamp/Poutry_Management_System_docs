# monthly_records テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 施設×月×鳥種の元データをワイド形式で保持し、実績/予定・差分管理を可能にする。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | monthly_records |  |
| テーブル論理名 | 月次元データ |  |
| ドメイン/コンテキスト | Submission |  |
| 主キー | id（推奨複合: facility_id + bird_species_id + target_month + version_no） | 自動採番＋ユニーク制約で担保 |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | レコードID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | facility_id | 施設ID | BIGINT UNSIGNED |  | ○ |  | `facilities.id` |  |
| 3 | bird_species_id | 鳥種ID | BIGINT UNSIGNED |  | ○ |  | `bird_species.id` |  |
| 4 | target_month | 対象月 | DATE |  | ○ |  | 月初(YYYY-MM-01) |  |
| 5 | version_no | 版番号 | INT UNSIGNED |  | ○ | 1 | 保存/更新ごとに+1 |  |
| 6 | data_kind | データ区分 | ENUM('actual','plan') |  | ○ |  | 実績/予定 |  |
| 7 | input_method | 入力経路 | ENUM('csv','manual') |  | ○ |  |  |  |
| 8 | registered_by_admin_id | 登録管理者ID | BIGINT UNSIGNED |  |  |  | 当面admin |  |
| 9 | registered_by_producer_id | 登録生産者ID | BIGINT UNSIGNED |  |  |  | 将来拡張 |  |
| 10 | input_timestamp | 取込タイムスタンプ | TIMESTAMP(0) |  |  |  | CSVの `MM/DD/YYYY HH:MM:SS` を変換して保存。手入力時はNULL | 最新重複判断に利用 |
| 11 | source_email_raw | CSVメールアドレス（生） | VARCHAR(255) |  |  |  | CSVで受領した文字列をそのまま保持（正規化しない）。手入力時はNULL。 | 履歴追跡用 |
| 12 | note_text | 月次メモ | TEXT |  |  |  | 手入力時に職員が入力する「月次メモ（全体メモ）」。空欄はNULLを許容。 | 関係機関向け出力には含めない |
| 13 | chick_incoming_count | ひな入雛羽数 | BIGINT |  |  | 0 | 単位は羽（整数）で保持。画面入力も羽（整数）。 | ブロイラーのみ使用 |
| 14 | live_processed_count | 生体処理羽数 | BIGINT |  |  | 0 | 単位は羽（整数）で保持。画面入力も羽（整数）。 | 全鳥種で使用 |
| 15 | live_processed_weight_kg | 生体処理重量(kg) | BIGINT |  |  | 0 | 単位はkg（整数）で保持。CSVがkgの場合はそのまま、画面入力がトンの場合は `トン × 1000` で換算する。 | ブロ・地鶏で使用 |
| 16 | prohibited_count | 禁止羽数 | BIGINT |  |  | 0 | 単位は羽（整数）。 | 全鳥種で使用（先月実績のみ） |
| 17 | full_disposal_count | 全部廃棄羽数 | BIGINT |  |  | 0 | 単位は羽（整数）。 | 全鳥種で使用（先月実績のみ） |
| 18 | partial_disposal_count | 一部廃棄羽数 | BIGINT |  |  | 0 | 単位は羽（整数）。 | 全鳥種で使用（先月実績のみ） |
| 19 | comment_text | コメント | TEXT |  |  |  | CSV提出時の生産者コメント（手入力では未使用/NULL）。 |  |
| 20 | created_at | 作成日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP |  |  |
| 21 | updated_at | 更新日時 | TIMESTAMP(0) |  |  | CURRENT_TIMESTAMP | on update |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| ユニーク | monthly_records_unique | facility_id, bird_species_id, target_month, version_no | ○ | 版管理 |  |
| インデックス | monthly_records_month_idx | facility_id, target_month |  | 月次検索 |  |
| インデックス | monthly_records_species_idx | bird_species_id, target_month |  | 種別検索 |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | monthly_records_facility_fk | facilities.id 参照 |  |
| 外部キー | monthly_records_species_fk | bird_species.id 参照 |  |
| 外部キー | monthly_records_admin_fk | admins.id 参照 | NULL可 |
| 外部キー | monthly_records_producer_fk | producers.id 参照 | NULL可 |
| リレーション | Monthly-Changes | monthly_records.id ⇔ monthly_record_change_logs.record_id |  |
| リレーション | Monthly-Exports | monthly_records.id ⇔ agency_exports.source_record_id | 任意（facility単位の月次スナップショットに紐付ける場合のみ） |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
