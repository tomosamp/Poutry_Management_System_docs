# export_templates テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 関係機関提出フォーマットをマスタ管理し、出力・ハイライト仕様を統一する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | export_templates |  |
| テーブル論理名 | 提出先テンプレート |  |
| ドメイン/コンテキスト | Export |  |
| 主キー | code（VARCHAR） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | code | テンプレコード | VARCHAR(50) | ○ | ○ |  | `ALIC_BR_NATIONAL` 等 |  |
| 2 | name | テンプレ名 | VARCHAR(100) |  | ○ |  | 表示用名称 |  |
| 3 | output_format | 出力形式 | ENUM('excel','pdf') |  | ○ |  |  |  |
| 4 | bird_species_code | 鳥種コード | VARCHAR(20) |  | ○ |  | `bird_species.code` |  |
| 5 | layout_definition_json | レイアウト定義 | JSON |  | ○ |  | 列マッピング・ハイライト仕様 |  |
| 6 | is_active | 有効フラグ | TINYINT(1) |  | ○ | 1 |  |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | code | ○ | コード一意 |  |
| 外部キー | export_templates_species_code_foreign | bird_species_code |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | export_templates_species_code_foreign | bird_species.code 参照 |  |
| リレーション | Template-Export | export_templates.code ⇔ agency_exports.template_code |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |
