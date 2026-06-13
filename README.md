# FLIP DECK

TikTok / YouTube Shorts 風のフリップUIで、ペライチ HTML アプリ（ミニゲームなど）を縦スワイプで次々に楽しむビューア。

## 構成

```
index.html      フリップUI本体（お品書きを menu.json から読み込む）
menu.json       お品書き（表示するアプリの一覧・説明）
apps/           iframe で表示するペライチ HTML 群
  tameru.html     ためる   — チャージ＆リリース
  tomeru.html     とめる   — タイミング（光を止める）
  tsumu.html      つむ     — ブロックスタッカー
  yokeru.html     よける   — ワンボタン回避
.nojekyll       GitHub Pages の Jekyll 処理を無効化
```

## アプリの追加・変更

1. ペライチの自己完結 HTML（外部依存なし）を `apps/` に置く。
2. `menu.json` の `apps` 配列にエントリを足す：

   ```json
   {
     "file": "your-app.html",
     "title": "タイトル",
     "tag": "GAME",
     "description": "一覧に出る説明文。"
   }
   ```

   - `file` … `apps/` 内のファイル名（`https://` で始まる外部URLも可）
   - `title` … カード上のタグと一覧の見出し（省略時はファイル名）
   - `tag` … 一覧右側の小バッジ（任意）
   - `description` … お品書きの説明文（任意）

並び順は `menu.json` の記載順。右上の「⋮」からお品書きを開き、項目を選ぶとそのカードへジャンプします。「順番／ランダム」もそこで切り替えます。

## ローカル確認

`file://` で直接開くと `menu.json` の `fetch` がブラウザにブロックされることがあります。簡易サーバー経由で開いてください：

```bash
python -m http.server 8000
# → http://localhost:8000/ を開く
```

## GitHub Pages へのデプロイ

リポジトリのルートをそのまま公開します。

```bash
git init
git add .
git commit -m "FLIP DECK"
git branch -M main
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

GitHub のリポジトリ設定 → **Settings → Pages** で：

- **Source**: `Deploy from a branch`
- **Branch**: `main` / `(root)`

数分後に `https://<user>.github.io/<repo>/` で公開されます。

## メモ

- 各アプリは `sandbox="allow-scripts allow-pointer-lock allow-popups"` の iframe で読み込まれます（`allow-same-origin` なし）。`localStorage` はブロックされ得るので、ハイスコア等はメモリ内保持が前提です。
- ペライチ HTML は自己完結（CSS/JS インライン、外部依存なし）で作ると安定します。
