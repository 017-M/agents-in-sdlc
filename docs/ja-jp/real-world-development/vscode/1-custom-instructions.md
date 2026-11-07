---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい規約に従う様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、一部の規約に従わない場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に出力が完全一致することではなく、動作の**傾向**がどう変わるかを確認します。

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
2. 簡単なプロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とする glob を設定し、指示を適用するファイルを決めています。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用しやすい小さなファイルに分け、コード生成時に参考にする例を Copilot に示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する要件はありません。次の手順で、TSDoc コメントとファイルヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードやモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再度送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認してコードを更新します。ヘルパーのファイルとテストが変更されることがあります。
6. 提案されたヘルパーが、型付けされた関数として `db` クライアントを最初の引数に取り、型付けされたパブリッシャーの配列を返すことを確認します。`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約が使われています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示の更新後に出力の一貫性が高まることが、この演習の目的です。

## リポジトリの新しい規約を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するために、リポジトリのコーディング規約を記述しましょう。

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト規約に従うよう指示し、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットする

指示ファイルも、ほかのリポジトリアセットと同じようにソース管理できます。指示をブランチにコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と、ヘルパーの作成に伴って Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けてプルリクエストを作成します。手動で作成する方法もありますが、GitHub ツールがプロジェクトに接続されているので Copilot に任せましょう。Issue を検索し、Issue をクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を行います。プロジェクトテンプレートでは、GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うときに VS Code からサインインを求められた場合は、案内に従って許可してください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルを更新する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新だけでなく、今後のフィルター機能の作業に使うパブリッシャーヘルパーの基礎も追加したことを説明します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドの内容を確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められたら、コマンドの内容を確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援を受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- コード生成のプロンプトを送信し、**既存の**指示を使った Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい規約が反映される様子を確認しました。
- 指示の更新とヘルパーの基礎をコミットして `main` に取り込み、次の演習で利用できるようにしました。

次は[エージェントモード][next-lesson]を使って、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot をカスタマイズするための指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
