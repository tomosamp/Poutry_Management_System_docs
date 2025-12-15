# facility_merge_histories テーブル詳細設計
フェーズ: 1 / 状態: 現行 / 最終更新: 2025-12-11 / 最新版リンク: 相対パス

## ドキュメント目的
- 施設（生産者）合併の履歴を保持し、再追跡可能にする。

## 1. 基本情報
| 項目 | 内容 | 備考 |
|---|---|---|
| テーブル物理名 | facility_merge_histories |  |
| テーブル論理名 | 施設合併履歴 |  |
| ドメイン/コンテキスト | Producer |  |
| 主キー | id（BIGINT UNSIGNED AUTO_INCREMENT） |  |

## 2. カラム定義
| No | カラム名 | 論理名 | 型 | PK | Not Null | デフォルト | 説明/業務ルール | 備考 |
|---|---|---|---|---|---|---|---|---|
| 1 | id | 履歴ID | BIGINT UNSIGNED | ○ | ○ | AUTO INCREMENT |  |  |
| 2 | source_facility_id | 旧施設ID | BIGINT UNSIGNED |  | ○ |  | 合併で消滅 |  |
| 3 | target_facility_id | 存続施設ID | BIGINT UNSIGNED |  | ○ |  | 合併後に残る |  |
| 4 | merged_at | 合併日時 | TIMESTAMP(0) |  | ○ |  |  |  |
| 5 | merged_by_admin_id | 実行管理者ID | BIGINT UNSIGNED |  | ○ |  | `admins.id` |  |
| 6 | reason | 合併理由 | TEXT |  |  |  | 任意入力 |  |

## 3. インデックス・キー設計
| 種別 | 名称 | 対象カラム | ユニーク | 用途/目的 | 備考 |
|---|---|---|---|---|---|
| 主キー | PRIMARY | id | ○ |  |  |
| インデックス | facility_merge_source_idx | source_facility_id |  |  |  |
| インデックス | facility_merge_target_idx | target_facility_id |  |  |  |

## 4. 制約・リレーション
| 種別 | 名称 | 内容 | 備考 |
|---|---|---|---|
| 外部キー | facility_merge_source_fk | facilities.id 参照 |  |
| 外部キー | facility_merge_target_fk | facilities.id 参照 |  |
| 外部キー | facility_merge_admin_fk | admins.id 参照 |  |
| リレーション | Merge-Facility | facilities.merged_into_facility_id ⇔ facility_merge_histories.target_facility_id | 運用で同期 |

## 改定履歴
| 改定日 | 版数 | 変更概要 | 担当 |
|---|---|---|---|
| 2025-12-11 | v0.1 | 初版作成 | Codex |

