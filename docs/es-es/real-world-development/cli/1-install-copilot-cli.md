---
title: "Lección 1 - Instala GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y busca la incidencia de filtrado precargada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación basado en agentes que se ejecuta en el terminal. Te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Así puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- autenticarás tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- buscarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad a la terminal, donde muchos desarrolladores ya trabajan habitualmente. En esta lección instalarás y autenticarás la herramienta para dejarla lista para el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. GitHub Codespaces ya incluye Node.js, así que usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autentícate con GitHub

La primera vez que se inicia, Copilot CLI te pide que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones de autenticación con código de dispositivo para autorizar Copilot CLI.
3. Copilot CLI mostrará este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponda al repositorio de Tailspin Toys y selecciona **Yes, and remember this folder for future sessions** para responder que sí.

> [!NOTE]
> En un Codespace, quizá ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está listo.

## Familiarízate con la herramienta

Los comandos que escribes en el terminal normal se ejecutan directamente en el Codespace. Una vez iniciado Copilot CLI, los mensajes en lenguaje natural se envían al agente y los comandos de barra controlan la conversación.

1. Introduce `/model`, usa las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Introduce `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla a Copilot para comprobar que funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Introduce `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que busque la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI usa la rama que esté seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el acceso directo del taller

Normalmente, Copilot CLI pide permiso antes de usar herramientas fuera de los permisos establecidos. En este taller, lo reiniciarás con `--yolo`, un acceso directo aprobado por ti que elimina estas solicitudes de permiso dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual. No lo configures nunca como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de lectura y escritura de GitHub que se usarán en las lecciones siguientes para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta sencilla sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, usarás `copilot --resume` para volver a la misma conversación y rama de funcionalidad.

## Resumen y pasos siguientes

¡Enhorabuena! En esta lección:

- has instalado GitHub Copilot CLI con npm.
- has autenticado tu cuenta de GitHub.
- has confiado en el repositorio del taller y probado una conversación.
- has encontrado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [empezarás tu primer cambio concreto][next-lesson] y usarás Copilot CLI para mostrar una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
