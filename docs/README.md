# 📊 Xアルゴリズム完全攻略ガイド - インフォグラフィックス版

## 🌐 ライブデモ
[https://[your-username].github.io/the-algorithm/](https://[your-username].github.io/the-algorithm/)

## 📝 概要
このプロジェクトは、X（旧Twitter）が公開したアルゴリズムのソースコードを解析し、実際にシステムがどのように投稿を評価しているかを視覚的にわかりやすく解説したインフォグラフィックスです。

## 🎯 主な内容

### 1. アルゴリズムスコアの構成（100点満点）
- 🔥 初動30分のエンゲージメント: **40%**
- 📝 テキスト品質スコア: **20%**
- ⭐ ユーザー信頼度: **20%**
- 👥 コミュニティ適合度: **10%**
- 🎨 メディア品質: **10%**

### 2. 黄金の30分タイムライン
投稿後30分間の具体的なアクションプラン

### 3. 実践チェックリスト
毎日実践できる具体的な行動リスト

## 🚀 GitHub Pagesへのデプロイ方法

1. このリポジトリをフォーク
2. リポジトリの Settings → Pages へ移動
3. Source を「Deploy from a branch」に設定
4. Branch を「main」、フォルダを「/docs」に設定
5. Save をクリック
6. 数分後、`https://[your-username].github.io/the-algorithm/` でアクセス可能

## 🛠️ カスタマイズ

### 色の変更
`index.html`の`:root`セクションで色を変更できます：
```css
:root {
    --primary: #1DA1F2;  /* メインカラー */
    --dark: #14171A;     /* テキストカラー */
    --success: #17BF63;  /* 成功色 */
    --warning: #FFAD1F;  /* 警告色 */
    --danger: #E0245E;   /* 危険色 */
}
```

### コンテンツの編集
HTMLファイル内のテキストを直接編集してください。

## 📊 技術仕様

- **フレームワーク**: Pure HTML/CSS/JavaScript（依存関係なし）
- **レスポンシブ対応**: モバイル・タブレット・デスクトップ対応
- **アニメーション**: CSS Animations & Intersection Observer API
- **パフォーマンス**: 軽量設計（外部ライブラリ不使用）

## 🔗 関連リンク

- [X公式アルゴリズムリポジトリ](https://github.com/twitter/the-algorithm)
- [解析ドキュメント（日本語）](../Xアルゴリズムの仕組み_わかりやすい版.md)

## 📄 ライセンス

このプロジェクトは教育目的で作成されています。
X（旧Twitter）のアルゴリズムコードはApache License 2.0でライセンスされています。

## 🤝 コントリビューション

改善提案やバグ報告は、Issuesまたはプルリクエストでお願いします。

---

Made with ❤️ based on X's open source algorithm