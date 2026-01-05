# monthly_aggregate_values テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-24 / 最新版リンク: 相対パス

## ドキュメント目的
- 全国/地区別の月次集計値を保持し、出力・差分検知・グラフ用途に利用する。
- `monthly_records` / `monthly_record_change_logs` 更新時に再集計し、版管理で履歴を残す。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | monthly_aggregate_values |  |
| テーブル論理名 | 月次集計値 |  |
| ドメイン/コンテキスト | Aggregation |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 集計値ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | bird_species_id | 鳥種ID | BIGINT UNSIGNED |  | ○ |  | `bird_species.id` |  |
| 3 | region_block_id | 地域ブロックID | BIGINT UNSIGNED |  |  |  | `region_blocks.id`。全国はNULL。成鶏は全国のみ。 |  |
| 4 | target_month | 対象月 | DATE |  | ○ |  | 月初(YYYY-MM-01) |  |
| 5 | data_kind | データ区分 | ENUM('actual','plan') |  | ○ |  | 実績/予定 |  |
| 6 | metric_key | 項目キー | VARCHAR(100) |  | ○ |  | `chick_incoming_count` / `live_processed_count` / `live_processed_weight_kg` / `prohibited_count` / `full_disposal_count` / `partial_disposal_count` |  |
| 7 | value_integer | 集計値 | BIGINT |  | ○ | 0 | 単位は羽・kgのまま保持する。 |  |
| 8 | aggregate_version_no | 集計版番号 | INT UNSIGNED |  | ○ | 1 | 変更がある場合のみ +1 |  |
| 9 | source_updated_at_max | 元データ最終更新 | TIMESTAMP(0) |  |  |  | `monthly_records.updated_at` / `monthly_record_change_logs.changed_at` の最大値 |  |
| 10 | calculated_at | 集計日時 | TIMESTAMP(0) |  | ○ | CURRENT_TIMESTAMP | 再集計実行時刻 |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| ユニーク | monthly_aggregate_values_unique | bird_species_id, region_block_id, target_month, data_kind, metric_key, aggregate_version_no | ○ | 版管理 | region_block_idはNULL許容 |
| インデックス | monthly_aggregate_values_latest_idx | bird_species_id, region_block_id, target_month, data_kind, metric_key |  | 最新版検索 |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | monthly_aggregate_values_species_fk | bird_species.id 参照 |  |
| 外部キー | monthly_aggregate_values_region_fk | region_blocks.id 参照 (NULL可) |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-24 | v0.1 | 初版作成 | Codex |
