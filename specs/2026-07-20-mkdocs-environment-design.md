# squbok-guide-v3 MkDocs 環境構築 設計書

- 日付: 2026-07-20
- 作成者: Atsushi Tsuchiya

## 目的

`/Users/tsuchiyaatsushishi/.ghq/gitlab/atsuchiy11/prince2` の MkDocs Material 構成を土台に、
GitHub リポジトリ `atsuchiy11/squbok-guide-v3` 上で SQuBOK ガイドの MkDocs 環境を構築する。
今回作るのは**骨組み(スケルトン)**であり、SQuBOK の各章の中身は後日ユーザー自身が執筆する。

## 決定事項(ブレインストーミング結果)

| 項目 | 決定 |
| --- | --- |
| コンテンツ | SQuBOK 構成の雛形。ただし「まずは骨組みのみ」= Home + 5 大分類のプレースホルダページ |
| ホスティング | GitHub Pages(GitHub Actions でデプロイ) |
| Python 環境管理 | uv + pyproject.toml |
| Python バージョン | 3.14.3(mise のグローバル設定に準拠) |
| テーマ | prince2 と同じ Material 設定を引き継ぎ、GitHub 向けに調整 |

## ディレクトリ構成

```
squbok-guide-v3/
├── .github/workflows/deploy.yml   # GitHub Actions: uv sync → mkdocs gh-deploy
├── docs/
│   ├── index.md                   # Home(プレースホルダ)
│   ├── squbok/
│   │   ├── 01_quality_concepts.md          # ソフトウェア品質の基本概念
│   │   ├── 02_quality_management.md        # ソフトウェア品質マネジメント
│   │   ├── 03_quality_technologies.md      # ソフトウェア品質技術
│   │   ├── 04_specialized_topics.md        # 専門的なソフトウェア品質の概念と技術
│   │   └── 05_application_domains.md        # ソフトウェア品質の応用領域
│   └── assets/images/.gitkeep     # favicon/画像用の空ディレクトリ
├── mkdocs.yml
├── pyproject.toml                 # uv 管理(mkdocs-material 依存)
├── .python-version                # "3.14.3"
├── .gitignore                     # site/, .venv/ など
├── README.md
└── specs/                         # 本設計書(MkDocs 対象外)
```

各 md ファイルは「# 見出し + 一言プレースホルダ」の最小構成とする。
SQuBOK V3 の 5 大分類を骨組みとして採用する。

## mkdocs.yml

prince2 の設定を基本的に引き継ぎ、以下を変更する。

### 引き継ぐ設定
- `theme: name: material` / `language: ja`
- palette(indigo, light + dark のトグル)
- features 一式(navigation.instant, tabs, sections, expand, top, search.suggest/highlight/share, content.code.copy, content.tabs.link など)
- markdown_extensions(toc[permalink], tables, admonition, codehilite, pymdownx.details)
- `plugins: [search]`

### 変更する設定
- `site_name` → `SQuBOK Guide`
- `site_description` → SQuBOK 用の説明文
- `site_author` → `Atsushi Tsuchiya`
- `site_url` → `https://atsuchiy11.github.io/squbok-guide-v3/`
- `copyright` → `Copyright © 2026 Atsushi Tsuchiya`
- `repo_name` → `atsuchiy11/squbok-guide-v3`
- `repo_url` → `https://github.com/atsuchiy11/squbok-guide-v3`
- `edit_uri` → `edit/main/docs/`
- `icon.repo` → `fontawesome/brands/github`(gitlab から変更)
- `favicon` 行 → 画像が無いため削除(またはコメントアウト)
- `nav` → 下記の通り

### nav 構成
```yaml
nav:
  - Home: index.md
  - 📘 SQuBOK Guide:
      - 1. ソフトウェア品質の基本概念: squbok/01_quality_concepts.md
      - 2. ソフトウェア品質マネジメント: squbok/02_quality_management.md
      - 3. ソフトウェア品質技術: squbok/03_quality_technologies.md
      - 4. 専門的なソフトウェア品質の概念と技術: squbok/04_specialized_topics.md
      - 5. ソフトウェア品質の応用領域: squbok/05_application_domains.md
```

## Python 環境(uv)

- `pyproject.toml` に `mkdocs-material` を依存として記載(dev 用 dependency-group)。
- `.python-version` に `3.14.3` を記載。
- ローカルプレビュー: `uv sync` → `uv run mkdocs serve`。
- Python 3.14 系で mkdocs-material が動作するかは未検証だが、方針は「動けばよい」。
  最新の mkdocs-material を使用し、動かない場合はバージョンを調整する。

## GitHub Actions デプロイ(.github/workflows/deploy.yml)

- トリガー: `main` ブランチへの push。
- 手順:
  1. `actions/checkout`
  2. `astral-sh/setup-uv`(Python は `.python-version` に従う)
  3. `uv sync`
  4. `uv run mkdocs gh-deploy --force`(`gh-pages` ブランチへ push)
- 必要な権限: `permissions: contents: write`(gh-pages への push 用)。
- GitHub リポジトリ設定で Pages のソースを `gh-pages` ブランチに指定する必要がある。
  これは CLI 操作ではなく手動設定のため、手順を README に記載する。

## 成功基準

- `uv run mkdocs serve` でローカルにサイトが表示される(Home + 5 章のプレースホルダ)。
- `uv run mkdocs build` がエラーなく完了する。
- `main` へ push すると GitHub Actions が走り、`gh-pages` にデプロイされる。
- README に、ローカル起動手順と GitHub Pages 有効化手順が記載されている。

## スコープ外

- SQuBOK 各章の実コンテンツ執筆。
- 中分類以下の詳細な nav 展開。
- 画像・favicon の追加。
- prince2 からのコンテンツ移植。
