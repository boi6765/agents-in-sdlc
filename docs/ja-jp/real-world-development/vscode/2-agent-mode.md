---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションに小さな変更を加える場合でも、複数のファイルを更新し、テストを実行するなどの作業が必要になることがあります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更し忘れたファイルを見つけて、さらに更新します。この作業は際限なく続く場合があります。

このような場面で Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に動作します。開発者と同じように、プロジェクト構造を調査し、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新しい機能を追加する方法を見ていきましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途に対応しています。エージェント モードは IDE 内でタスクを実行し、素早くフィードバックを得ながら対話できます。Cloud agent はチーム メンバーのように非同期で動作し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ レイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode が生成した変更やテストをレビューしてから、コードベースにマージする方法。

## シナリオ

ゲームの一覧が増えてきたので、利用者がカテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー ヘルパーを追加しました。Copilot Agent Mode を使い、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者向けのテーマを持つボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリであり、ページはビルド時に静的 HTML としてレンダリングされます。ページは frontmatter から Drizzle ORM を使ってローカルの SQLite データベースに直接クエリを実行します。独立したバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に配置し、インタラクティブな動作は標準の DOM API を使った小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` ブランチに戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナル ウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のように URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace を使用している場合、Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、issue のバックログが自動的に作成されています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは自律的に計画を立て、複数のファイルに変更を加え、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使用したため、提案の一貫性を高め、操作しやすくする目的で、手順に記載しています。ただし、LLM の出力は確率的です。ワークショップの記述と異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の出力は確率的であるため、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、利用者にとって有用だと判断した順に issue の優先順位を付けていることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトへの機能追加方法を説明する[ユーザー インターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方法に関する全体的なガイダンスを確認します。次のような内容があります。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - 次のようなファイルの種類に固有の指示ファイルへのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準にします。これにより、指示を再利用しやすく、小さな単位に分けやすくなり、例やテンプレートを Copilot に示せます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、アプリケーションに少なくとも 3 つの変更を加える必要があります。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する。
- フィルタリングの動作に関するテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を実行してもらいましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを追加する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 issue の要件に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを特定する様子を確認します。データ レイヤーのヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを行います。タスクやコードベースの複雑さによって、数分かかる場合があります。作業中にコード エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了すると、すべての変更に対して **Keep** または **Undo** を選択できるようになるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでユーザーにテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、一部のテストが失敗したりする場合があります。問題ありません。Copilot はエラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のセッションが完了した Chat 画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やその他の AI ツールが生成したコードを必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. すべてが動作し、コードも確認できたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って、Tailspin Toys の Web サイトに新しい機能を追加する方法を確認しました。この演習では、次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ レイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う方法。
- Copilot Agent Mode が生成した変更やテストをレビューしてから、コードベースにマージする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼します。コードを作成する際は、次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズを利用者が選べるようにするか、固定値にするかなど、ページネーションの実装方法を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- GitHub Copilot にコンテキストを伝えながら変更を依頼し、反復が必要になる場合があります。Copilot を使う際の通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
