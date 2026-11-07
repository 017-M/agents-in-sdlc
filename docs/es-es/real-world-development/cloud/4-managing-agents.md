---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se encargue de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Puedes gestionar las tareas asignadas al agente en la nube con varias herramientas, incluida la [página de agentes][agents-page] de GitHub.com. Desde Mission Control puedes consultar todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas e incluso orientar una sesión que sigue en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería conveniente añadir un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo la funcionalidad para alternar entre modos, tiene sentido incluirlo. Orientarás el trabajo del agente para que añada también un modo claro.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - clonar el código del repositorio.
   - configurar el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Permite consultar la PR y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto dependerá del nombre que haya usado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso también tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde Mission Control puedes seguir todas las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- has usado Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
