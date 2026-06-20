# AI-DLC コアルール

このプロジェクトは AI 駆動開発ライフサイクル (AI-DLC) に従って開発される。
以下のルールはフェーズによらず常に適用される。

## 現在のフェーズ

**Inception** — `.ai-dlc-rule-details/Inception/` のルールを参照

## フェーズ別ルールの参照先

| フェーズ | ルール |
|---------|--------|
| Inception | `.ai-dlc-rule-details/Inception/` |
| Construction | `.ai-dlc-rule-details/construction/` |
| Extensions | `.ai-dlc-rule-details/extensions/` |
| Operations | `.ai-dlc-rule-details/operations/` |

## AI の役割と権限

### AI がやること

- コードのドラフト生成
- テストの生成
- ドキュメントの生成・更新
- コードレビュー (`/code-review`)
- バグの原因候補提示
- リファクタリング提案

### AI がやらないこと (人間が判断する)

- アーキテクチャの最終決定
- ADR の承認
- 本番へのデプロイ実行
- secrets / 認証情報の管理
- ユーザー影響のある仕様変更の決定

## コーディング規約

### 言語・フレームワーク

- Backend: Ruby on Rails (API モード)
- Frontend: Next.js (App Router / TypeScript)
- DB: PostgreSQL

### 設計原則

- DDD (Domain-Driven Design) を採用
- Rails はモジュラーモノリス構成 (`app/contexts/[context_name]/`)
- Bounded Context を越える AR アソシエーションは禁止
- ビジネスロジックはコントローラーに書かない

### 命名規則

- Ruby: snake_case (Rails 標準)
- TypeScript: camelCase (変数/関数), PascalCase (コンポーネント/型)
- DB テーブル: `[context]_[plural_noun]` (例: `accounting_journals`)
- ブランチ: `feature/[context]-[feature]`

## コミュニケーション

- 日本語で回答する
- コードコメントは原則書かない (書く場合は WHY のみ、日本語可)
- 実装前に設計を簡潔に説明し、承認を得てから実装する
