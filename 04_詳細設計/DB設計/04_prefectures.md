# prefectures テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 都道府県マスタを管理し、地域ブロック・施設住所で参照する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | prefectures |  |
| テーブル論理名 | 都道府県 |  |
| ドメイン/コンテキスト | Master |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 都道府県ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | code | 都道府県コード | VARCHAR(10) |  | ○ |  | JISコード想定 | UNIQUE |
| 3 | name | 都道府県名 | VARCHAR(50) |  | ○ |  |  |  |
| 4 | region_block_id | 地区ID | BIGINT UNSIGNED |  | ○ |  | `region_blocks.id` | 成鶏は施設側でNULL可 |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| ユニーク | prefectures_code_unique | code | ○ | コード照合 |  |
| 外部キー | prefectures_region_block_id_foreign | region_block_id |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | prefectures_region_block_id_foreign | region_blocks.id 参照 |  |
| リレーション | Pref-Facility | prefectures.id ⇔ facilities.prefecture_id(予定) | 住所拡張時 |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |

