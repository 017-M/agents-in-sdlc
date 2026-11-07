---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示ファイルの変更前後を比較するプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムで影響を比較するのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにして、モダンなデザインにします。
- コードのドキュメントには TSDoc のコメントを使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを記述します。

指示ファイルを使うと、Copilot がこれらの開発手法に沿って作業するために必要な情報を提供できます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、スタイルに合った提案やコードを生成できるようにします。好みのコーディング規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルだけに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求とともに Copilot に送られる単一の指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティス、全般的なガイダンスなど、多くのチャットや CLI の要求に共通する情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用できます。TypeScript や Astro などの言語や、UI コンポーネントやユニットテストの作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの規格に準拠した指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な使い方が含まれています。基本的な方針は次のとおりです。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全般的なコーディング規約など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなど、ファイルの種類や特定のタスクに関する指示を記述します。
- 自然な言葉を使い、明確なガイダンスを記述します。コードのよい例と避けるべき例も示します。

指示ファイルの作り方に唯一の正解はありません。AI の使い方と同じように、実際に試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応する指示ファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で方法が異なります。該当する場合は、各環境の演習で説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアとなる `copilot-instructions.md` と、タスクに応じて使う複数の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とする glob を指定し、指示を適用するファイルを決めています。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用しやすい小さなファイルに分け、コード生成時に参考にする例を Copilot に示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する要件はありません。次の手順で、TSDoc コメントとファイルヘッダーに関する規則を追加します。

## リポジトリの新しい規約を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するために、リポジトリのコーディング規約を記述しましょう。

Copilot cloud agent は、Issue が対象とするブランチから指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。cloud agent が新しい指示を使えるように、変更を `main` にマージします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、プロジェクトレベルの情報を Copilot に提供します。コード提案を改善するため、リポジトリのコーディング規約を記録します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング規約が記載されていますが、コード内ドキュメントの規則がまだありません。そのため、先ほど生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の規約の直後に、次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを記述するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前の演習で確認したように、指示ファイルは、リポジトリ全体のガイダンスには `.github/copilot-instructions.md`、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使えます。今回追加したドキュメントコメントの規則のような、プロジェクト全体の規約はリポジトリレベルのファイルに記述します。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。そのため、cloud agent が新しい指示を使えるように、変更を `main` にマージします。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成する手順を省略し、`main` に直接コミットします。ここでは実際のプロジェクトに近い手順を体験するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。このハーネスの後半で cloud agent が作成したプルリクエストを確認すると、追加した規約がどのように使われたかを確認できます。

次は、Copilot cloud agent に[最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
