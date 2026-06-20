# CI/CD・監視ルール (Operations)

## CI/CD パイプライン構成

### GitHub Actions ワークフロー

```
PR 作成時:
  ├─ lint (RuboCop / ESLint)
  ├─ typecheck (tsc --noEmit)
  ├─ test (RSpec / Jest)
  ├─ security (brakeman / bundle audit)
  └─ /code-review (AI レビュー) ← オプション

main マージ時:
  ├─ 上記全て
  ├─ build (Docker image)
  └─ deploy to staging

タグ (v*) プッシュ時:
  └─ deploy to production
```

### AI による CI 設定生成ルール

```
プロンプトテンプレート:
「Rails [version] + Next.js [version] 構成の GitHub Actions ワークフローを作成してください。
要件:
- PR: lint / test / security を並列実行
- main: staging 自動デプロイ
- secrets は ${{ secrets.XXX }} で参照 (値は含めない)
- キャッシュ: bundler / npm / Docker layer
- 失敗時は Slack 通知」
```

**注意**: AI が生成したワークフローに secrets の値が含まれていないか必ず確認する。

## デプロイ戦略

### ゼロダウンタイムデプロイ

```
1. マイグレーション先行 (後方互換 DDL のみ)
2. アプリデプロイ (Rolling update)
3. 後片付けマイグレーション (カラム削除等) は次リリース以降
```

### フィーチャーフラグ

大きな機能変更はフィーチャーフラグで段階リリース:

```ruby
if FeatureFlag.enabled?(:new_journal_ui, current_user)
  # 新実装
else
  # 旧実装
end
```

## 監視

### 必須メトリクス

| メトリクス | 警告閾値 | 重大閾値 |
|-----------|---------|---------|
| API レイテンシ (p95) | 200ms | 500ms |
| エラーレート (5xx) | 0.1% | 1% |
| DB コネクション使用率 | 70% | 90% |
| ジョブキュー長 | 100 | 500 |

### ログ戦略

```ruby
# 構造化ログ (JSON) を標準とする
Rails.logger.info({
  event: "journal_created",
  journal_id: journal.id,
  user_id: current_user.id,
  amount: journal.total_amount
})
```

AI によるログ分析プロンプト:

```
「以下のエラーログを分析し、原因候補と調査手順を提示してください: [ログ貼り付け]」
```

## セキュリティ

### 定期スキャン (毎週 CI で自動実行)

```bash
bundle exec brakeman --no-pager
bundle exec bundler-audit check --update
npm audit --audit-level=high
```

### フィンテック特有の注意点

- ユーザーの金融データへのアクセスは必ずログに残す
- 金額計算は浮動小数点を使わない (`BigDecimal`)
- 外部 API キー (銀行 API 等) は Vault / AWS Secrets Manager で管理

## インシデント対応

### Severity 定義

| Level | 定義 | 対応時間 |
|-------|------|---------|
| P0 | 決済・仕訳が停止 | 即時 (24h) |
| P1 | 主要機能が不安定 | 4時間以内 |
| P2 | 一部機能の問題 | 翌営業日 |

### AI 支援インシデント対応

```
プロンプト例:
「以下のスタックトレースとログを分析してください。
環境: Rails / PostgreSQL / Sidekiq
エラー: [貼り付け]
直近の変更: [デプロイ内容]
原因候補と緊急対応手順を教えてください。」
```

Runbook テンプレートは `docs/runbooks/` を参照。
