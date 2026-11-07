---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores exigem um processo mais robusto. O aplicativo GitHub Copilot foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a desenvolver o que é necessário da maneira adequada. Nesta lição, você começará a seguir um processo típico de desenvolvimento orientado por agentes: partirá de uma issue para criar um recurso, verificará o código e o comportamento e, por fim, mesclará a alteração ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante o fluxo de trabalho do recurso. Normalmente, você usaria sessões ou PRs diferentes para tipos distintos de alteração, mas faremos um atalho para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar o recurso no modo **Autopilot**.
- revisar o código.
- validar manualmente o recurso em um canvas de navegador.

Ao continuar esse trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas decisões como a combinação de categorias precisam ser acordadas antes de começar a programar. Você usará o modo Plan para resolver essas questões e, depois, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, eles ficam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo parecido com este:

1. Abrir uma issue registrada com os detalhes do trabalho.
2. Planejar o que será criado.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar a alteração.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Ao seguir esse processo padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelas mesmas verificações que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você o revisa e aprova antes de permitir que o agente o execute.
- **Autopilot**: o agente trabalha de forma totalmente autônoma, escrevendo código, executando testes e iterando sem aguardar orientação.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Antes de começar, confirme que o PR da avaliação por estrelas foi mesclado e que sua `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já existe no backlog; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o `PATH` ou a configuração do gerenciador de versões e feche e reabra o aplicativo completamente. Repita a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje o recurso de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele cobre a filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Discuta qualquer comportamento pouco claro, como combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha-o focado em implementar e testar a filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e os esclarecimentos acordados para a validação posterior.

## Aprove o Autopilot explicitamente

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme que o indicador de modo ativo mostra **Autopilot**.

O Copilot começará a implementar o recurso. Ele seguirá o plano, gerará código e executará testes de forma iterativa.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare apenas os servidores que você iniciou.

## Revise e verifique a implementação

Assim como qualquer código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e disponibiliza uma versão de pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Envie este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme que os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme os resultados.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem mescle o PR da filtragem. Mantenha esta sessão, a worktree e a branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará as melhorias da skill e do agente de QA antes de criar e mesclar o PR combinado na Lição 8. Depois de parar o servidor, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou o recurso no modo **Autopilot**.
- revisou o código.
- validou manualmente o recurso em um canvas de navegador.

Em seguida, você aprenderá mais sobre a geração de código e como garantir que ela siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
