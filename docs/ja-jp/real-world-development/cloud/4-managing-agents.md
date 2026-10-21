---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの数回の演習で、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、これらのタスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。たとえば、GitHub.com の[エージェント ページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるエージェントのタスクをすべて確認できます。実行された操作を調べたり、進行中のセッションを誘導したりできます。

このレッスンでは、次のことを行います。

- エージェント ページを開き、cloud agent のタスクを監視します。
- 実行中のセッションを誘導し、追加の機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加することにしました。サイトのスタイルを更新し、切り替え機能を追加する作業がすでに進んでいるため、ライト モードも含めるのが妥当です。エージェントの作業を誘導し、ハイ コントラスト モードとライト モードの両方を追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインにあるタスクの一覧を確認します。Copilot に割り当てた次のようなタスクが表示されます。
    - コードベースのドキュメントの更新。
    - ゲーム変更用 API の生成。
    - Web サイトへのハイ コントラスト モードの追加。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような作業が含まれます。
    - リポジトリからのコードのチェックアウト。
    - Copilot が作業する環境の作成。
    - MCP サーバーの設定。
    - 割り当てられたタスクを完了するための各種作業。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や選択した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは PR と変更されたファイルを監視できます。

## Cloud agent を誘導する

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。Copilot が付けたタイトルや作業の状態によって、表示は異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションでセットアップが完了し、作業が始まるまで数分待ちます。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot エージェント ページで作業中のセッションを誘導するダイアログを表示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映したことを確認します。

## Copilot に作業を続けてもらう

前回と同様、Copilot は更新されたタスクに取りかかります。メッセージを送信したときに実行していた手順が完了した後、新しい依頼がワークフローに追加されます。

今回も数分かかるため、これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、誘導する中心的な場所である Copilot エージェント ページを確認しました。Mission control では、進行中と完了済みのタスクを追跡し、作業をレビューして、実行中のタスクに追加の指示を送れます。

次の概念を確認しました。

- Mission control とエージェント ページを使って、cloud agent のタスクを監視しました。
- 実行中のセッションを誘導し、追加の機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、Copilot が作成した[プルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
