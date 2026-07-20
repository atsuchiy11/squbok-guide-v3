# squbok-guide-v3 MkDocs 環境構築 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** prince2 の MkDocs Material 構成を土台に、GitHub Pages(Actions)デプロイと uv 管理を備えた SQuBOK ガイドの MkDocs 骨組みを構築する。

**Architecture:** uv で `mkdocs-material` を管理し、`mkdocs.yml` は prince2 の設定を引き継ぎつつ URL/icon を GitHub 向けに変更。`docs/` に Home + SQuBOK V3 の5大分類プレースホルダを配置。`main` への push で GitHub Actions が `mkdocs gh-deploy` を実行し `gh-pages` へデプロイする。

**Tech Stack:** MkDocs, Material for MkDocs, uv, Python 3.14.3, GitHub Actions, GitHub Pages

## Global Constraints

- Python バージョン: 3.14.3(mise グローバル設定に準拠、`.python-version` に記載)
- 依存管理: uv + `pyproject.toml`(pip/venv 手動管理はしない)
- サイト言語: 日本語(`language: ja`)
- リポジトリ: `atsuchiy11/squbok-guide-v3`(GitHub)
- 公開 URL: `https://atsuchiy11.github.io/squbok-guide-v3/`
- Author / Copyright: `Atsushi Tsuchiya` / `Copyright © 2026 Atsushi Tsuchiya`
- 検証コマンドは全て `uv run` 経由で実行する
- 各タスク完了時にコミットする(frequent commits)

---

## File Structure

- `pyproject.toml` — uv プロジェクト定義。mkdocs-material を dev dependency-group に記載
- `.python-version` — `3.14.3`
- `.gitignore` — `site/`, `.venv/`, `__pycache__/`, `.DS_Store`
- `mkdocs.yml` — サイト設定(prince2 引き継ぎ + GitHub 向け調整)
- `docs/index.md` — Home プレースホルダ
- `docs/squbok/0X_*.md` — SQuBOK 5大分類プレースホルダ(5ファイル)
- `docs/assets/images/.gitkeep` — 画像用空ディレクトリ
- `.github/workflows/deploy.yml` — GitHub Actions デプロイ
- `README.md` — ローカル起動手順 + GitHub Pages 有効化手順

---

## Task 1: uv プロジェクト初期化と mkdocs-material 導入

**Files:**
- Create: `pyproject.toml`
- Create: `.python-version`
- Create: `.gitignore`

**Interfaces:**
- Produces: `uv run mkdocs` が実行可能な環境。`uv.lock` が生成される。

- [ ] **Step 1: `.python-version` を作成**

ファイル `/Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3/.python-version`:
```
3.14.3
```

- [ ] **Step 2: `pyproject.toml` を作成**

```toml
[project]
name = "squbok-guide-v3"
version = "0.1.0"
description = "SQuBOK Guide documentation site powered by MkDocs"
readme = "README.md"
requires-python = ">=3.14"

[dependency-groups]
dev = [
    "mkdocs-material",
]
```

- [ ] **Step 3: `.gitignore` を作成**

```
# Python
__pycache__/
*.py[cod]
.venv/

# MkDocs
site/

# macOS
.DS_Store
```

- [ ] **Step 4: 依存をインストールして lock を生成**

Run: `cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3 && uv sync`
Expected: `.venv` が作成され、`uv.lock` が生成される。エラーなく完了。
（Python 3.14 で mkdocs-material が解決できない場合は、`requires-python` を `>=3.12` に下げ、`.python-version` は 3.14.3 のままとするか、動作するバージョンに調整する。）

- [ ] **Step 5: mkdocs が動くことを確認**

Run: `uv run mkdocs --version`
Expected: `mkdocs, version X.Y.Z ...` が表示される。

- [ ] **Step 6: Commit**

```bash
cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3
git add pyproject.toml .python-version .gitignore uv.lock
git commit -m "chore: init uv project with mkdocs-material"
```

---

## Task 2: docs コンテンツ骨組みの作成

**Files:**
- Create: `docs/index.md`
- Create: `docs/squbok/01_quality_concepts.md`
- Create: `docs/squbok/02_quality_management.md`
- Create: `docs/squbok/03_quality_technologies.md`
- Create: `docs/squbok/04_specialized_topics.md`
- Create: `docs/squbok/05_application_domains.md`
- Create: `docs/assets/images/.gitkeep`

**Interfaces:**
- Consumes: なし
- Produces: `mkdocs.yml` の nav が参照するファイル群。各パスは Task 3 の nav と一致させること。

- [ ] **Step 1: `docs/index.md` を作成**

```markdown
# SQuBOK Guide

ソフトウェア品質知識体系ガイド(SQuBOK V3)の学習ドキュメントサイトです。

左のナビゲーションから各章を参照してください。

!!! note "作成中"
    本サイトは骨組みを構築した段階です。各章の内容は順次追記します。
```

- [ ] **Step 2: `docs/squbok/01_quality_concepts.md` を作成**

```markdown
# 1. ソフトウェア品質の基本概念

!!! note "準備中"
    この章の内容は今後追記します。
```

- [ ] **Step 3: `docs/squbok/02_quality_management.md` を作成**

```markdown
# 2. ソフトウェア品質マネジメント

!!! note "準備中"
    この章の内容は今後追記します。
```

- [ ] **Step 4: `docs/squbok/03_quality_technologies.md` を作成**

```markdown
# 3. ソフトウェア品質技術

!!! note "準備中"
    この章の内容は今後追記します。
```

- [ ] **Step 5: `docs/squbok/04_specialized_topics.md` を作成**

```markdown
# 4. 専門的なソフトウェア品質の概念と技術

!!! note "準備中"
    この章の内容は今後追記します。
```

- [ ] **Step 6: `docs/squbok/05_application_domains.md` を作成**

```markdown
# 5. ソフトウェア品質の応用領域

!!! note "準備中"
    この章の内容は今後追記します。
```

- [ ] **Step 7: `docs/assets/images/.gitkeep` を作成(空ファイル)**

内容は空でよい。画像用ディレクトリを git に残すため。

- [ ] **Step 8: Commit**

```bash
cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3
git add docs/
git commit -m "docs: add SQuBOK skeleton pages"
```

---

## Task 3: mkdocs.yml の作成

**Files:**
- Create: `mkdocs.yml`

**Interfaces:**
- Consumes: Task 2 で作成した `docs/index.md` と `docs/squbok/0X_*.md`(nav の参照先)
- Produces: `uv run mkdocs build` が成功する完全な設定

- [ ] **Step 1: `mkdocs.yml` を作成**

```yaml
# MkDocs configuration file for SQuBOK Guide
# Documentation: https://www.mkdocs.org/

# Site information
site_name: SQuBOK Guide
site_description: ソフトウェア品質知識体系ガイド powered by MkDocs
site_author: Atsushi Tsuchiya
site_url: https://atsuchiy11.github.io/squbok-guide-v3/

# Repository information
repo_name: atsuchiy11/squbok-guide-v3
repo_url: https://github.com/atsuchiy11/squbok-guide-v3
edit_uri: edit/main/docs/

# Copyright
copyright: Copyright &copy; 2026 Atsushi Tsuchiya

# Theme configuration
theme:
  name: material
  language: ja

  # Color scheme
  palette:
    # Light mode
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: ダークモードに切り替え

    # Dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: ライトモードに切り替え

  # Features
  features:
    - navigation.instant
    - navigation.tracking
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - navigation.top
    - search.suggest
    - search.highlight
    - search.share
    - content.code.copy
    - content.tabs.link

  # Icons
  icon:
    repo: fontawesome/brands/github
    edit: material/pencil
    view: material/eye

nav:
  - Home: index.md
  - 📘 SQuBOK Guide:
      - 1. ソフトウェア品質の基本概念: squbok/01_quality_concepts.md
      - 2. ソフトウェア品質マネジメント: squbok/02_quality_management.md
      - 3. ソフトウェア品質技術: squbok/03_quality_technologies.md
      - 4. 専門的なソフトウェア品質の概念と技術: squbok/04_specialized_topics.md
      - 5. ソフトウェア品質の応用領域: squbok/05_application_domains.md

markdown_extensions:
  - toc:
      permalink: true
  - tables
  - admonition
  - codehilite
  - pymdownx.details

plugins:
  - search
```

- [ ] **Step 2: strict モードでビルドして検証(nav 参照漏れを検出)**

Run: `cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3 && uv run mkdocs build --strict`
Expected: `INFO - Documentation built in ...` で成功。warning でエラーにならないこと(nav に無いファイル等が無い)。

- [ ] **Step 3: ローカルサーブで目視確認(任意・短時間)**

Run: `uv run mkdocs serve`（起動確認後 Ctrl+C）
Expected: `http://127.0.0.1:8000/` で Home と5章が表示される。ライト/ダークトグルが効く。

- [ ] **Step 4: ビルド生成物を掃除**

Run: `rm -rf site`
Expected: `.gitignore` 対象なので不要な生成物を残さない。

- [ ] **Step 5: Commit**

```bash
cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3
git add mkdocs.yml
git commit -m "feat: add mkdocs.yml for SQuBOK Guide"
```

---

## Task 4: GitHub Actions デプロイワークフロー

**Files:**
- Create: `.github/workflows/deploy.yml`

**Interfaces:**
- Consumes: `pyproject.toml`, `.python-version`, `mkdocs.yml`
- Produces: `main` push 時に `gh-pages` ブランチへデプロイする CI

- [ ] **Step 1: `.github/workflows/deploy.yml` を作成**

```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up uv
        uses: astral-sh/setup-uv@v6
        with:
          python-version-file: ".python-version"

      - name: Install dependencies
        run: uv sync

      - name: Deploy to GitHub Pages
        run: uv run mkdocs gh-deploy --force
```

- [ ] **Step 2: YAML 構文をローカル確認**

Run: `cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3 && uv run python -c "import yaml,sys; yaml.safe_load(open('.github/workflows/deploy.yml')); print('yaml ok')"`
Expected: `yaml ok`

- [ ] **Step 3: Commit**

```bash
cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3
git add .github/workflows/deploy.yml
git commit -m "ci: add GitHub Actions workflow for GitHub Pages deploy"
```

---

## Task 5: README とドキュメント整備

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: 全タスクの成果物
- Produces: セットアップ・起動・デプロイ手順のドキュメント

- [ ] **Step 1: `README.md` を作成**

```markdown
# squbok-guide-v3

ソフトウェア品質知識体系ガイド(SQuBOK)の MkDocs ドキュメントサイト。

公開 URL: https://atsuchiy11.github.io/squbok-guide-v3/

## 必要環境

- Python 3.14.3(mise 等で管理)
- [uv](https://docs.astral.sh/uv/)

## セットアップ

```bash
uv sync
```

## ローカルプレビュー

```bash
uv run mkdocs serve
```

`http://127.0.0.1:8000/` で確認できます。

## ビルド

```bash
uv run mkdocs build --strict
```

## デプロイ

`main` ブランチへ push すると GitHub Actions が `mkdocs gh-deploy` を実行し、
`gh-pages` ブランチへデプロイします。

### 初回のみ: GitHub Pages の有効化

1. GitHub リポジトリの **Settings → Pages** を開く
2. **Source** を **Deploy from a branch** にする
3. Branch を **`gh-pages`** / **`/ (root)`** に設定して Save

初回の Actions 実行後に `gh-pages` ブランチが作成されるため、その後に上記設定を行ってください。
```

- [ ] **Step 2: 最終ビルド確認**

Run: `cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3 && uv run mkdocs build --strict && rm -rf site`
Expected: エラーなく成功。

- [ ] **Step 3: Commit**

```bash
cd /Users/tsuchiyaatsushishi/.ghq/github.com/atsuchiy11/squbok-guide-v3
git add README.md
git commit -m "docs: add README with setup and deploy instructions"
```

---

## Self-Review 結果

- **Spec coverage:** ディレクトリ構成(T1-T5)、mkdocs.yml 引き継ぎ+変更(T3)、uv/Python 3.14.3(T1)、GitHub Actions(T4)、README 手順(T5)、成功基準(build/serve/CI/README)を全てカバー。スコープ外項目(実コンテンツ、中分類、画像、prince2 移植)はプランに含めていない。
- **Placeholder scan:** プラン内に TBD/TODO 等の未確定記述なし。各コードステップに実内容を記載済み。
- **Type/path consistency:** Task 2 で作成する docs パスと Task 3 の nav 参照先が一致。`.python-version`(T1)を Actions(T4)が参照。
- **既知リスク:** Python 3.14 で mkdocs-material が未解決の場合、T1 Step4 に記載のフォールバック(`requires-python` 調整)で対応。
