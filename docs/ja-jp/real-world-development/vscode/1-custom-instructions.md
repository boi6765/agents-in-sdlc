---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成に関するガイダンスを Copilot に提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を適用したまま、フィルタリングの最初のデータ スライス (パブリッシャー ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が更新後の指示を利用できるよう、指示の更新とフィルタリングのスライスを `main` にコミットします。

> [!CAUTION]
> 生成されるコードが、設定した標準の一部に従わない場合があります。Copilot の出力は確定的ではありません。この演習の目的は、指示を更新した後の動作の**傾向**を確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

優れた開発チームとして、Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには必ずユニットテストを用意します。
- UI はダーク モードで、モダンな印象にします。
- TSDoc のドキュメント コメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行するための情報を用意します。

### カスタム指示

カスタム指示では、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。これにより、より適切な提案やコード スニペットを生成できるようになります。リポジトリ全体に適用する指示や、タスク単位で特定のファイルの種類に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot へのチャットや CLI のほとんどのリクエストに関係する技術スタック、開発内容の概要、ベスト プラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対する指針を記載します。TypeScript や Astro などの言語、UI コンポーネントや新しいユニットテストの作成などのタスクに関するガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` フロントマターを含む `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準に準拠した指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトの例は、一般的な方法を示しています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイルの種類 (ユニットテスト、Astro コンポーネント、データ レイヤー) や特定のタスクに対する指示を記載します。
- 自然な言葉を使い、指示を明確に保ちます。コードの良い例と避けるべき例を示します。

指示ファイルの作成方法は一つではありません。AI の活用方法と同様、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルをそろえることをお勧めします。このプロジェクトの指示を確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスクに対応するファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。操作方法は環境によって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init`)。それぞれの環境のレッスンで、該当する操作を紹介します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアの `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションと、プロジェクトの簡単な説明を確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示も確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここではリポジトリのルートを基準に適用対象のファイルを決める glob が設定されています。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。他の指示ファイル (`unit-tests.instructions.md` など) や、プロジェクト内の既存ファイルへのリンクを確認してください。これにより、大きな指示セットを小さな再利用可能なファイルに分けたり、コード生成時に Copilot が参考にする例を指定したりできます。ここにあるパスはリポジトリのルートではなく、指示ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順では、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、最初は現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で使ったエディター タブが開いている場合は、閉じて必要なコンテキストだけを残します。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。多くの場合、ヘルパー ファイルとテストの両方を変更します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付けされたパブリッシャーの配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規則によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的なため、指示がなくてもコメントが追加されることがあります。その場合も問題ありません。指示を更新した後に一貫性が高まることが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コーディング標準を文書化して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがまだないことを確認します。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に配置して全体的なガイダンスを提供することも、`*.instructions.md` ファイルとして特定の言語、ファイルの種類、タスクに関する指示を提供することもできます。今回追加したドキュメント コメントの規則のように、プロジェクト全体に適用する標準はリポジトリ レベルのファイルに記載します。

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話のバッファーをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot の対象ファイルにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム用のパブリッシャー データ アクセス ヘルパーです。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * すべてのパブリッシャーの ID と名前を含む一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns パブリッシャー オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準に沿うよう Copilot を導き、次の演習で使う実際のコードに適用しました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルはリポジトリ内の他の資産と同様に、ソース管理で管理します。変更をコミットし、ブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル (テストなど) が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで次のコマンドを実行し、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、プルリクエストを作成し、バックログにあるドキュメント標準の issue を関連付けます。手動で作成してもかまいませんが、GitHub ツールがプロジェクトに接続されているため、Copilot に任せることもできます。Copilot に issue を見つけて PR を作成し、issue をクローズしてからマージするよう依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 操作を依頼します。プロジェクト テンプレートでは GitHub のツールがワークスペースに接続済みなので、設定は必要ありません。Copilot が初めて GitHub ツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。指示に従って認証してください。後の演習で、この接続の仕組みである Model Context Protocol を学びます。

1. Codespace 内の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、今後のフィルタリング作業のために追加した新しいパブリッシャー ヘルパーの基盤の両方を説明する PR を現在のブランチから作成し、関連する issue を見つけるよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot がユーザーに代わって GitHub 操作を実行するための **Allow** を求めたら、コマンドを確認し、必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot がユーザーに代わって GitHub 操作やシェル コマンドを実行するための **Allow** を求めたら、コマンドを確認し、必要に応じて **Allow** を選択します。

GitHub Copilot の助けを借りてプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認しました。その後、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で作業を続けられるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体の新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
