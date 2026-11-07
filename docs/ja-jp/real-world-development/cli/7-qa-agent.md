---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、明確な QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に照らして報告内容を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫した方法で確認したいと考えています。カスタムエージェントを使うと、QA プロセスを統括し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する特定の役割に特化した Copilot です。プロファイルには、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの規約を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらの機能を使って要件を評価し、結果を報告するよう Copilot に指示します。ほかのカスタマイズを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。QA が実施するチェックと、作業時の制約をプロファイルに定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA の流れと制約をプロファイルに反映したことを確認します。これにより、検証だけを依頼したのに不完全または広範すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名がわかりやすくなります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が証拠を示し、実装コードを変更する前に許可を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. 必要な役割や制約が抜けている場合は、続ける前に既定のエージェントに修正を依頼します。

## Issue に照らして QA を実行する

Copilot CLI は、会話を始めるときにプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開して QA を選択します。これにより、すでに共有している Issue と計画の内容を使えます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで、QA エージェントに機能を確認してもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業する様子を確認します。
4. 完了したら、エージェントのレポートを読みます。

## まとめと次のステップ

再利用可能な専門エージェントをワークフローに加え、その結果を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、[機能 PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
