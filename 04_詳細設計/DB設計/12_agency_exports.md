# agency_exports テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 関係機関提出時のスナップショットを版管理し、再提出時も過去値を残す。
- 月次/年次の違いは `period_type` で表現し、年次出力も「1回の出力＝1スナップショット」として扱う。
- 出力元は `monthly_aggregate_values` の最新集計値を前提とする。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | agency_exports |  |
| テーブル論理名 | 提出スナップショット |  |
| ドメイン/コンテキスト | Export |  |
| 主キー | id（推奨複合: facility_id + bird_species_id + period_type + target_month + template_code + export_version_no） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | スナップショットID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | facility_id | 施設ID | BIGINT UNSIGNED |  |  |  | 全国/地区集計など施設に紐づかない出力はNULL |  |
| 3 | bird_species_id | 鳥種ID | BIGINT UNSIGNED |  | ○ |  |  |  |
| 4 | period_type | 期間区分 | ENUM('month','year') |  | ○ | 'month' | 月次/年次の違い。年次出力は1年分（12ヶ月）を含むスナップショットとして扱う。 |  |
| 5 | target_month | 対象月（期間キー） | DATE |  | ○ |  | `period_type='month'` は対象月（月初）。`period_type='year'` は対象年度の開始月（月初、例: `2025-04-01`）。 | 月初(YYYY-MM-01) |
| 6 | template_code | テンプレコード | VARCHAR(50) |  | ○ |  | `export_templates.code` |  |
| 7 | export_version_no | 提出版 | INT UNSIGNED |  | ○ | 1 | 再出力ごとに+1 |  |
| 8 | source_record_id | 元レコードID | BIGINT UNSIGNED |  |  |  | 元データ参照（任意） | 年次出力や集計出力ではNULL |
| 9 | source_record_version_no | 元版番号 | INT UNSIGNED |  |  |  | 差分ハイライト用（任意） | 年次出力や集計出力ではNULL |
| 10 | payload_json | 提出データJSON | JSON |  | ○ |  | 出力時の値スナップショット（年次は12ヶ月分の系列/提出月/ハイライト対象月を含む） |  |
| 11 | exported_at | 出力日時 | TIMESTAMP(0) |  | ○ | CURRENT_TIMESTAMP |  |  |
| 12 | exported_by_admin_id | 出力管理者ID | BIGINT UNSIGNED |  | ○ |  | `admins.id` |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| ユニーク | agency_exports_unique | facility_id, bird_species_id, period_type, target_month, template_code, export_version_no | ○ | 版管理 | facility_idはNULL許容 |
| インデックス | agency_exports_month_idx | bird_species_id, target_month |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | agency_exports_facility_fk | facilities.id 参照 (NULL可) |  |
| 外部キー | agency_exports_species_fk | bird_species.id 参照 |  |
| 外部キー | agency_exports_template_fk | export_templates.code 参照 |  |
| 外部キー | agency_exports_admin_fk | admins.id 参照 |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
