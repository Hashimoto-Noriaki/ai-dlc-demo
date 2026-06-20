# ADR テンプレート (Architecture Decision Record)

## ファイル命名規則

```
docs/adr/ADR-NNNN-[kebab-case-title].md
例: docs/adr/ADR-0001-use-rails-modular-monolith.md
```

## テンプレート

```markdown
# ADR-NNNN: [タイトル]

- **ステータス**: Proposed | Accepted | Deprecated | Superseded by ADR-XXXX
- **日付**: YYYY-MM-DD
- **決定者**: [名前]

## コンテキスト

[なぜこの決定が必要か。現状の問題や制約を記述する]

## 検討した選択肢

| 選択肢 | メリット | デメリット |
|--------|---------|-----------|
| A      |         |           |
| B      |         |           |

## 決定

[何を選んだか、1行で]

## 理由

[なぜこの選択肢を選んだか。トレードオフを含めて記述]

## 結果

[この決定によって生じる制約・影響]
```

## 必須 ADR 一覧 (Inception で決める)

- [ ] ADR-0001: モノレポ vs マルチレポ
- [ ] ADR-0002: Rails モジュラーモノリス構成
- [ ] ADR-0003: API 設計方針 (REST / GraphQL)
- [ ] ADR-0004: 認証方式
- [ ] ADR-0005: フロントエンド状態管理
- [ ] ADR-0006: DB スキーマ分離戦略 (Bounded Context 単位)
- [ ] ADR-0007: イベント連携方式 (Sync / Async)
