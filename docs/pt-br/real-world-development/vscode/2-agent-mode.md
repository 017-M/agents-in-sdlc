---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de uma aplicação costumam exigir alterações em vários arquivos e operações como executar testes. Em geral, uma pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as mudanças, executar testes, depurar, descobrir qual arquivo ficou de fora, fazer outra alteração e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente foi criado para trabalhar com mais autonomia no IDE. Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora tenham nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências distintas. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você criou um auxiliar para editoras; agora, concluirá o restante do trabalho na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele lista jogos e exibe seus detalhes. A aplicação Astro gera páginas HTML estáticas durante o build. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM — não há uma API de back-end separada nem framework de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` do Astro, usando as APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev`, que inicia o servidor de desenvolvimento da aplicação Astro. Execute-a no GitHub Codespaces:

1. Volte ao Codespace. Você deverá estar na branch `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um banner com a URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço cria um túnel privado para o servidor em execução no Codespace.

### Explore o site

Com o site em execução, explore suas funcionalidades:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará o que selecionar.

A primeira versão do site funciona, mas queremos adicionar recursos. Vamos começar examinando o backlog. Ao criar seu projeto a partir do template, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre essas issues.

1. Na lista de agentes da visualização do Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e chama ferramentas.

   ![Seletor de agente na visualização do Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criá-lo para tentar manter as sugestões relativamente consistentes e proporcionar uma boa experiência. Como LLMs são probabilísticos, as sugestões podem ser diferentes das mostradas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda assim concluir a tarefa.

3. Envie este prompt para perguntar sobre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista criada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, examine o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que descreve como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Como os caminhos são relativos ao arquivo de instruções, é possível reutilizar e dividir conjuntos maiores de orientações e apontar o Copilot para exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento dos filtros.
- atualizar a página de listagem de jogos para incluir os controles de filtragem.

Antes de mesclar tudo ao projeto, os testes precisam ser executados e aprovados. O modo de agente pode cuidar dessas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes da visualização do Chat.

   ![Seletor de agente na visualização do Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Use este prompt para pedir ao Copilot que implemente o recurso descrito na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos da funcionalidade, incluindo os auxiliares da camada de dados, a interface e os testes. Depois, ele começará a editar arquivos e executar testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. O processo pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o trabalho, podem aparecer os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as alterações; não é necessário selecionar esses botões enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e alguns testes podem falhar durante o processo. Isso é esperado: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- o modo de agente explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implemente paginação

À medida que a lista de jogos cresce, será necessário oferecer paginação. Use o que aprendeu neste exercício e peça ao Copilot que implemente esse recurso. Considere:

- seguir as práticas recomendadas existentes, inclusive as instruções do projeto.
- decidir como a paginação deve funcionar, por exemplo, se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer contexto e orientações suficientes no prompt.
- iterar com o GitHub Copilot, pedindo alterações e oferecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Copilot: modos ask, edit e agent — o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
