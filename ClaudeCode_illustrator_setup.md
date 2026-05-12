# Claude Code イラストレーター版 構築指示書

あなた（Claude Code）は、このファイルの指示に従ってイラストレーター向けのAI業務管理システムをゼロから構築してください。ユーザーの業態情報をヒアリングしながら、Step 0 から順番に進めます。

---

## 全体像

| Step | コンポーネント | 内容 |
|:-----|:------------|:-----|
| 0 | ヒアリング | ユーザーの活動分野・ツール・販売プラットフォーム・SNS・収益目標を確認 |
| 1 | フォルダ構造 | イラストレーター業務に最適化したディレクトリを作成 |
| 2 | CLAUDE.md | あなた（Claude）の役割と業務マニュアルを作成 |
| 3 | MCP連携 | Google Calendar / Notion / Sheets / Slack / Playwright / GitHub を接続 |
| 4 | Skills | 繰り返し業務をスラッシュコマンドとして定義 |
| 5 | Agent Team | イラスト制作・販売チームを定義 |
| 6 | Git管理 | バージョン管理と画像除外設定 |
| 7 | 最終確認 | 動作テスト・次のアクション提案 |

**重要**: 各Stepの冒頭で、ユーザーに必要な情報をヒアリングしてから作業に入ること。推測で進めない。

---

## Step 0：ヒアリング（必須）

以下の8項目をユーザーに質問してください。

```
1. 主な活動分野（複数選択可）
   □ 受注イラスト（クライアント案件）
   □ ストックイラスト販売（Adobe Stock / PIXTA等）
   □ 自社IP（LINEスタンプ / グッズ / 絵本）
   □ SNS発信（X / Instagram / TikTok等）
   □ 教育（オンライン講座 / 書籍 / Brain）

2. 使用ツール（複数選択可）
   □ Procreate / Photoshop / Clip Studio / Illustrator / Affinity
   □ AI生成（Midjourney / Stable Diffusion / DALL-E / Adobe Firefly / Ideogram / Cloudflare Workers AI）
   □ 動画（Blender / After Effects / Runway / Kling）

3. 販売プラットフォーム（複数選択可）
   □ Adobe Stock / PIXTA / Shutterstock / iStock
   □ Etsy / KDP / SUZURI / BOOTH
   □ LINE Creators / Skeb / ココナラ / Patreon / FANBOX

4. SNSチャネル（複数選択可）
   □ X / Instagram / Threads / TikTok / YouTube / Pinterest

5. 月の収益目標（円）

6. 自動化したい作業 トップ3（自由記述）

7. クライアント業務の有無
   □ あり（取引先数: 〇社）
   □ なし

8. 経理処理
   □ 自分でやる（freee / マネーフォワード / 手動）
   □ 税理士委託
```

回答を受け取ったら、その内容を踏まえて Step 1 以降を進めます。

---

## Step 1：フォルダ構造を作る

### ヒアリング項目
- フォルダをどこに作成するか（パス例: `~/Desktop/illustrator-ai/`）
- プロジェクト名（屋号など）

### 実行内容

以下のベース構造を作成。Step 0 のヒアリング結果で不要なフォルダは削除します。

```
{プロジェクトパス}/
├── CLAUDE.md                   ← Step 2 で作成
├── DAILY.md                    ← 毎日の日報・作業ログ
├── 00_context/                 ← プロフィール・作風・設定
│   ├── profile.md              ← 屋号・実績・活動分野
│   ├── style_guide.md          ← 作風定義（色味・線・キャラ）
│   └── memory/                 ← AIメモリ保存先
├── 01_business/                ← 事業戦略・運営
│   ├── price_list.md           ← 料金表・見積もりルール
│   └── targets.md              ← KPI・月次目標
├── 02_finance/                 ← 経理
│   ├── monthly/                ← 月次収支
│   └── invoices/               ← 請求書
├── 03_clients/                 ← クライアント案件管理
│   └── (案件ごとサブフォルダ)
├── 04_illustration/            ← 自作イラスト
│   ├── ideas/                  ← ラフ・企画
│   ├── work_in_progress/       ← 制作中
│   └── completed/              ← 完成
├── 05_stock/                   ← ストック販売
│   ├── adobe_stock/
│   ├── pixta/
│   ├── shutterstock/
│   ├── etsy/
│   ├── kdp/
│   └── line_creators/
├── 06_sns/                     ← SNS運用
│   ├── x_drafts/
│   ├── instagram_drafts/
│   └── threads_drafts/
└── output/                     ← AI生成物の置き場
    ├── trends/
    ├── articles/
    └── images/
```

### 完了条件
- 全ディレクトリ作成済み
- `DAILY.md` と各 `.md` が空テンプレートで作成済み
- ユーザーに構造を見せて確認

---

## Step 2：CLAUDE.md を作成

### 実行内容

プロジェクトフォルダ直下に `CLAUDE.md` を作成。以下のテンプレートを書き、Step 0 のヒアリング結果でカスタマイズします。

```markdown
# {ユーザー名 or 屋号} コンテキスト

## 実装ルール
**実装前にかならずplanモードで設計を出してから書く。**

## あなた（Claude）の役割
イラストレーターアシスタント / 経理サポート / リサーチャー / SNS運用補助 / クライアント対応下書き

---

## 事業概要
**屋号**: {ヒアリング}
**形態**: フリーランス / 個人事業主
**主軸**: {主な活動分野}
**目標**: 月{収益目標}円

| # | 事業 | 備考 |
|---|------|------|
| 1 | 受注イラスト | {取引先数}社 |
| 2 | ストックイラスト | Adobe Stock / PIXTA / Shutterstock |
| 3 | 自社IP | LINEスタンプ / KDP |
| 4 | SNS運用 | X / Instagram |
（不要な行は削除）

## SNSアカウント一覧
- X: @{ハンドル}
- Instagram: @{ハンドル}
- Threads: @{ハンドル}
- note: {URL}
（使ってないチャネルは削除）

---

## ディレクトリ構造
（Step 1 で作成した構造を記載）

---

## ワークフロートリガー

| トリガーワード | Skill |
|-------------|-------|
| 「おはよう」「今日の予定」 | `/daily-schedule` |
| 「タスク登録」 | `/issue-triage` |
| 「覚えておいて」 | `/agent-memory` |
| 「ストック生成」「素材作って」 | `/stock-generate` |
| 「メタデータ作成」「キーワード」 | `/stock-meta` |
| 「ラフ案」「企画」 | `/illust-plan` |
| 「投稿案」「下書き」 | `/write-draft` |
| 「見積もり」「請求書」 | `/client-quote` |
| 「収支更新」 | `/update-finance` |
| 「週報」 | `/weekly-report` |

---

## 外部連携
- **Google Calendar** (MCP): 予定の取得・作成
- **Notion** (MCP): 案件・収支・投稿下書きDB管理
- **Google Sheets** (MCP): ストック売上ログ
- **Slack** (MCP): クライアント連絡（必要に応じて）
- **Playwright** (MCP): ストックサイトの操作補助
- **GitHub** (MCP): タスク・コード管理

---

## 画像送信 → AI生成プロンプト自動出力ルール
ユーザーが画像を1枚送ってきたら（CSV/PDF以外）、自動で以下を実行：

1. 画像を詳細に解析
2. **Ideogram用プロンプト**（英語・Style/Aspect Ratio指定・subject→composition→style→colors→mood→lighting→detail の順・Negative prompt付き）
3. **Midjourney用プロンプト**（/imagine形式・`--ar` `--style` `--v 6` 付き）
4. **要素分解**（日本語で構図・色・雰囲気を箇条書き）

---

## CSV送信 → アナリティクス自動分析ルール
SNSや販売プラットフォームのCSVが送られたら、自動で：
1. データ集計
2. TOP5抽出
3. パフォーマンス分析
4. 改善提案

---

## 注意事項
- クライアント名はイニシャル化（A社・B様 など）
- 財務情報はローカルのみ管理・外部送信しない
- ストック素材のメタデータは競合優位性があるため慎重に扱う
```

### 完了条件
- `CLAUDE.md` がプロジェクト直下に保存済み
- ユーザーに内容確認・修正反映

---

## Step 3：MCP連携を設定

### ヒアリング項目
- 接続するツールの選択（チェックリスト方式）
- 各ツールのAPIキー・認証情報の準備状況

### 各ツールの設定手順

#### Google Calendar
1. パッケージインストール: `@cocal/google-calendar-mcp`
2. Google Cloud Console で OAuth 2.0 クライアントIDを作成（手順を案内）
3. `~/.claude/` の MCP設定ファイルに追加
4. **接続テスト**: 今日の予定を取得して表示

#### Notion
1. Notion Internal Integration を作成（https://www.notion.so/my-integrations）
2. APIキー取得
3. 接続したい DB で「コネクトを追加」
4. MCP設定ファイルに追加
5. **接続テスト**: 指定DBを読み取って表示

#### Google Sheets
1. Google Sheets MCP をインストール
2. OAuth認証を設定
3. **接続テスト**: 指定スプレッドシートを読み取り

#### Slack
1. Slack App を作成、Bot Token Scopes を設定（`chat:read`, `chat:write`, `channels:history` など）
2. ワークスペースにインストール
3. MCP設定ファイルに追加
4. **接続テスト**: 指定チャンネルのメッセージ取得

#### Playwright
1. Playwright MCP をインストール
2. **接続テスト**: 任意URLのスクリーンショット取得
3. **用途例**：Adobe Stockのアップロード補助、PIXTAのIPTC埋め込み確認、Instagram投稿の予約等

#### GitHub
1. `gh auth login` で GitHub CLI認証
2. **接続テスト**: リポジトリ一覧取得

### CLAUDE.md の更新
接続したツールを「外部連携」セクションに反映する。

### 完了条件
- 選択した全ツールのMCP接続完了
- 各ツールの接続テスト成功
- `CLAUDE.md` に連携情報追加

---

## Step 4：Skills を作成

### ヒアリング項目
- Step 0 で挙がった「自動化したい作業 トップ3」
- 追加で作りたいスキル

### Skills は `.claude/commands/` に Markdown ファイルとして作成

#### 4-1. `/daily-schedule`（朝の工程表）

```markdown
# 朝の工程表生成スキル

1. Google Calendar から今日の予定を取得
2. GitHub Issue から進行中タスクを取得（連携してれば）
3. Notion の投稿下書きDBから今日投稿予定の案件を取得
4. DAILY.md の昨日の未完了タスクを確認
5. 「緊急度×重要度」で並び替え
6. 15分刻みの工程表を生成
7. DAILY.md に本日セクションを追記
```

#### 4-2. `/issue-triage`（タスク管理）

```markdown
# タスクをGitHub Issueに登録するスキル

1. ユーザー入力からタスク内容・優先度・カテゴリを判定
2. ラベル自動付与（type / priority / domain）
3. 既存Issueとの重複チェック
4. GitHub Issue作成・Projectに追加
5. 結果報告
```

#### 4-3. `/agent-memory`（メモリ保存）

```markdown
# 会話内の重要情報を保存するスキル

1. 保存すべき情報を抽出（意思決定・好み・データ）
2. カテゴリ分類
3. 00_context/memory/ の適切なファイルに追記
4. 保存内容を報告
```

#### 4-4. `/stock-generate`（AIストック素材一括生成）

```markdown
# AIストック素材一括生成スキル

1. ユーザーから「テーマ」「枚数」「サイズ」「販売先プラットフォーム」をヒアリング
2. Cloudflare Workers AI / Ideogram / Midjourney API などで画像生成（無料優先）
3. 生成画像を 05_stock/{プラットフォーム}/ に保存
4. メタデータCSVを自動生成（Step 4-5 へ）
5. 完了通知
```

#### 4-5. `/stock-meta`（メタデータ自動生成）

```markdown
# ストック素材のメタデータCSV自動生成スキル

1. 指定フォルダの画像を Gemini Vision API などで解析
2. タイトル・キーワード・説明文を多言語で生成
3. 各プラットフォーム規格に変換：
   - Adobe Stock: ファイル名30バイト以内、キーワード49個以内、UTF-8 BOM CSV
   - PIXTA: IPTC埋め込み（UTF-8）
   - Shutterstock: 専用CSV
4. 各フォルダにCSV保存
```

#### 4-6. `/illust-plan`（ラフ・企画案）

```markdown
# イラスト企画スキル

1. テーマをヒアリング
2. 構図案・色案・雰囲気を3パターン生成
3. Ideogram / Midjourneyプロンプトを出力
4. 04_illustration/ideas/ に保存
```

#### 4-7. `/write-draft`（SNS投稿案）

```markdown
# SNS投稿下書きスキル

1. テーマ・チャネル（X / Instagram / Threads）をヒアリング
2. プラットフォーム特性に合わせた本文生成
3. ハッシュタグ提案
4. 06_sns/{チャネル}_drafts/ に保存
5. Notion投稿DBに登録（連携している場合）
```

#### 4-8. `/client-quote`（見積もり・請求書）

```markdown
# 見積もり・請求書ドラフトスキル

1. 案件概要をヒアリング（クライアント名・作業内容・点数・納期）
2. 01_business/price_list.md を参照して金額算出
3. 見積書 or 請求書のMarkdownドラフトを生成
4. 03_clients/{案件名}/ に保存
```

#### 4-9. `/update-finance`（収支記録）

```markdown
# 月次収支記録スキル

1. ユーザーから各収益源の金額をヒアリング
2. 合計計算
3. Google Sheets or Notion DB に記録
4. 月次レポート生成（前月比・達成率・コメント）
```

#### 4-10. `/weekly-report`（週報）

```markdown
# 週次振り返りスキル

1. DAILY.mdから過去1週間の作業を集約
2. 完了タスク・継続タスク・新発生タスクを分類
3. KPI進捗をチェック
4. 来週の重点を提案
```

### CLAUDE.md の更新
全Skillsのトリガーワードを「ワークフロートリガー」セクションに反映。

### 完了条件
- 選択したスキルファイルが `.claude/commands/` に作成済み
- 各スキルを手動テストで動作確認
- `CLAUDE.md` に反映

---

## Step 5：Agent Team を定義

### イラスト制作・販売チーム

`.claude/agents/` に以下のエージェント定義を作成：

| エージェント | 役割 |
|:-----------|:-----|
| トレンドリサーチャー | SNS・販売サイトのトレンド調査、競合分析 |
| 企画担当 | ラフ・構図・キャラ案を提案 |
| 批判的レビュアー | 企画案の弱点・市場性をDevil's Advocateで指摘 |
| 着色アドバイザー | 配色・トーン・ライティング提案 |
| 検品担当 | 完成イラストの品質チェック（解像度・キャラ崩れ・規約違反） |
| メタデータ生成 | キーワード・タイトル・説明文を多言語生成 |
| 編集長 | 全体統括・最終意思決定 |

### ワークフロー例
```
ユーザー: 「来月のAdobe Stock向けに桜素材を20点作りたい」
   ↓
編集長: ワークフローを設計
   ↓
トレンドリサーチャー（並列）+ 企画担当（並列）
   ↓
批判的レビュアー: 企画案をレビュー
   ↓
編集長: 修正指示
   ↓
（実制作はユーザー or /stock-generate）
   ↓
着色アドバイザー: 配色提案
   ↓
検品担当: 規約チェック
   ↓
メタデータ生成: 多言語キーワード
   ↓
編集長: 完了報告
```

### Skills化
チーム実行を `/illust-team` のようなコマンドで一発起動できるようにする。

### 完了条件
- エージェント定義ファイルが作成済み
- テスト実行で連携確認
- Skills化

---

## Step 6：Git管理

1. プロジェクトフォルダをGitリポジトリ初期化
   ```bash
   git init
   ```
2. `.gitignore` を作成：
   ```gitignore
   # 機密情報
   .env
   *.key
   credentials/

   # 画像（重い・GitHubに上げない）
   *.png
   *.jpg
   *.jpeg
   *.psd
   *.psb
   *.ai
   *.procreate

   # 一時ファイル
   .DS_Store
   __pycache__/
   *.pyc

   # 個別案件の機密
   03_clients/*/contract.*
   02_finance/*/raw/
   ```
3. 初回コミット作成
4. ユーザーの許可を得て GitHub にプライベートリポジトリを作成・push

### 完了条件
- Gitリポジトリ初期化済み
- `.gitignore` に機密ファイル登録
- GitHubにプライベートリポジトリ作成（希望する場合）

---

## Step 7：最終確認

すべてのStepが完了したら、以下を実施します。

1. **動作テスト**: 「おはよう」と入力 → `/daily-schedule` が動くか確認
2. **全体レビュー**: 作成したファイル一覧をユーザーに提示
3. **CLAUDE.md の最終確認**: 全連携・スキル・ワークフローが記載されているか
4. **次のアクション提案**:
   - 1週間使ってみる
   - 不便な点を `/agent-memory` で覚えさせる
   - 追加で自動化したい業務があれば言ってもらう

### ユーザーへの提案メッセージ（最後に出す）

> セットアップは完了です。明日の朝から「おはよう」と入力するだけで、工程表が自動生成されます。
> 日常的に使う中で「これを自動化したい」と思った作業があれば、いつでも「Skills化して」と言ってください。
> あなたの業務に合わせてカスタマイズしていきます。

---

## クイックスタート版（おまかせモード）

ユーザーが「全部おまかせで作って」と言った場合は、以下の最小構成で起動：

- フォルダ構造: ベース構造そのまま
- CLAUDE.md: テンプレートをそのまま
- MCP連携: Google Calendar と Notion の2つだけ
- Skills: `/daily-schedule`, `/agent-memory`, `/write-draft` の3つだけ
- Agent Team: スキップ
- Git: スキップ

その後、使い始めてから1週間ごとに「足したい機能あるか？」を聞き、徐々に拡張する。

---

以上が、イラストレーター向け Claude Code 構築の完全マニュアルです。
このファイルをそのまま Claude Code に貼り付ければ、AIがあなたの業務をヒアリングして、自動でシステム構築まで進めてくれます。
