---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。解決されていないセキュリティの問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな課題があります。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行します。エージェントは作業が完了すると、開発者がレビューできるドラフト PR を作成します。これにより、単調なタスクを委任し、開発プロセスを加速して、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent では、次のことを確認します。

- コード生成環境のカスタマイズ。
- 操作が安全に行われることの確認。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを開発した請負業者は、ドキュメントを十分に整備していませんでした。実際には、ドキュメントがまったくありません。まず、アプリケーション内のすべてのエクスポートされた関数に TSDoc のドキュメント コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの他のタイトルを関連ゲームとして表示し、利用者がゲームを探し続けられるようにします。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、優先度の高い issue がほかにあります。

このようなタスクは優先度が下がりやすく、Copilot cloud agent に任せるのに適しています。cloud agent に非同期で作業してもらう間、開発者は別のタスクに集中し、後から Copilot の作業をレビューして期待どおりであることを確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。人間の開発者に作業を依頼するときと同様に、[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法があります。割り当てると Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境をセットアップして、タスクに取りかかります。実行中でも完了後でも Copilot のセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法のような機能はありません。考える作業を省略できるわけではありません。一見簡単そうな操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲は慎重に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして捉え、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、Copilot を導く方法を確認しました。カスタム指示の演習を完了していれば、`.github/copilot-instructions.md` の動作を確認済みです。まだの場合は、ここで簡単に確認しておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれている指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの github.com ページ、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認し、特に TypeScript の規則と TSDoc のドキュメント コメントに関する要件に注目します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てるときは、これらの指示ファイルによる TSDoc のドキュメント コメント、TypeScript の規則、コメント ヘッダーが PR に反映されていることを確認してください。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は特定の環境やセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェア エンジニアと同じように作業します。

Cloud agent は作業時に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` で構成する[セットアップ ワークフロー][setup-workflow]を作成すると、環境をカスタマイズできます。作業開始前に必要な開発ツールや依存関係を利用できるようになります。学習の流れを円滑にし、この機会を活用できるよう、ラボでは事前に設定されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一の Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと似ていますが、いくつかの重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを設定しているため、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の作業開始を待たずにワークフローが正常に実行されるかテストする場合に便利です。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、古い情報しかないプロジェクトや、ドキュメントがまったくないプロジェクトも少なくありません。このような技術的負債は放置されやすく、生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を得意としており、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で作業を確認します。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで数分かかる場合があります。Copilot が環境を作成し (前に紹介したワークフローを実行し)、プロジェクトを分析して、問題への最適な対処方法を検討しています。

13. todo リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するため、ウィンドウの再読み込みが必要な場合があります。

16. セッションをスクロールし、Copilot が問題を解決する過程を確認します。コードを調査して状態を把握し、計画を考えて適切な方法を決め、コードを作成します。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の一つは、タスクを非同期で実行している間、開発者が別の作業に集中できるようにすることです。その機能を活用し、もう一つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加するコードを書きましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の一つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業自体は長時間かからないかもしれませんが、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後すぐに、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![目の絵文字で Copilot が issue に取り組んでいることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように動作するため、常に監視する必要はありません。作業が完了した後にレビューできます。次は、カスタム エージェントの作成と使用に進みましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期で作業を実行できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに Copilot を活用できます。

次の概念を確認しました。

- コード生成環境のカスタマイズ。
- 操作が安全に行われることの確認。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

Copilot cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
