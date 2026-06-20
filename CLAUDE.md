# AI-DLC プロジェクト

## プロジェクト概要

AI 駆動開発ライフサイクル (AI-DLC) の実践リポジトリ。
会計 SaaS (freee 相当) のクローンを題材に、各開発フェーズで AI をどう活用するかを定義・実践する。
ドメインが大きく成長することを前提に DDD + Bounded Context で設計する。

## 技術スタック

| レイヤー | 技術 |
| --- | --- |
| Backend | Ruby on Rails (API モード) |
| Frontend | Next.js (App Router / TypeScript) |
| DB | PostgreSQL |
| アーキテクチャ | DDD + Bounded Context (モジュラーモノリス) |

## 現在のフェーズとアクション

**フェーズ**: Inception
**次のアクション**: ドメイン発見 (Event Storming) → Bounded Context マップ作成

フェーズが進んだらここを更新すること。

## フェーズ別ルール参照先

| フェーズ | ルールファイル |
| --- | --- |
| Inception | `.ai-dlc-rule-details/Inception/` |
| Construction | `.ai-dlc-rule-details/construction/` |
| Extensions | `.ai-dlc-rule-details/extensions/` |
| Operations | `.ai-dlc-rule-details/operations/` |

作業前に現在のフェーズの `00_OVERVIEW.md` を確認すること。

## AI への振る舞いルール

### 基本姿勢

- 実装前に設計を1〜2文で説明し、承認を得てから実装する
- 理解できない要件は実装せず、確認する
- コンテキスト境界 (`app/contexts/[name]/`) を越える変更は必ず事前に相談する

### コーディング

- ビジネスロジックはコントローラーに書かない (集約 / Service に閉じる)
- コメントは WHY のみ。WHAT はコードで表現する
- テストのないコードを生成しない (テスト先行)
- secrets・認証情報をコードに含めない

### コミュニケーション

- 回答は日本語
- 長い説明より短い確認を優先する
- エラーや問題を発見したら実装を止めて報告する

### AI がやらないこと (必ず人間が判断)

- ADR の最終承認
- 本番デプロイの実行
- secrets の管理
- ユーザー影響のある仕様変更の決定
