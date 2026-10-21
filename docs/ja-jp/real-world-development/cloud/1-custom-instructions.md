---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここではプロンプトの「変更前と変更後」を比較しません。Copilot cloud agent は GitHub issue に対して非同期に動作するため、リアルタイムで並べて比較するのが困難です。この環境で cloud agent が作成したプルリクエストを後でレビューするときに、指示ファイルの影響を確認できます。

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
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示も確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここではリポジトリのルートを基準に適用対象のファイルを決める glob が設定されています。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。他の指示ファイル (`unit-tests.instructions.md` など) や、プロジェクト内の既存ファイルへのリンクを確認してください。これにより、大きな指示セットを小さな再利用可能なファイルに分けたり、コード生成時に Copilot が参考にする例を指定したりできます。ここにあるパスはリポジトリのルートではなく、指示ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順では、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、プロジェクト全体の情報を Copilot に提供する `.github/copilot-instructions.md` です。コード内のドキュメントを TSDoc コメントとファイル レベルのコメント ヘッダーで記述するルールを追加します。以降の演習で issue を割り当てると、cloud agent はこのルールを参照します。

編集前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み込みますが、変更を明確にコミットするため、ここではブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチから指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が更新した指示を使うには、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Copilot に割り当てる issue を処理する際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略し、変更を `main` に直接コミットしてもかまいません。ここでは、実際のプロジェクトでの方法を体験できるようブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。以降の演習で cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認できます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
