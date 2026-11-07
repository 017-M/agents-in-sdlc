---
title: "Lección 1 - Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La [**aplicación GitHub Copilot**][about-copilot-app] es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de realizar todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Con Node.js instalado y tu copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar ese repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- conocerás el espacio de trabajo, incluido el backlog precargado por la plantilla.
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

El primer paso para usar la aplicación GitHub Copilot es instalarla. Hay versiones disponibles para Windows, macOS y Linux. Vamos a instalarla, autenticarnos y añadir el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si el proceso de bienvenida te permite seleccionar un repositorio, selecciona el repositorio de Tailspin Toys que has creado a partir de la plantilla. También puedes omitir la selección y añadir el proyecto al terminar.
6. Cuando se te solicite, elige un tema y completa el proceso de bienvenida.
7. Busca tu repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta que lo añadas de nuevo.
8. Si el repositorio aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Usa el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En la disposición que se muestra aquí, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza todo en varias áreas de la barra lateral:

- **New**: inicia aquí una sesión de chat con Copilot.
- **Pull requests**: muestra tus solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Desde aquí puedes explorarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra tus incidencias del mismo modo. Desde aquí puedes explorarlas y filtrarlas, y empezar una sesión a partir de una incidencia.
- **Automations**: contiene tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar tareas periódicas de mantenimiento del proyecto o delegar otras tareas repetitivas. El resumen las presenta como un paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar Playwright MCP.
- **Chats**: contiene conversaciones breves para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después del primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión debajo de él para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, de modo que puedes usar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir las valoraciones con estrellas.

Explorarás el espacio de trabajo a medida que avances en el taller.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puedes preguntarle y te ayudará a orientarte.

### Busca el backlog precargado

Casi todos los proyectos tienen trabajo pendiente y Tailspin Toys no es una excepción. Vamos a explorar el backlog que se creó al preparar el repositorio desde la plantilla.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio de Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no por el número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Empezarás por la incidencia de filtrado después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es usarla para aprender sobre la *propia aplicación*. Para eso sirve un **chat rápido**. Puedes hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo, así que es ideal para una consulta rápida que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar tu primer chat; **Chats** aparecerá después.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y pasos siguientes

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- conocer el espacio de trabajo y encontrar el backlog precargado en **Issues**.
- usar un chat rápido para hacer una pregunta breve y puntual.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la usarás para mostrar una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
