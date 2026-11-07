---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデルの選択、セッションの共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する仕組みを確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、クラウドエージェントへの委任に使える任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキストウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認したり、手動で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きな違いはないことがあります。

> [!NOTE]
> Copilot CLI はコンテキストウィンドウが埋まると、自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。無関係なタスクに切り替える場合は、`/clear` または `/new` で会話をリセットできます。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用できるモデルを表示して選べます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと利用状況を確認します。
3. 現在のモデルを使うか、別のモデルを選びます。<kbd>Esc</kbd> を押すと一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力して、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合った方法を選びます。たとえば `/share file` は、ローカルの Markdown ファイルにエクスポートします。
3. 誰かに送信したり公開したりする前に、エクスポートした内容を確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業の拡張や委任

ワークショップの基本的な内容は完了しました。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に分離された Git worktree を作成します。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期で処理します。エージェントはプルリクエストを開くことがあります。

これらのコマンドは、追加の worktree やリモート作業を作ることがあるため、任意です。使う場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業をさらに学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI でスラッシュコマンドを使うと、設定やセッションの共有、内部動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot がコンテキストを管理する仕組みを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、クラウドエージェントへの委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は、[学んだことを振り返り][next-lesson]、これからの学習方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
