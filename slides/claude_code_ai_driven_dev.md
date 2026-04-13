---
marp: true
theme: default
paginate: true
header: "AI駆動開発の取り組み"
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

<!-- _class: title -->
<!-- _header: "" -->
<!-- _paginate: false -->

# データ活用Tにおける<br>AI駆動開発の取り組み

ビジネスソリューションG データ活用T 菊池翼

---

# 今日のゴール

この発表では2つのことをお伝えします。

<br>

1. **AIを使った業務効率化のノウハウ**を共有する
2. **データ活用チームの有志で行っているAI駆動開発**の取り組みを紹介する

---

<!-- _class: section-divider -->
<!-- _header: "" -->

# AI駆動開発とは

---

# AIを開発に活用するアプローチ

| アプローチ | 例 | 特徴 |
|:--|:--|:--|
| **AIチャット** | ChatGPT など | 質問すると回答してくれる。「聞く → 答えをもらう → 自分で適用する」 |
| **コード補完** | Cursor など | コードを書いている最中に続きを提案してくれる |
| **コーディングエージェント** | Claude Code など | タスク全体を自律的に実行する。「指示を出す → AIが調べて・書いて・テストする」 |

---

<!-- _class: section-divider -->
<!-- _header: "" -->

# Claude Codeとは

---

# Claude Codeの概要

Anthropic社が提供する**コーディングエージェント**。
ターミナル、VS Code、デスクトップアプリ、ブラウザ（claude.ai/code）など多くの環境で使える。

<br>

- 自然言語で指示するだけで、コードの**生成・編集・実行**をまとめてやってくれる
- ファイルの読み書き、コマンド実行、Web検索などの**ツールを標準搭載**している
- プロジェクトの**コード全体を理解**した上で作業する

---

# Claude Codeの導入効果 — 数字で見る

**Anthropic社内の調査結果**（2025年2月〜8月）：

- エンジニアの平均生産性が **+50%** 向上
- 日常的にClaude Codeを使うエンジニアが 28% → **59%** に増加
- AIがなければ着手されなかったタスクが全体の **27%**
- 手動では45分以上かかるタスクを1回のやり取りで完了するケースも

---

# Claude Codeの導入効果 — 企業事例

**企業での導入事例：**
- Palo Alto Networks: 2,000人の開発者に導入し、平均 **25%**（最大40%）の生産性向上

<br>

> 出典: Anthropic "How AI Is Transforming Work at Anthropic" (2025), AWS/Palo Alto Networks Case Study

---

# Claude Codeの特徴的な機能 ①

## CLAUDE.md（プロジェクトルール）

プロジェクトのルールや規約をmarkdownで書いておくと、AIが毎回それに従って作業する。口頭の指示ではなく、**ドキュメントとしてルールを管理**する。

<br>

## MCP Server（外部ツール連携）

AIが外部のツールやサービスにアクセスできるようになる仕組み。ドキュメント参照、Excel操作、外部APIの呼び出しなどを**AIが直接実行**できる。

---

# Claude Codeの特徴的な機能 ②

## Skill（カスタム自動化）

よく使うワークフローをテンプレート化して、**AIに繰り返し実行させる**仕組み。

<br>

## hooks（自動処理）

AIの操作の特定のタイミングで自動的にスクリプトを実行する仕組み。**フォーマッターの自動実行**、危険なコマンドのブロック、ポリシーの強制などに使える。

---

<!-- _class: section-divider -->
<!-- _header: "" -->

# 実際の活用事例

---

# Datadresserの全体アーキテクチャ

構成要素：

- **Excel パラメータシート** — 各リソース（Database, Warehouse, Role等）の設定値を定義
- **Python** — ExcelからCSVへの変換処理。リソースごとに変換処理を持つ
- **Terraform** — CSVの値をもとにSnowflakeリソースをプロビジョニング
- **GitHub Actions** — CI/CD（pytest、Terraformプラン、dbtテスト等）

---

# ライブデモ: 変換処理の自動生成

これから実際にClaude Codeを使って、SnowflakeのDatabaseリソースのパラメータシートを自動生成するデモを行います。

<br>

**注目ポイント：**
- Snowflakeの公式ドキュメントをもとに**パラメータシートが作成**されるか
- 既存の資材に合わせて**Pythonの変換処理が作成**されるか
- **動作確認も自主的に行ってくれる**か

---

<!-- _class: section-divider -->
<!-- _header: "" -->

# AIに正しく動いてもらうための工夫

---

# 工夫① Claude Codeができる操作を把握する

Claude Codeは**テキストファイルの読み書き、Web検索、bashやプログラムの実行**などが標準で可能。

一方でExcel操作など標準では対応していない作業もあるので、**MCP Serverで拡張する**など工夫が必要。

---

# 工夫② ルールを明文化する（CLAUDE.md）

「こう書いて」と毎回指示するのではなく、プロジェクトのルールを**CLAUDE.mdに書いておく**。

AIが毎回参照するので、**一貫性のあるアウトプット**が出る。

---

# 工夫③ 繰り返す作業はSkillにする

一度うまくいったワークフローは**Skillとしてテンプレート化**する。

**品質が安定**し、チーム内で共有もしやすくなる。

---

<!-- _class: section-divider -->
<!-- _header: "" -->

# 今後の取り組み

---

# 今後の取り組み

## Datadresserの進化
- 実案件での本番運用に耐えうるクオリティを目指す
- 最終的には、**要件定義書をインプットとするだけで基盤が構築される状態**が理想

## 効果の定量化
- AI活用による**工数削減効果を定量的に測定**する
- 今後のAI活用を推進するための判断材料にしたい

---

# 今後の取り組み（続き）

## AI活用の裾野を広げる
- 自チームだけでなく、より多くの人がAIを使うことで業務に活かせるアイデアを増やしていきたい
- **4月下旬からBS本部でAIワークショップを主催します。** 約40名の方々に実際にClaude Codeを触っていただき、業務に活かしたりチームに持ち帰っていただく試み。
