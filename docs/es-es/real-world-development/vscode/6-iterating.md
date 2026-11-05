---
title: "Ejercicio 6 - Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario. Has usado el modo de agente para añadir filtrado en el cliente y el servidor, Playwright MCP para probarlo manualmente en un navegador y un agente personalizado para implementar funciones de accesibilidad —opciones de alto contraste y modo claro—. También has orientado la sesión mientras estaba en curso para ampliar el trabajo. Ahora publicarás esos cambios locales y los revisarás como lo haría tu equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Hay que revisar todo lo que genera Copilot. Envía los cambios de accesibilidad desde tu Codespace, abre una solicitud de incorporación de cambios y revisa las diferencias antes de involucrar al resto del equipo.

## Publicar las funciones de accesibilidad

Los controles de alto contraste y modo claro que implementaste con el agente personalizado de accesibilidad en el [ejercicio 4][exercise-4] y el [ejercicio 5][exercise-5] están confirmados en el Codespace. Envíalos a una rama y abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarlos:

1. Vuelve a tu Codespace.
2. Abre la vista **Source Control** de VS Code.
3. Confirma que los cambios de accesibilidad están confirmados mediante commits. Si quedan cambios sin confirmar del ejercicio 5, añádelos al área de preparación y crea un commit con un mensaje descriptivo, como `Add high-contrast and light-mode toggles`.
4. Publica la rama con **Publish Branch** o con **...** → **Push**.
5. VS Code ofrecerá abrir la rama nueva en github.com. Acepta la propuesta o ve manualmente al repositorio y selecciona **Compare & pull request** en el aviso de la rama.
6. Escribe un título claro, como `Add high-contrast and light-mode toggles`, y una descripción breve que resuma qué se ha hecho y por qué.
7. Selecciona **Create pull request**.
8. Cuando se abra la PR, selecciona **Files changed** para revisar todo el trabajo. Presta especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
9. Vuelve a la pestaña **Conversation**.
10. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

11. Espera a que terminen los flujos de trabajo. Si todo va bien, deberían superarse correctamente.

> [!TIP]
> ¿Quieres una segunda opinión sobre el trabajo de accesibilidad? Menciona a `@copilot` en un comentario de la PR y pídele, por ejemplo, que revise si hay otros problemas de las Pautas de Accesibilidad para el Contenido Web (WCAG) o sugiera mejoras para la navegación con teclado. Copilot iniciará otra sesión para responder al comentario.

## Ejercicio opcional: seguir explorando en local

Trabajar de forma iterativa con un agente en el IDE es una habilidad que se desarrolla con la práctica. Puedes probar estas ideas desde VS Code:

- añadir un formulario de interés para patrocinadores en la página de detalles del juego;
- implementar la paginación en la página de lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos en `src/lib/`;
- ampliar el alcance del agente de accesibilidad; por ejemplo, para revisar el orden del foco del teclado en todo el sitio.

## Resumen

¡Enhorabuena! Has completado el entorno de VS Code. En este taller:

- **Usaste Playwright MCP para probar manualmente la funcionalidad.** Añadiste el servidor Playwright MCP y dejaste que Copilot controlara un navegador para verificar el filtrado antes de abrir una PR.
- **Dirigiste al modo de agente para hacer cambios coordinados en todas las capas.** En una sesión, añadiste una funcionalidad de filtrado que afectó al cliente, al servidor y a las pruebas.
- **Usaste un agente personalizado.** Seleccionaste el agente de accesibilidad y observaste cómo implementaba el modo de alto contraste en el repositorio.
- **Gestionaste y orientaste una sesión de agente.** Revisaste los cambios propuestos en el editor, aceptaste los que querías y ampliaste la sesión con una solicitud posterior para añadir el modo claro.
- **Cerraste el proceso con una solicitud de incorporación de cambios.** Publicaste el trabajo local y lo revisaste de principio a fin, como haría tu equipo.

## Revisión y siguientes pasos

Aquí termina el itinerario obligatorio de VS Code. Puedes dar por completado el taller.

Para ampliar tu perspectiva sobre las capacidades de agentes de Copilot, los otros entornos cubren escenarios relacionados desde distintas interfaces:

- **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades, un agente de QA personalizado, Playwright MCP, Agent Merge y traspasos con contexto.
- **[Entorno del agente en la nube](../../cloud/)**: asigna incidencias al agente en la nube, supervisa sesiones desde la página de agentes e itera de forma asíncrona sobre solicitudes de incorporación de cambios.

También puedes seguir ampliando lo que empezaste aquí. [awesome-copilot][awesome-copilot] ofrece archivos de instrucciones, agentes personalizados y habilidades que puedes adaptar a tus proyectos.

Como ampliación opcional, [Incorporar Foundry][exercise-7] utiliza VS Code y Microsoft Foundry Toolkit para preparar un modelo, desplegar Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
