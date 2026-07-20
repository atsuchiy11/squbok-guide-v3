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
