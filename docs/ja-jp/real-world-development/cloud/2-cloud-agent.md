---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発したコントラクターが、ドキュメントを十分に作成しませんでした。まず、アプリケーションのすべてのエクスポート関数に TSDoc のコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示します。完成度の高い実装はまだ必要ありません。UX の受け入れテストに使える状態にすることが目的です。現在はブロッカーですが、ほかに優先度の高い Issue があるため、後回しになっています。

どちらも、優先順位が下がりやすいタスクの例です。Copilot cloud agent に任せると、非同期で処理している間に別の作業に集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行します。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ることができます。割り当てられると、Copilot は進捗を記録するドラフトプルリクエストを作成し、環境をセットアップして、タスクに取り組みます。作業の途中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイドに使う方法を学びました。すでにカスタム指示のハンズオンを完了している場合は、`.github/copilot-instructions.md` が機能することを確認したはずです。まだの場合は、ここで内容を簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認すると、後で指示の効果を把握できます。

リポジトリの GitHub Web UI、または codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に該当するファイルに適用されます。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てるとき、プルリクエストに TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも、これらの指示を確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成する際は、通常、必要な開発ツールや依存関係を使えるように環境を準備します。Copilot にタスクを割り当てる場合も同様です。cloud agent もソフトウェアエンジニアのように作業します。

cloud agent は [GitHub Actions][github-actions] を作業環境として使います。`.github/workflows/copilot-setup-steps.yml` に設定した[特別なセットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要なツールや依存関係をインストールできます。ラボをスムーズに進めるため、このワークフローはあらかじめ設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローに似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot がプルリクエストの作業を始める前に、GitHub Actions でこのジョブを実行します。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の実行を待たずに正しく動作するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、ドキュメントがまったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に任せるのに適したタスクです。バックグラウンドで必要なドキュメントを生成してもらい、後の演習でレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。前述のワークフローで環境を準備し、プロジェクトを分析して、作業手順を決めています。

13. todo リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 最新の状態を表示するため、ブラウザーのウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールしながら、Copilot が問題を解決する様子を確認します。コードを調査して状況を把握し、計画を考え、判断して、コードを作成する流れを確認できます。

この作業には数分かかります。Copilot cloud agent の主な利点は、非同期で作業を実行し、その間にほかの作業に集中できることです。その機能を使い、別のタスクも Copilot cloud agent に割り当ててから、アプリの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームを追加する作業は短時間で終わるかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![目の絵文字で Copilot が Issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後にレビューできます。それでは、カスタムエージェントの作成と使用に進みましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期で作業を進められます。技術的負債への対応、新機能の作成、コードの別フレームワークへの移行などを依頼できます。

このレッスンでは、次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使います。Copilot cloud agent は[MCP サーバー][cloud-agent-mcp]も利用でき、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
