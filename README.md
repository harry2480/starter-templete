# Product Starter

Claude Code への指示だけでプロダクトを構築できるスターターキットです。

## ハーネスエンジニアリングとは

このスターターキットは、**ハーネスエンジニアリング**の考え方に基づいて設計されています。

ハーネスエンジニアリングとは、AIエージェントが正しく力を発揮できるように情報やルールを整えることを指します。CLAUDE.md による共通ルールの注入、Skills（スラッシュコマンド）による定型作業の標準化、dependency-cruiser による依存方向の機械的な検証など、**複数のガードレールを多重に敷くことで、AIが書くコードの品質を構造的に担保**します。

これにより、Claude Code を複数セッション並列で回しても、設計が崩れにくい開発が可能になります。

詳しい背景と実践事例については、以下の記事をご覧ください。

> [Claude Code Webを10並列で回す！超並列LLMコーディングを実現するためのハーネスエンジニアリング](https://note.com/jujunjun110/n/n66306cab294a) — Jun Ito

### このスターターキットに組み込まれたガードレール

| ガードレール | 仕組み |
|---|---|
| **設計ルールの注入** | CLAUDE.md + docs/ に DDD 4層・命名規約・依存ルールを明文化。全セッションが同じルールで動く |
| **Skills（スラッシュコマンド）** | `/add-feature`, `/db-table`, `/add-page` など定型作業をコマンド化し、品質のばらつきを抑制 |
| **依存方向の機械的検証** | dependency-cruiser で「domain は外部に依存しない」等のルールを CI で自動チェック |
| **レイヤー別テスト戦略** | domain/application は Unit テスト、infrastructure は Integration テスト。テスト方針もドキュメント化 |
| **品質チェックの自動化** | `pnpm verify` で lint → typecheck → test → depcruise を一括実行 |

## 技術スタック

- Next.js 15 (App Router) + Vercel
- Supabase PostgreSQL + Prisma
- shadcn/ui + Tailwind CSS
- vitest + dependency-cruiser
- Biome (lint/format)

## はじめかた

### セットアップ

Claude Code で `/init-pj` を実行してください。前提ツールのインストールからDB構築まで自動で行います。

## 使い方

Claude Code に自然言語で指示するだけで機能を追加できます。

```
「ユーザー管理機能を作って」
「お気に入り機能を追加して」
「/articles ページを作って」
「Stripe決済と連携して」
「○○テーブルにstatusカラムを追加して」
「このエラーを直して: [エラーメッセージ]」
```

## コマンド一覧

| コマンド | 内容 |
|---|---|
| `pnpm dev` | 開発サーバー起動 |
| `pnpm verify` | 品質チェック（lint → typecheck → test → depcruise） |
| `pnpm test:unit` | Unit テスト |
| `pnpm lint:fix` | 自動フォーマット |
| `pnpm db:migrate` | DBマイグレーション |
| `pnpm knip` | 未使用コード検出 |

## プロジェクト構成

```
apps/webapp/src/
├── app/                    # ページ（Next.js App Router）
├── backend/
│   ├── domain/             # ビジネスルール（モデル、インターフェース）
│   ├── application/        # ユースケース
│   ├── infrastructure/     # DB・外部API実装
│   └── presentation/       # DI組み立て、データ取得、Server Actions
└── frontend/
    └── components/         # UIコンポーネント
```

## サンプル実装について

初期状態では Claude API を使ったジョーク生成機能がサンプルとして含まれています。
`ANTHROPIC_API_KEY` を設定すると API 経由で動作し、未設定の場合は Stub（固定値）で動作します。

```bash
echo 'ANTHROPIC_API_KEY="your-api-key"' >> apps/webapp/.env.local
```
