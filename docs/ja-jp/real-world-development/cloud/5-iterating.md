---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を構築し、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

これまでに何度も確認したように、生成 AI が加わっても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスを実行します。そのため、チームでのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した変更を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を維持するためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権は作業に使うブランチに**限定**されます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR)、つまりコードにドキュメントを追加する変更を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定される**動作です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のコメントや、その他のドキュメントが追加されたコードを確認します。変更内容は状況によって異なります。

    変更を確認するときは、TSDoc コメント、TypeScript の規約、ファイルヘッダーに注目してください。演習 2 の最初に確認したカスタム指示ファイルから反映されています。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業することは、一方的なやり取りではありません。チームメンバーと同じように、プルリクエストやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確率的に変わるため、依頼内容を一律に指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例、またはコードベースに追加したいドキュメントについての提案を使って、**@copilot** をメンションし、生成されたドキュメントへの変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。更新を行う新しいセッションが始まります。
3. **Back to pull request** を選択して、プルリクエストに戻ることもできます。

    ![Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

納得できるまで、Copilot とのやり取りを続けられます。問題がなければ、ドラフト PR をレビュー可能な状態にし、`main` ブランチにマージします。

![ドラフト PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます。後者を依頼する場合は、カスタム指示を更新する**前**にタスクを割り当てたことを思い出してください。これまでと同様に **Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、カスタムのアクセシビリティエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control から追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューします。特に次の点を確認してください。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - ハイコントラストモードとライトモード用の CSS またはスタイルの変更。
   - ARIA ラベルやキーボード操作など、アクセシビリティ属性。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

カスタムアクセシビリティエージェントによって、Copilot がアクセシビリティのベストプラクティスに沿って機能を実装できました。アクセシビリティに関する問題や改善点があれば、前の PR と同様にコメントで **@copilot** にメンションして変更を依頼できます。

## 任意の演習 — さらに作業を委任する

cloud agent は、実際のバックログ項目を渡し、その間に別の作業に取り組めるときに効果を発揮します。練習として、リポジトリに Issue を追加し、Copilot に割り当ててみましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、後で cloud agent の出力に指示が反映されることを確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使用**し、ハイコントラストとライトモードのトグルを追加しました。
- **Copilot agents ページを Mission control として使い**、進行中のアクセシビリティセッションを監視してガイドしました。
- **cloud agent のプルリクエストをレビューして改善**し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を発展させることができます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP の統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次のアイデアを試してみましょう。

- リファクタリングやテスト範囲の拡大を Issue にして、cloud agent に割り当てます。
- 別の分野に特化したカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
