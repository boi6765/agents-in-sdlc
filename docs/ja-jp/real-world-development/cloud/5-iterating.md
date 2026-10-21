---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加に関する複数のタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザイン チームが改善を重ねるための関連ゲーム機能の構築、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

これまで何度も説明したように、生成 AI を導入してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードは常にレビューし、通常の DevOps プロセスに沿って作業します。ここでは、チームによるレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監視なしで非同期にタスクを実行するため、安全性を保つためのセキュリティ制約が設けられています。たとえば、次のようなものがあります。

- Copilot に許可されるのは、リポジトリへの読み取りアクセスと、コードの作業に使うブランチへの**書き込みアクセスのみ**です。
- Cloud agent は GitHub Actions 内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- 既定では、[外部リソースへのアクセスが制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも制限の対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent がコードにドキュメントを追加したプルリクエスト (PR) を確認しましょう。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は既定で外部リソース (外部 MCP サーバーへのアクセスなど) が制限されているため、これは**想定される動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。その場合は作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードを確認します。新しく作成された TSDoc のドキュメント コメントなどが含まれます。実際の変更内容は異なる場合があります。

    変更を確認しながら、TSDoc のドキュメント コメント、TypeScript の規則、コメント ヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄で、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は、一方通行ではありません。チーム メンバーと同じように、プルリクエストのコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はそのコメントを認識し、新しいセッションを開始して対応します。結果は確定的ではないため、必ず同じ結果になる指示はできません。Copilot に依頼する内容の例を示します。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例や、コードベースに追加したいドキュメントに関する別の提案を使い、**@copilot** にメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が変更を行うための新しいセッションを開始します。
3. **Back to pull request** を選択して、プルリクエストに戻ります。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

満足できるまで、Copilot とのやり取りを続けてもかまいません。問題がなければ、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対応して、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄で、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同様、Copilot cloud agent に更新を依頼し、反復して改善できます。たとえば、表示する関連ゲームの数を変えたり、コメント ヘッダーと TSDoc のドキュメント コメントを追加したりできます。カスタム指示を更新する**前**に割り当てたタスクであることを思い出してください。前回と同様、**Conversation** タブにコメントを追加すると、Copilot がそのコメントを認識して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で Mission control から依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューし、特に次の点に注目します。
   - モードを切り替える UI コンポーネント。
   - 利用者の設定を保持する local storage の使い方。
   - ハイ コントラスト モードとライト モード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄で、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動してブラウザーでハイ コントラスト モードとライト モードをテストし、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタム エージェントが、アクセシビリティのベスト プラクティスに従って機能を実装するよう Copilot を導いたことを確認してください。アクセシビリティに関する問題や改善点が見つかった場合は、前の PR と同様、コメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

バックログにある実際のタスクを cloud agent に任せ、自分は別の作業に集中できます。この方法を習慣にするため、リポジトリに issue をいくつか作成し、Copilot に割り当ててみましょう。たとえば、次のタスクを依頼できます。

- ゲーム詳細ページに支援者向けの関心表明フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の一連の演習を完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力に後から反映される内容を把握しました。
- **Copilot cloud agent に issue を割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **アクセシビリティ用カスタム エージェントを作成して使用し**、ハイ コントラストとライト モードの切り替えを追加しました。
- **Copilot エージェント ページを Mission control として使用し**、実行中のアクセシビリティ セッションを監視して誘導しました。
- **cloud agent のプルリクエストをレビューして反復的に改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent の一連の演習を完了しました。ほかの環境で同じような作業を試したい場合は、次のワークショップを確認してください。

- 🖥️ **[VS Code の演習](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI の演習](../../cli/)** — Plan と Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent との連携を使い、ターミナルからレビュー済みの変更を 2 つ完成させます。

自分のリポジトリでも、次のアイデアを試してください。

- リファクタリングやテスト カバレッジに関する issue を cloud agent に割り当てる。
- 別の分野に対応するカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
