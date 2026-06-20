# AI コーディングルール (Construction)

## 基本原則

1. **AI はドラフトを書く、人間が承認する**
   - AI 生成コードはそのままマージしない。必ず差分を確認する
   - 理解できないコードはマージしない

2. **コンテキスト境界を守る**
   - AI に「このコンテキストの外を触らないように」と毎回明示する
   - 別コンテキストへの直接参照は ACL (Anti-Corruption Layer) を介す

3. **テストのないコードを AI に書かせない**
   - テストを先に書いてから実装を依頼する

## Rails 実装ルール

### ディレクトリ構成 (モジュラーモノリス)

```bash
app/
  contexts/
    accounting/
      models/          # AR モデル (集約ルート)
      services/        # ユースケース (ApplicationService)
      events/          # ドメインイベント
      repositories/    # (必要な場合)
    invoicing/
    expenses/
    ...
  controllers/
    api/v1/
      accounting/
      invoicing/
```

### AI へ渡す実装指示テンプレート

```bash
## 実装依頼

**コンテキスト**: [accounting / invoicing / ...]
**集約**: [Journal / Invoice / ...]
**ユースケース**: [CreateJournal / ...]

**仕様**:
- [箇条書きで業務ルール]

**制約**:
- app/contexts/[context]/ 以下のみ触ること
- AR の validates でバリデーション
- Service はトランザクションを持つ
- 外部コンテキストには ID 参照のみ

**テスト**: [添付の RSpec を通過させること]
```

## Next.js 実装ルール

### コンポーネント命名

```bash
app/
  (features)/
    [context]/           # accounting, invoicing など
      [feature]/
        page.tsx          # Server Component
        _components/      # このページ専用コンポーネント
  components/
    ui/                  # 汎用 UI コンポーネント
```

### AI への指示で必ず含めること

- `'use client'` が必要かどうかの判断基準を伝える
- Zod スキーマで型を共有する
- エラーハンドリングの方針 (Error Boundary / toast)

## 禁止事項

- [ ] AI に secrets / 環境変数を含むコードを生成させない
- [ ] `eval()` / `send()` を使ったコードを AI に生成させない
- [ ] SQL を直書きするコード (ActiveRecord スコープを使う)
- [ ] コンテキストを跨ぐ AR アソシエーション (`has_many :through` で別コンテキスト)
