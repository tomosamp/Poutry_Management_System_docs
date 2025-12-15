# bird_species テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 鳥種マスタを定義し、全機能で統一コードを利用する。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | bird_species |  |
| テーブル論理名 | 鳥種マスタ |  |
| ドメイン/コンテキスト | Master |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 鳥種ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT | 3件固定 |  |
| 2 | code | 鳥種コード | VARCHAR(20) |  | ○ |  | 例: BR/SC/JG | UNIQUE |
| 3 | name | 鳥種名 | VARCHAR(50) |  | ○ |  | 表示名 |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| ユニーク | bird_species_code_unique | code | ○ | コード照合 |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | - |  | - |
| リレーション | Species-Facility | bird_species.id ⇔ facilities.bird_species_id |  |
| リレーション | Species-Monthly | bird_species.id ⇔ monthly_records.bird_species_id |  |
| リレーション | Species-Template | bird_species.code ⇔ export_templates.bird_species_code |  |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |

