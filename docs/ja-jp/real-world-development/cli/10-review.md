---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと登録済みのフィルター機能の Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証の証拠を確認しました。
- フィルター機能の変更全体をレビューし、Agent Merge を許可して PR をマージしました。
- コンテキスト、モデル、共有、クラウド委任に使えるスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには、更新された `main` からそれぞれ別のブランチで作成する、2 つの PR があります。

1. **星評価:** ゲームカードに、既存の `starRating` と未評価であることを示す状態を表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告します。QA エージェントは、要件とテスト範囲のレビューをこれらの結果とまとめ、PR を作成する前の確認に役立てます。

追加のテストは、実際に不足している部分を補うものである必要があります。QA の実行時に新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証の証拠を確認してください。変更を加えた場合は、影響を受ける検証をやり直します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズは、チームのニーズに合わせて調整しながら、ほかの会話でも再利用できます。指示は規約を定め、スキルは繰り返し実行する作業を説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びましょう。**Plan** は実装前に方針を検討するとき、**Interactive** は対象を絞った変更を進めながら確認したいとき、**Autopilot** は範囲を明確にしたタスクを任せるときに適しています。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えることで、結果は大きく変わります。

## さらに試す

基本的なワークフローは完了しました。次の CLI 機能も確認してみましょう。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点から検討します。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。実際のコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで試してください。学んだことをチームで共有し、ほかの人からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
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
