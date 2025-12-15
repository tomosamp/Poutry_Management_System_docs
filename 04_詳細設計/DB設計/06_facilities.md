# facilities テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 施設情報を鳥種単位で管理し、生産者アカウント・月次データの基点とする。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | facilities |  |
| テーブル論理名 | 施設 |  |
| ドメイン/コンテキスト | Producer |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 施設ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | bird_species_id | 鳥種ID | BIGINT UNSIGNED |  | ○ |  | `bird_species.id` | 同施設で複数鳥種を持たない |
| 3 | region_block_id | 地区ID | BIGINT UNSIGNED |  |  |  | 成鶏はNULL可 |  |
| 4 | prefecture_id | 都道府県ID | BIGINT UNSIGNED |  | ○ |  | `prefectures.id` |  |
| 5 | name | 施設名 | VARCHAR(100) |  | ○ |  |  |  |
| 6 | name_kana | 施設名カナ | VARCHAR(150) |  | ○ |  |  |  |
| 7 | postal_code | 郵便番号 | VARCHAR(8) |  | ○ |  | `NNN-NNNN` または `NNNNNNN` |  |
| 8 | city | 市区町村 | VARCHAR(100) |  | ○ |  |  | 画面入力に合わせて分離保持 |
| 9 | address_line | 番地 | VARCHAR(100) |  | ○ |  |  | 画面入力に合わせて分離保持 |
| 10 | business_type | 事業者種別 | ENUM('company','individual','family') |  |  |  | 企業/個人/家族経営 |  |
| 11 | closed_at | 廃業日 | DATE |  |  |  |  |  |
| 12 | merge_or_close_memo | 合併/廃業メモ | TEXT |  |  |  | 任意 |  |
| 13 | sort_order | 並び順 | INT |  |  | 0 | 画面表示用の並び順 | ブロ/地鶏は地区内、成鶏は鳥種内で運用。DB制約なし |
| 14 | merged_into_facility_id | 合併先施設ID | BIGINT UNSIGNED |  |  |  | 合併後の参照先 | self FK |
| 15 | merged_at | 合併日時 | TIMESTAMP(0) |  |  |  |  |  |
| 16 | deleted_at | 論理削除 | TIMESTAMP(0) |  |  |  |  |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| インデックス | facilities_region_sort_idx | region_block_id, sort_order |  | 地区内ソート用（任意） | ユニーク制約なし |
| インデックス | facilities_species_sort_idx | bird_species_id, sort_order |  | 成鶏ソート用（任意） | ユニーク制約なし |
| インデックス | facilities_prefecture_idx | prefecture_id |  | 都道府県検索（任意） |  |
| 外部キー | facilities_species_fk | bird_species_id |  |  |  |
| 外部キー | facilities_region_fk | region_block_id |  |  | 成鶏はNULL可 |
| 外部キー | facilities_prefecture_fk | prefecture_id |  |  |  |
| 外部キー | facilities_merged_fk | merged_into_facility_id |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | facilities_species_fk | bird_species.id 参照 |  |
| 外部キー | facilities_region_fk | region_blocks.id 参照 | NULL許容 |
| 外部キー | facilities_prefecture_fk | prefectures.id 参照 |  |
| 外部キー | facilities_merged_fk | facilities.id 自己参照 |  |
| リレーション | Facility-Producer | facilities.id ⇔ producers.facility_id |  |
| リレーション | Facility-Monthly | facilities.id ⇔ monthly_records.facility_id |  |
| リレーション | Facility-MergeHistory | facilities.id ⇔ facility_merge_histories.{source,target} |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
