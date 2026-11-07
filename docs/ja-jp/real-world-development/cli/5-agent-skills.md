---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。これまで、コードを手動で検証し、指示ファイルを使って規約に従わせました。では、テストや lint、継続的インテグレーション (CI) のほかの処理はどうでしょうか。

こうした作業には、**エージェント スキル**が適しています。スキルを使うと、Copilot が必要な処理を正しく実行する方法を理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめ、必要なときにエージェントが読み込めるようにします。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングのガイダンスを記述します。必要に応じて、スキルが使用するスクリプトやほかのリソースをサブフォルダーに置くこともできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルには、プロジェクトの既存コマンドを使うため、`SKILL.md` だけが含まれています。

スキルは、チームで共有して再利用するリポジトリ資産としてプロジェクトの `.github/skills` フォルダーに配置できます。また、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックの実行に使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明によって、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順をどのようにガイドしているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。まず、Copilot に 3 種類のチェックを実行してもらいましょう。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

どの処理を実行したか、結果が成功したか、ツールが実際に何を報告したかがわかるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを実行して、レポートがどう変わるか確認しましょう。Copilot CLI では、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 最初と同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. レポートの最後の部分を確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能 PR に含めます。次は、[Playwright MCP サーバー][next-lesson]を使って Copilot がサイトを直接操作できるようにします。

## ほかのスキルの例

次のコミュニティの例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート用スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
