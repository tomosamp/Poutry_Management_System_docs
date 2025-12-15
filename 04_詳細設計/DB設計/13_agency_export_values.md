# agency_export_values テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 提出スナップショットを正規化して保持し、提出先別の列マッピングに対応する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | agency_export_values |  |
| テーブル論理名 | 提出スナップショット値 |  |
| ドメイン/コンテキスト | Export |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 値ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | agency_export_id | スナップショットID | BIGINT UNSIGNED |  | ○ |  | `agency_exports.id` |  |
| 3 | metric_key | 項目キー | VARCHAR(100) |  | ○ |  | テンプレの列名やmetricコード |  |
| 4 | value_decimal | 数値 | DECIMAL(18,4) |  |  |  | 数値系を格納 | 精度未確定 |
| 5 | value_text | テキスト値 | TEXT |  |  |  | 文字列系を格納 |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| インデックス | agency_export_values_export_idx | agency_export_id |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | agency_export_values_export_fk | agency_exports.id 参照 |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |

