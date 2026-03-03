# Marp スライド生成プロジェクト

## プロジェクト概要
ユーザーが `drafts/` フォルダにマークダウンでレジュメ（原稿）を作成し、Claudeがそれを装飾してMarp形式のスライドとして `slides/` フォルダに出力する。

## フォルダ構成
- `drafts/` — ユーザーが作成するレジュメ原稿（入力）
- `slides/` — Claudeが生成するMarpスライド（出力）
- `dist/` — Marp CLIが出力するHTML/PDF/PPTX（ビルド成果物）

## スライド生成ルール

### 絶対ルール
- ユーザーが書いた文章の内容は変更しない（誤字修正や表現の変更は行わない）
- デザイン・レイアウト・装飾はすべてClaudeが担当する

### テーマ・スタイル
テーマは **default** を使用する（固定）。
すべてのスライドファイルの先頭に以下のフロントマターとカスタムCSSを含める：

```yaml
---
marp: true
theme: default
paginate: true
header: "スライドタイトル"
style: |
  section {
    font-family: 'Segoe UI', 'Noto Sans JP', sans-serif;
  }
  section.title {
    text-align: center;
    justify-content: center;
  }
  section.title h1 {
    font-size: 2.2em;
    color: #2563eb;
  }
  section.title p {
    font-size: 1.1em;
    color: #64748b;
  }
  section.section-divider {
    background: #2563eb;
    color: white;
    text-align: center;
    justify-content: center;
  }
  section.section-divider h1 {
    color: white;
    font-size: 2em;
  }
  section.section-divider header {
    color: rgba(255,255,255,0.5);
  }
  h1 {
    color: #1e3a5f;
    border-bottom: 2px solid #2563eb;
    padding-bottom: 8px;
  }
  h2 {
    color: #2563eb;
  }
  code {
    background: #f1f5f9;
    color: #e11d48;
  }
  table th {
    background: #2563eb;
    color: white;
  }
  blockquote {
    border-left: 4px solid #2563eb;
    background: #eff6ff;
    padding: 0.5em 1em;
  }
  strong {
    color: #1e40af;
  }
  footer {
    color: #94a3b8;
  }
  header {
    color: #94a3b8;
  }
---
```

### スライド構成
- `---` でページを区切る
- 最初のスライドはタイトルスライド: `<!-- _class: title -->` `<!-- _header: "" -->` `<!-- _paginate: false -->`
- セクション区切りスライド: `<!-- _class: section-divider -->` `<!-- _header: "" -->`
- 各スライドの情報量は適切に保つ（1スライドに詰め込みすぎない）
- 箇条書きは1スライドあたり最大5-6項目を目安にする

### デザインディレクティブ
Claudeは以下のMarpディレクティブを積極的に活用する：
- `<!-- _class: title -->` — タイトルスライド（中央寄せ、ブルー見出し）
- `<!-- _class: section-divider -->` — セクション区切り（ブルー背景、白文字）
- `<!-- header: "..." -->` — ヘッダー（スライドタイトルを常時表示）
- `<!-- _header: "" -->` — 個別スライドでヘッダーを非表示
- `<!-- _paginate: false -->` — 個別スライドでページ番号を非表示

### テキスト装飾
- **太字**、*斜体* を適切に使い、重要なポイントを強調する
- コードブロックには言語指定をつける（```python など）
- 必要に応じて表（テーブル）や番号付きリストを使用する

## 見切れチェック
HTML作成時に一時的に各スライドのPNGを作成し、見切れや見づらい箇所がないか確認する。
- `npx marp <スライドファイル> --html --images png` でPNG出力
- 全スライドのPNG画像を目視確認する
- 見切れがある場合はスライドの分割や、コードブロック等は左右2カラムレイアウトを使うなどして修正する
- 確認完了後、一時生成したPNGファイルは削除する

## ビルドコマンド
- `npm run dev` — スライドのライブプレビュー
- `npm run build` — HTML出力
- `npm run build:pdf` — PDF出力
- `npm run build:pptx` — PPTX出力
