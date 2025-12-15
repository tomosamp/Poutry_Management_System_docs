# region_blocks テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 地区ブロックA〜Fを管理し、地区別集計・提出で使用する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | region_blocks |  |
| テーブル論理名 | 地区ブロック |  |
| ドメイン/コンテキスト | Master |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 地区ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | code | 地区コード | CHAR(1) |  | ○ |  | a〜f | UNIQUE |
| 3 | name | 地区名 | VARCHAR(50) |  | ○ |  | 例: 北海道・東北地区 |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| ユニーク | region_blocks_code_unique | code | ○ | コード照合 |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| リレーション | Region-Pref | region_blocks.id ⇔ prefectures.region_block_id |  |
| リレーション | Region-Facility | region_blocks.id ⇔ facilities.region_block_id | 成鶏はNULL可 |
| リレーション | Region-Aggregation | region_blocks.code ⇔ export_templates(aggregation_scope=district) | 提出時 |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |

