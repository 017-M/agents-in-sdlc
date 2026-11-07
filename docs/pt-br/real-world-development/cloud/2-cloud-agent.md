---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações enfrentem algum tipo de dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em atividades de maior impacto.

Neste exercício, você explorará como usar o agente de nuvem do Copilot para:

- personalizar o ambiente usado para gerar código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas para resolver. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas da aplicação.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros títulos da mesma categoria, permitindo que as pessoas continuem explorando. Por enquanto, a equipe quer apenas uma implementação que permita testar a experiência de usuário. Isso é um impedimento no momento, mas há outras issues com prioridade maior.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e são boas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades; depois, você volta para revisar o trabalho.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias maneiras de delegar tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois que termina. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo tarefas que parecem simples podem ser complexas. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Considere-o um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas deste repositório

Em exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu a atividade prática sobre instruções personalizadas, viu o arquivo `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para consultá-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções já incluídos no repositório. Depois, será mais fácil identificar a influência deles.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace:

- `.github/copilot-instructions.md` — leia a seção **Code standards**, principalmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe se o pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos nos arquivos. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente requer um ambiente específico e scripts de configuração para instalar as ferramentas necessárias. Isso também se aplica ao Copilot, que executa tarefas de engenharia de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente. Você pode personalizá-lo com um [workflow de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes de o agente começar a trabalhar. Isso permite disponibilizar as ferramentas e dependências necessárias. O arquivo já vem configurado para facilitar o laboratório e garantir que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicação - Node.js (agora, toda a aplicação usa Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo segue o formato dos outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar que o Copilot a execute.

## Adicione documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ser ignorado, reduzindo a produtividade e dificultando a manutenção ou a integração de pessoas desenvolvedoras. O Copilot é útil para criar documentação, por isso esta é uma boa tarefa para delegar ao agente de nuvem. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) criado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se nenhum PR aparecer na lista, aguarde um ou dois minutos e atualize o navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito), analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o agente pretende realizar.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve a tarefa: ele explora o código, entende o estado atual, faz uma pausa para pensar e definir uma abordagem e, então, cria código.

Isso pode levar alguns minutos. Uma das principais vantagens do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso e atribuir outra tarefa ao agente; enquanto ele trabalha, você poderá desenvolver funcionalidades na aplicação.

## Adicione uma seção de jogos relacionados à página de detalhes

Como vimos, uma das vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto o agente cuida de outro. Mesmo que a seção de jogos relacionados não demore muito, você pode usar esse tempo em outras atividades. Vamos atribuí-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, um emoji 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem atua de forma semelhante a uma pessoa desenvolvedora, não é necessário acompanhar cada etapa: você pode revisar o trabalho quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Use-o para resolver dívidas técnicas, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalização do ambiente usado para gerar código.
- execução segura de operações.
- importância de definir o escopo das issues com clareza.
- atribuição de issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
