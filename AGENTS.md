# Instrucciones para el agente

## Contexto

Leer los siguientes ficheros para obtener contexto completo del proyecto:

- @context/current-feature.md
- @context/current-feature-file-spec.md
- @context/feature-workflow.md
- @context/coding-conventions.md

## Características Especiales del Workflow de Features

Cuando la tarea implique iniciar, implementar, verificar o finalizar una feature, trabajar con un GitHub Issue, o modificar `context/current-feature.md`, el agente debe cargar automáticamente el skill `feature-workflow` para acceder al workflow estructurado y los comandos especializados.

## GitHub

Al crear o modificar issues o pull requests:

- Utilizar Markdown válido.
- Los saltos de línea deben ser saltos reales, no la cadena literal `\n`.
- Para cuerpos con varias líneas, utilizar `--body-file` o un mecanismo equivalente que preserve correctamente el formato.