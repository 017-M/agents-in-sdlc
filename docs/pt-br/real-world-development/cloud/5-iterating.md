---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência de uso e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse o recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo modos de alto contraste e claro. Vamos revisar as alterações no código e, se necessário, orientar o Copilot para aprimorar o trabalho antes de habilitar a revisão pela equipe.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Portanto, vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente seguro. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem trabalha no GitHub Actions, que cria um ambiente separado e temporário para a tarefa.
- Os workflows do GitHub Actions precisam de aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Para isso, use a interface habitual de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo servidores MCP. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título parecido com **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a indicação **[WIP]**. Aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários TSDoc e outra documentação. As alterações exatas podem variar.

    Procure comentários TSDoc, convenções de TypeScript e cabeçalhos nos arquivos. Essas orientações vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para a aplicação Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do PR ou comentários em linha, assim como faria com qualquer pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto único a recomendar. Algumas ideias de solicitações:

- Adicionar cabeçalhos no início dos arquivos de código com uma breve descrição do conteúdo.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura da aplicação Astro.

1. Adicione um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione a guia **Files changed** para revisar as mudanças.

Continue iterando até ficar satisfeito. Depois disso, você pode converter o PR de rascunho em pronto para revisão e mesclá-lo à branch principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, vamos voltar ao PR que o Copilot criou para resolver a issue de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão ser adicionados à seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para a aplicação Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode selecionar essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Abra o terminal do Codespace e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode pedir alterações ao agente de nuvem iterativamente. Por exemplo, solicite que ele ajuste a quantidade de jogos relacionados ou adicione cabeçalhos e comentários TSDoc (lembre-se: a tarefa foi atribuída **antes** da atualização das instruções personalizadas). Para isso, adicione um comentário à guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, especialmente:
   - os componentes de interface para alternar entre os modos.
   - o uso do armazenamento local para manter as preferências.
   - as alterações de CSS ou estilo para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript que controla a alternância de modos.

6. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para a aplicação Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** selecione essa branch no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie a aplicação e teste os controles dos modos de alto contraste e claro no navegador. Confirme se eles funcionam e se as preferências continuam salvas após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou a implementação de acordo com as práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para pedir atualizações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para praticar, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para expressar interesse em apoiar o jogo na página de detalhes.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizadas incluídos no repositório** para ver como eles influenciam o trabalho do agente.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, incluindo controles para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e ajustou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar aprendendo, explore outros percursos que complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
