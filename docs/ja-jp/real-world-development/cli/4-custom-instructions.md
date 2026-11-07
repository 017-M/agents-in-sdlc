---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要があるなら、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードの内容だけでなく、コードの構成方法も指定できます。フィルター機能を実装したところで、Copilot が参照した指示を確認し、ドキュメントの規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示ファイルと、パスに応じた指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング規約に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` にあるエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントには `Props` コントラクトの説明を記述し、関連するコードを変更したときはコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使うと、Copilot がこれらの開発手法に沿って作業するために必要な情報を提供できます。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求とともに Copilot に送られる単一の指示ファイルです。多くのチャットや CLI の要求に共通するプロジェクト情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに適用するガイドラインを記述します。

> [!NOTE]
> ほかの指示形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すぐに利用できる指示ファイルが含まれています。まず内容を確認してから変更を加え、指示による違いを見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターでターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示を適用するファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、コード生成の参考例を Copilot に示せます。

## チームの方針に合わせて指示ファイルを更新する

既存の指示ファイルはよい出発点ですが、追加できる内容があります。新しく生成する TypeScript に TSDoc コメントを含めるため、基本となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを探します。およそ 35 行目にあります。
2. 次の項目を、このセクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話を始めるときにリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しい指示を読み込むため、変更を保存した後、フィルター機能の会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新した指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新たに生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを使ってコンテキストを把握する仕組みを確認し、新しい規約を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示ファイルと、パスに応じた指示ファイルがエージェントに適用される仕組みを確認しました。
- 指示ファイルを更新し、コーディング規約を追加しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用できる `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
