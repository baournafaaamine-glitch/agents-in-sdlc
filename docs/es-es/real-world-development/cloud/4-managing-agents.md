---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado al agente en la nube de Copilot tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] en GitHub.com. Desde este espacio de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, revisar sus operaciones y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya estaba trabajando en actualizar los estilos del sitio y añadir opciones de alternancia, parecía lógico incluirlo. Orientarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Consulta el estado de las tareas que tiene asignadas el agente en la nube:

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque utilizado.

4. Observa también el panel de PR, que aparece a la derecha y permite ver la solicitud y los archivos modificados.

## Orientar al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste:

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que le haya asignado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber una opción para este modo que permita a los usuarios seleccionar el modo de visualización que prefieran.
   ```

   ![Tarea del agente en la nube con el diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando. Como antes, el trabajo tardará varios minutos; puedes aprovechar ese tiempo para repasar lo aprendido hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el espacio central para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde ahí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una tarea en curso para ajustar el alcance o añadir directrices.

En esta lección:

- exploraste Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigiste una sesión en curso para pedir una funcionalidad adicional.

A continuación, [iterarás sobre las solicitudes de incorporación de cambios que creó Copilot][next-lesson].

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
