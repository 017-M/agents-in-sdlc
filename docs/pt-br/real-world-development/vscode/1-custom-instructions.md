---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações de contexto —, disponibilize essas informações. Há várias maneiras de compartilhar contexto com o Copilot. Entre as principais estão os [arquivos de instruções][instruction-files], que fornecem orientação sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções aplicados a caminhos específicos.
- gerar a primeira parte dos dados para filtragem (um auxiliar de editoras) usando as instruções atuais.
- adicionar um novo padrão global ao arquivo `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código gerado seguir o novo padrão.
- fazer commit das instruções e da parte de filtragem na branch `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a tendência de mudança após a atualização das instruções, não obter exatamente o mesmo código.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que explique sua finalidade.

Com arquivos de instruções, você fornecerá ao Copilot as informações necessárias para que ele realize as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas dão contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos do seu código. Elas podem especificar convenções, bibliotecas e tipos de comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em todas as solicitações feitas no repositório. Ele deve conter informações sobre o projeto que sejam relevantes para a maioria das solicitações, como a pilha tecnológica, uma visão geral do que está sendo criado, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que fornecem diretrizes para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para complementações de código ou sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [aceita arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos no projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo — como testes unitários, componentes Astro e camada de dados — ou de tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve (e não deve) ser escrito.

Não existe uma única maneira correta de criar arquivos de instruções nem de usar IA. Experimente para descobrir o que funciona para seus ambientes e cenários.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, você encontrará arquivos para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a criar arquivos de instruções. Cada ambiente oferece essa opção de uma forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição sobre cada ambiente explicará quando isso for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório: há um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Leia as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e leia os links para outros arquivos de instruções e arquivos existentes. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos para o Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos nos arquivos.

## Crie uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch nova:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará o que selecionar.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar o resultado.

1. Feche as guias do editor abertas em exercícios anteriores para que o Copilot considere apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na lista de agentes da visualização do Chat, selecione **Agent** para que o Copilot possa alterar arquivos.

   ![Seletor de agente na visualização do Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, muitas vezes ao arquivo do auxiliar e aos testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicadas a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a melhoria de consistência depois da atualização das instruções.

## Adicione um novo padrão do repositório

Como vimos, `.github/copilot-instructions.md` fornece orientações gerais ao Copilot. Vamos documentar os padrões do projeto para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ainda não há uma regra para a documentação no código, o que explica a ausência de comentários no auxiliar gerado.
3. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Os arquivos de instruções podem ficar na raiz do repositório, em `.github/copilot-instructions.md`, para orientações gerais, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo do repositório é o lugar adequado para padrões globais, como a regra de comentários de documentação que você acabou de adicionar.

## Envie o mesmo prompt novamente e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão de projeto e a aplicá-lo a código real, que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são recursos do repositório e devem ser gerenciados com o controle de versão, como qualquer outro arquivo.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme se as instruções e as alterações no auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para criar o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a branch enviada, crie um pull request e associe-o à issue do backlog sobre a atualização dos arquivos de instruções. O Copilot pode encontrar a issue, criar um PR que a encerra e mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot atua no GitHub em seu nome. O template do projeto já configura as ferramentas do GitHub no espaço de trabalho, então não é necessário configurá-las. Na primeira vez que o Copilot usar uma delas, o VS Code poderá pedir que você entre no GitHub.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot para encontrar a issue relacionada à atualização dos arquivos de instruções e criar um PR a partir da branch atual. Explique que o PR encerra essa issue e também inclui a base do auxiliar de editoras para o trabalho de filtragem:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot procurará a issue e criará o PR.
5. Quando solicitado, revise a ação do GitHub e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot para mesclá-lo e voltar à branch `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado, revise as ações do GitHub e os comandos de shell e selecione **Allow** se estiver de acordo.

Você acabou de criar e mesclar um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um padrão global ao arquivo `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código proposto seguir o novo padrão.
- fazer commit das instruções e da base do auxiliar na branch `main`, para que o próximo exercício possa usá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson], enquanto o Copilot adiciona um recurso ao projeto.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
