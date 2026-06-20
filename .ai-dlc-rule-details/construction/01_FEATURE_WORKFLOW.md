# 機能開発ワークフロー (Construction)

## ブランチ戦略

```bash
main          ─── 本番リリース済み
  └─ develop  ─── 統合ブランチ
       └─ feature/[context]-[feature]  例: feature/accounting-create-journal
       └─ fix/[context]-[description]
```

## 1 機能の開発手順

### Step 1: タスク分解 (AI に依頼)

```bash
プロンプト例:
「[ユーザーストーリー] を Rails + Next.js で実装するための
技術タスクを分解してください。Bounded Context は [context名] です。」
```

### Step 2: DB 設計

- マイグレーションファイルを先に作成する
- テーブル名はコンテキストプレフィックスを付ける (`accounting_journals`)
- 外部キーは同一コンテキスト内のみ。コンテキスト跨ぎは ID 参照のみ

### Step 3: テスト先行 (RSpec)

```ruby
# spec/[context]/[aggregate]_spec.rb を先に書く
# AI プロンプト: 「以下の仕様でRSpecを書いてください: ...」
```

テスト構成:

```bash
spec/
  models/          # 集約・エンティティの単体テスト
  services/        # ユースケースのテスト
  requests/        # API エンドポイントのテスト
  system/          # E2E (Capybara)
```

### Step 4: 実装 (AI コード生成)

```bash
プロンプト例:
「以下の RSpec を通過する Rails の [Model/Service/Controller] を実装してください。
DDD の集約パターンに従い、ビジネスロジックは Model に閉じてください。」
```

### Step 5: フロントエンド (Next.js)

- Server Components をデフォルト。インタラクションがある部分のみ `'use client'`
- データフェッチは Server Actions or Route Handlers
- コンポーネント粒度: Page → Feature → UI の3層

### Step 6: PR 作成・レビュー

```bash
# PR作成後に実行
/code-review medium
```

レビュー観点:

- ドメインロジックが集約に閉じているか
- コンテキスト境界を越える依存がないか
- テストカバレッジが基準を満たすか

## AI コーディングルール詳細

`02_AI_CODING_RULES.md` を参照。
