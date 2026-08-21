# ブログサービス要件

[한국어](03-requirements.md) | [English](03-requirements.en.md) | [日本語](03-requirements.ja.md)

← 前へ: [02. ブログサービスのスコープと優先順位](02-scope.ja.md)

## 適用

| 項目 | 値 |
| --- | --- |
| 基準 | Stage 01の製品目標、Stage 02の初回リリース範囲 |
| 対象 | Stage 03要件文書一式の全機能・非機能要件 |
| 優先順位 | すべて必須(Mandatory)、選択可能な下位優先順位なし |

## 役割

| 役割 | 識別 | 許可範囲 |
| --- | --- | --- |
| 訪問者(Visitor) | ログインの有無を問わない | 公開ポストの一覧・詳細閲覧。未ログイン状態では会員登録・ログイン以外の会員専用操作を禁止 |
| 会員(Member) | 一意のメールアドレス・パスワードで登録完了、現在ログイン状態 | 訪問者と同じ公開閲覧。ログイン中のポスト作成、自分の既存公開ポストの変更・完全削除、ログアウト |

## 要件文書

1. [アカウント要件](requirements/01-account.ja.md)
2. [ポスト要件](requirements/02-posts.ja.md)
3. [認可要件](requirements/03-authorization.ja.md)
4. [品質要件](requirements/04-quality.ja.md)

## 要件索引

| 要件ID | 機能・品質 | 役割・対象 | 優先順位 |
| --- | --- | --- | --- |
| `FR-AUTH-001` | [会員登録](requirements/01-account/FR-AUTH-001.ja.md) | 訪問者 | 必須(Mandatory) |
| `FR-AUTH-002` | [ログイン](requirements/01-account/FR-AUTH-002.ja.md) | 訪問者 | 必須(Mandatory) |
| `FR-AUTH-003` | [ログアウト](requirements/01-account/FR-AUTH-003.ja.md) | 会員 | 必須(Mandatory) |
| `FR-POST-001` | [ポスト作成](requirements/02-posts/FR-POST-001.ja.md) | 会員 | 必須(Mandatory) |
| `FR-POST-002` | [公開ポスト一覧閲覧](requirements/02-posts/FR-POST-002.ja.md) | 訪問者、会員 | 必須(Mandatory) |
| `FR-POST-003` | [公開ポスト詳細閲覧](requirements/02-posts/FR-POST-003.ja.md) | 訪問者、会員 | 必須(Mandatory) |
| `FR-POST-004` | [ポスト変更](requirements/02-posts/FR-POST-004.ja.md) | 会員 | 必須(Mandatory) |
| `FR-POST-005` | [ポスト削除](requirements/02-posts/FR-POST-005.ja.md) | 会員 | 必須(Mandatory) |
| `FR-AUTHZ-001` | [所有権・権限制限](requirements/03-authorization/FR-AUTHZ-001.ja.md) | 訪問者、会員 | 必須(Mandatory) |
| `NFR-PERF-001` | [初回リリース機能の応答時間](requirements/04-quality/NFR-PERF-001.ja.md) | 主要操作 | 必須(Mandatory) |
| `NFR-AVAIL-001` | [初回リリース機能の月間可用性](requirements/04-quality/NFR-AVAIL-001.ja.md) | 主要操作 | 必須(Mandatory) |

## Stage境界および保留決定

| Stage | 所有する決定 | 固定条件 |
| --- | --- | --- |
| Stage 02 | 除外範囲 | 明記された除外機能を維持、Stage 03要件文書一式による追加禁止 |
| Stage 04 | 正常・代替・失敗フローの順序、ユーザー・システム相互作用、境界状況 | Stage 03要件文書一式の要件変更禁止 |
| Stage 05 | 予想利用量、トラフィック構成、データ増加量、品質目標の測定条件、同時ユーザー100人の操作構成・測定環境・負荷算定根拠 | 同時ユーザー100人、p95 2秒以下、月間可用性99.5%以上の変更禁止 |
| Stage 08 | 要求・応答構造、API形式、エラー形式・コード、公開一覧の正確な後続結果閲覧方式 | 公開一覧は結果ごとに最大20件、当初の作成日時の降順、すべての対象ポストを後続結果で閲覧可能 |
| Stage 09 | アーキテクチャ、モジュール責務 | 本文書では未選択 |
| Stage 10 | データベーススキーマ、整合性規則、完全削除の保存・除去方式 | 本文書では未選択 |
| Stage 12 | 認証、セッション、パスワード保護、認可の具体的セキュリティメカニズム | 本文書では未選択 |

| 保留決定 | 状態 |
| --- | --- |
| HTTP方式 | 未選択 |
| APIスキーマ | 未選択 |
| ステータス・エラーコード | 未選択、Stage 08 |
| 認証トークン・セッション方式 | 未選択、Stage 12 |
| パスワードハッシュ方式 | 未選択、Stage 12 |
| データベース構造 | 未選択、Stage 10 |
| 完全削除の保存・除去方式 | 未選択、Stage 10 |
| アーキテクチャ | 未選択、Stage 09 |
| UI | 未選択 |
| 公開一覧の正確な後続閲覧方式 | 未選択、Stage 08 |
