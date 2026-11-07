---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do template, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e se integra nativamente ao GitHub, para que repositórios, branches e pipelines de CI funcionem sem configuração adicional. Ele foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo, cada um no próprio espaço de trabalho isolado, em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, instale o aplicativo e conecte o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo por meio do repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo template.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Como você pode imaginar, a primeira etapa é instalar o aplicativo. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar a conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe seu host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do template. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a configuração inicial.
7. Procure o repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, **Add GitHub repository**. Informe o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. Neste layout, pastas de repositórios, como `tailspin-toys`, aparecem agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza os recursos em algumas áreas na barra lateral:

- **New** — inicie uma nova sessão de chat com o Copilot.
- **Pull requests** — acesse seus pull requests pela integração nativa com o GitHub. Aqui você pode pesquisar e filtrar PRs, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues** — acesse suas issues da mesma forma. Você pode pesquisar e filtrar issues e iniciar uma sessão a partir de uma delas.
- **Automations** — tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas ajudam a gerenciar listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. A lição de encerramento apresenta links para esse recurso como próximo passo, não como outro exercício do workshop.
- **Customize** — adicione recursos ao aplicativo Copilot na forma de servidores MCP, plugins, skills e outros componentes. Você usará esta área para configurar o MCP do Playwright.
- **Chats** — conversas rápidas para perguntas e brainstorming que não precisam de uma branch ou de um espaço de trabalho próprios. Esta seção aparece depois que você inicia o primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions** — agrupa pastas de repositórios e sessões de agentes. Selecione o nome de um repositório, como `tailspin-toys`, para abrir sua visualização, ou selecione uma sessão abaixo dele para retomar a conversa. Cada sessão é executada em um espaço de trabalho isolado, então você pode executar várias ao mesmo tempo sem conflito. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, pergunte. Ele ajudará você.

### Localize o backlog criado pelo template

Quase todo projeto tem um backlog, e a Tailspin Toys não é diferente. Vamos explorar as issues criadas quando você criou o repositório a partir do template.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual. Depois, selecione **Issues** na navegação superior do repositório.
2. Localize estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler seus detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma pequena alteração inicial.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo*. Um **chat rápido** é ideal para fazer uma pergunta ou pensar em ideias sem criar uma branch ou worktree — uma pergunta rápida e descartável que não exige uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar o primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como as próprias sessões funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em sua própria worktree isolada do Git — isso permite executar vários agentes em paralelo sem conflito entre alterações. Você pode continuar a conversa ou iniciar um novo chat quando quiser.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto por meio do repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog criado em **Issues**.
- usar um chat rápido para fazer uma pergunta descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
