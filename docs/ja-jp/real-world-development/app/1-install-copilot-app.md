---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使用するデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分だけで作業するのではなく、複数のエージェントをそれぞれ分離されたワークスペースで並列に指示し、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトの準備ができたので、次はアプリをインストールして、リポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログなど、ワークスペースの構成を確認します。
- クイックチャットを試して、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するため、AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の案内に従って認証します。データ所在地に対応する GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディング中にリポジトリを選択できる場合は、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリの選択をスキップし、オンボーディング後にプロジェクトを追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに一覧にある場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた Owner とリポジトリ名を入力します。
9. サイドバーにリポジトリが表示されていることを確認します。このレイアウトでは `tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめて表示され、その下に個々のセッションが並びます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには次の領域があります。

- **New** - ここから Copilot との新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じてプルリクエストを表示します。アプリを離れずに PR の参照や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - 同じように Issue を表示します。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。To-do リストの管理、定期的なプロジェクトの保守、その他の面倒な作業を任せるのに役立ちます。振り返りでは次のステップとして紹介します。ワークショップ内の演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントによって、Copilot app に機能を追加します。Playwright MCP を設定する際に使用します。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの軽い会話です。最初のチャットを開始すると表示されます。見当たらない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。
- **Sessions** - リポジトリフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリビューが開き、その下のセッションを選ぶと会話に戻ります。各セッションは分離されたワークスペースで動作するため、変更を競合させずに複数のセッションを同時に実行できます。星評価を追加する際に、最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションを開始する起点にもなります。小規模な最初の変更が終わったら、フィルター機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。他のリポジトリの作業項目を表示するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問してみましょう。その場合は **quick chat** が適しています。Quick chats ではブランチや worktree を作らずに質問やブレインストーミングができます。セッションを必要としない、簡単な使い捨ての質問に最適です。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。その後、**Chats** が表示されます。
2. アプリのセッションの仕組みを質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の git worktree で実行されるため、変更を競合させずに複数のエージェントを並列実行できることがわかります。会話はいつでも続けられ、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットを使って、手早く質問する。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
