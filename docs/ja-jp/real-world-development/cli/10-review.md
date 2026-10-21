---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調査し、あらかじめ用意されたフィルタリングの issue を見つけました。
- 星評価を追加し、ポートフォワードしたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを試しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使える slash コマンドを確認しました。

## 作成したもの

このワークショップには、更新済みの `main` から作成した別々のブランチで行う、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価されていない状態をゲーム カードに明示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot によるブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA では PR 作成前に、要件とテスト カバレッジの確認にこれらの結果を加えました。

追加するテストでは、実際に不足している点を補う必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前に、コードと根拠を確認し、変更があれば関連する検証結果を更新してください。

## ベスト プラクティス

Copilot に提供するコンテキストとツールによって、作業内容が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、対象を絞った変更に関与し続けるには **Interactive**、範囲の明確なタスクには **Autopilot** を使います。定型的な編集では高速なモデル、複雑な作業ではより高性能なモデルを選択します。

環境と同様に、コンテキストも重要です。何を、なぜ、どのように構築したいかを明確に伝えることで、出力は大きく変わります。

## さらに試す

中心的なワークフローは完了しました。Copilot CLI には、ほかにも便利な機能があります。

- `/review` を使って、コード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` を使って、問題について話し合い、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並列に調整します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。実際のコードや趣味のコード、長年作りたいと思っていた小さなアプリなどに活用してみてください。学んだことをチームと共有し、チームの経験からも学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業と比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
