---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前のいくつかの演習では、ユーザーエクスペリエンスの改善と機能追加を目的に、GitHub Copilot cloud agent に 3 つの異なるタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。GitHub.com の[エージェントページ][agents-page]もその 1 つです。この Mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を確認したり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションをガイドし、機能を追加で依頼します。

## シナリオ

ハイコントラストモードを作成するようエージェントに依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイルを更新し、切り替え機能を追加しているため、同時に実装するのが適切です。ハイコントラストに加えてライトモードも追加されるように、エージェントの作業をガイドします。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左側のペインにあるタスク一覧を確認します。次のように、Copilot に割り当てたタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した操作を確認します。次のような操作があります。
    - リポジトリからコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するために、さまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業の進捗をさらに把握できます。

## cloud agent をガイドする

実行中のタスクを確認できました。次は、Copilot がハイコントラストモードに取り組む間にライトモードのトグルも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページに表示された cloud agent タスクと、アクティブなセッションをガイドするダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業に取り込むことを確認します。

## Copilot に作業を続けてもらう

更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼が作業に取り込まれます。

前と同じように完了まで数分かかります。ここで、これまでに学んだことや確認したことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent の監視とガイドの中心となる、Copilot agents ページを確認しました。この Mission control では、アクティブなタスクと完了したタスクを追跡し、実行中の作業を確認し、進行中のタスクの範囲を変更したり追加の指示を送ったりできます。

次の内容を確認しました。

- Mission control とエージェントページを使って cloud agent のタスクを監視する。
- 進行中のセッションに追加の機能を依頼する。

Copilot がアクセシビリティ機能を実装する間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
