---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、更新し忘れたファイルを見つけ、さらに変更を加える、といった作業を繰り返します。作業は次々に増えていきます。

こうした作業に役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとクラウドエージェントは異なる用途向けに作られています。エージェントモードは IDE 内で動作し、すばやいフィードバックとやり取りを実現します。クラウドエージェントは、Issue やプルリクエストを通じてやり取りしながら、チームの一員のように非同期で作業します。

この演習では、次の方法を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルにわたって変更を行います。
- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装します。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。次は、Copilot エージェントモードを使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するため、Tailspin Toys の Web サイトを確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧を表示し、ゲームの詳細を確認できます。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace 上で実行します。

1. codespace に戻ります。前の演習の後なので、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、codespace 内に新しいターミナルを開きます。
3. フィルター機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか確認するダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> codespace で localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーを実行している codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードやモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの最初の実装はすでに動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると Issue のバックログが自動作成されます。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェントドロップダウンで **Agent** を選択します。エージェントモードは自律的に計画し、複数ファイルに変更を加え、ターミナルコマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が、特定のモデルを推奨するものではありません。このワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案されるコードに一貫性を持たせ、よい体験を提供することが目的です。ただし、LLM は確率的に動作するため、ワークショップの例とは異なる提案が表示される場合があります。それは正常であり、想定される動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. Copilot に次のプロンプトを送信し、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと思われる項目を基に優先順位も付けています。

## 指示ファイルを確認する

Copilot にコードを生成してもらう前に、作業のコンテキストを提供する指示ファイルを確認します。Web サイトに機能を追加するときの方針を定めた [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加する際の全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルを基準とする相対パスです。指示を再利用しやすい小さなファイルに分け、複雑さを抑えながら、参考例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルター処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

また、コードベースにマージする前に、テストが成功する必要があります。Copilot エージェントモードにこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** に設定されていることを確認します。
5. 次のプロンプトを使い、バックログ内の関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを特定します。データレイヤーのヘルパー、UI、テストを探してから、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクの複雑さやコードベースの規模によっては数分かかります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が終わると、すべての変更に対して **Keep** または **Undo** を選択できるようになります。作業中に選択する必要はありません。

7. Copilot に確認されたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了し、エラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行しているブラウザーに戻り、新しい機能を確認します。
11. すべてが動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに新しい機能を追加しました。この演習では、次の方法を学びました。

- GitHub Copilot エージェントモードを使って、UI とデータレイヤーに新機能を実装する。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルにわたって変更する。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習: ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装してもらいましょう。次の点を検討してください。

- 既存の指示ファイルなど、プロジェクトのベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスを、プロンプトに含めます。
- Copilot と何度かやり取りして、追加の変更やコンテキストを伝える必要がある場合があります。これは Copilot を使うときの通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
