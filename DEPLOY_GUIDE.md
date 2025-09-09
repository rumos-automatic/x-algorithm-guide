# 🚀 GitHub Pagesへのデプロイガイド

## 📋 前提条件
- GitHubアカウントを持っている
- このプロジェクトのファイルがローカルにある

## 📝 ステップバイステップガイド

### 1. GitHubリポジトリの作成

```bash
# 新しいリポジトリを作成
git init
git add .
git commit -m "Initial commit: X Algorithm Infographic"
```

### 2. GitHubに新しいリポジトリを作成

1. GitHub.comにアクセス
2. 右上の「+」→「New repository」をクリック
3. リポジトリ名: `x-algorithm-guide`（任意）
4. Public を選択
5. 「Create repository」をクリック

### 3. リモートリポジトリと接続

```bash
git remote add origin https://github.com/[your-username]/x-algorithm-guide.git
git branch -M main
git push -u origin main
```

### 4. GitHub Pagesを有効化

1. リポジトリページで「Settings」タブをクリック
2. 左サイドバーの「Pages」をクリック
3. 「Source」セクションで以下を設定：
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/docs`
4. 「Save」をクリック

### 5. デプロイの確認

1. 数分待つ（初回は最大10分かかることがあります）
2. Settingsの「Pages」セクションに表示されるURLにアクセス：
   ```
   https://[your-username].github.io/x-algorithm-guide/
   ```

## 🔧 トラブルシューティング

### ページが表示されない場合

1. **Actions タブを確認**
   - デプロイが成功しているか確認
   - エラーがある場合はログを確認

2. **ブラウザキャッシュをクリア**
   - Ctrl+Shift+R（Windows）
   - Cmd+Shift+R（Mac）

3. **ファイル構造を確認**
   ```
   docs/
   ├── index.html    # メインファイル
   ├── _config.yml   # Jekyll設定
   └── README.md     # ドキュメント
   ```

### 日本語が文字化けする場合

`docs/index.html`の`<head>`内に以下があることを確認：
```html
<meta charset="UTF-8">
```

## 🎨 カスタマイズのヒント

### ドメインをカスタマイズ

1. カスタムドメインを持っている場合
2. Settings → Pages → Custom domain
3. ドメインを入力して「Save」

### OGP（Open Graph Protocol）を追加

`index.html`の`<head>`に追加：
```html
<meta property="og:title" content="Xアルゴリズム完全攻略ガイド">
<meta property="og:description" content="公開コードから判明したバズる投稿の科学">
<meta property="og:image" content="https://[your-username].github.io/x-algorithm-guide/og-image.png">
<meta property="og:url" content="https://[your-username].github.io/x-algorithm-guide/">
<meta name="twitter:card" content="summary_large_image">
```

## 📊 アクセス解析の追加

Google Analyticsを追加する場合：

1. Google Analyticsでプロパティを作成
2. 測定IDを取得（G-XXXXXXXXXX）
3. `index.html`の`</head>`直前に追加：

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

## 🔄 更新方法

コンテンツを更新した場合：

```bash
git add .
git commit -m "Update: [更新内容]"
git push origin main
```

GitHub Pagesは自動的に更新されます（通常1-2分）。

## 📱 SNSでシェア

デプロイが完了したら、以下のようにシェアできます：

### Xでシェア
```
🚀 Xのアルゴリズムを解析してインフォグラフィックスを作りました！

✅ 投稿が伸びる5つの要素
✅ 黄金の30分タイムライン
✅ 実践チェックリスト

公開コードから判明した事実をまとめています👀

https://[your-username].github.io/x-algorithm-guide/

#X #Twitter #アルゴリズム
```

## 📚 参考リンク

- [GitHub Pages公式ドキュメント](https://docs.github.com/en/pages)
- [Jekyll公式サイト](https://jekyllrb.com/)
- [X公式アルゴリズムリポジトリ](https://github.com/twitter/the-algorithm)

---

質問や問題がある場合は、Issueを作成してください！