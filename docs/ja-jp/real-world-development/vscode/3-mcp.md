---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使って、フィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作してもらい、機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- ワークスペースに Playwright MCP サーバーを追加します。
- Copilot に Playwright MCP サーバーを使わせて、ブラウザーでフィルター機能を手動でテストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にある機能を利用すると、さらに強力になります。Copilot が外部ツールやサービスと通信するために使う標準規格が Model Context Protocol (MCP) です。

![エージェントモードの内部構成と、コンテキスト、LLM、MCP サーバーおよび VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

エージェントは MCP サーバーを通じてツールやリソースにアクセスします。MCP サーバーは、AI エージェントと外部のツールやサービスの間を仲介します。既存の API や NPM パッケージのようなローカルツールとの通信も管理します。各 MCP サーバーは、それぞれ異なるツールやリソースを提供します。

よく使われている MCP サーバーを紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを AI エージェントが行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動操作を提供します。Web ページへの移動、フォームの入力、ボタンの選択などを AI エージェントが行えます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub が公開している [MCP レジストリ](https://github.com/mcp)を使うと、エコシステム内のサーバーを見つけて、プロジェクトに貢献できます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱ってください。利用する前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを利用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot がバックログの Issue を読み取れました。この設定は[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code が HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるようになります。機能をテストするために必要なツールです。

1. `.vscode/mcp.json` で `github` の隣に `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザーを画面に表示せずに実行します。デスクトップ環境がない codespace では、このオプションが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは、エンドツーエンドテストに Playwright をすでに使っているため、通常は Playwright のブラウザーもインストールされています。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して、もう一度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うときに、サーバーを起動して、信頼するかどうか確認します。

1. `.vscode/mcp.json` で `playwright` の設定の上に **Start** が表示されています。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するか確認された場合は、設定を確認して、起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼の確認を求めます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して変更を確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用し、結果を読み取らせることができます。その後、確認内容をまとめて報告します。会話を中断せずに、機能が想定どおりに動作するか確認できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。そのため、エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を基に推論します。支援技術と同様の情報を使うため、簡単な機能確認と同時にアクセシビリティの基本も確認できます。

サーバーに接続し、アプリを起動した状態で、Copilot に作成したフィルター機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を実行して、確認結果を報告します。Issue の受け入れ基準とレポートを比較してください。問題がある場合は、追加の質問をするか、プルリクエストを開く前にコードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して、もう一度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビューを受けるためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは、github.com や GitHub コマンドライン インターフェイス (CLI) などから作成できます。ここでは GitHub Copilot を使って、関連する Issue を探し、関連付けた PR を作成してもらいましょう。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使えるよう、エージェントの一覧で **Agent** が選択されていることを確認します。
3. Copilot に次のプロンプトを送信し、PR を作成してもらいます。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めたり操作したりするときに **Continue** を選択して続行します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の案内に従って許可します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- ワークスペースに Playwright MCP サーバーを追加しました。
- 出荷前に、ブラウザーでフィルター機能を手動でテストするためにサーバーを使いました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成しましょう。

## リソース

- [MCP とは何か。なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
