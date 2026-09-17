# Crear comandos y skills para el workflow de features

## Objetivos

- Definir los comandos y skills de OpenCode necesarios para establecer un workflow reproducible para el desarrollo de features a partir de GitHub Issues.
- El objetivo es que el agente pueda acompañar el ciclo de vida de una feature mediante instrucciones reutilizables y comandos explícitos, evitando concentrar toda la lógica del workflow en `AGENTS.md`.

## Notas

Este feature corresponde al issue #9: Crear comandos y skills para el workflow de features

Workflow previsto:
GitHub Issue → /start-feature <issue> → [prepara current-feature.md, marca issue como in-progress, se detiene] 
→ Implementación + verificación → /create-pr <issue> → [commit + push, crea PR] 
→ Revisión → [nuevos cambios, commits, push] 
→ /finish-feature <issue> → [actualiza y limpia current-feature.md, commit + push final, quita in-progress, añade done] 
→ PR listo para merge → Merge → GitHub cierra el issue

Skills a crear:
- feature-workflow: define workflow general, fases, contexto por fase, relación feature-issue-current-feature.md
- pull-request: define reglas para preparar y publicar PR (identificar cambios, evitar ajenos, revisar estado/diff/historial, usar rutas explícitas, commit/push, crear PR con Closes #<issue>, confirmación explícita)

Commands a crear:
- /start-feature: inicia feature desde issue, carga skill feature-workflow, actualiza current-feature.md, preserva historial, registra issue en Notas, marca issue in-progress, deteniéndose después de Inicio
- /create-pr: prepara y crea PR, carga skills, verifica rama/estado, identifica cambios tarea, ejecuta verificaciones, muestra cambios, solicita confirmación, crea commit si es necesario, push, crea PR contra main con Closes #<issue>, informa URL, deteniéndose
- /finish-feature: ejecuta fase Finalización, obtiene número issue, verifica corresponda a feature actual, verifica implementation verificada y cumple aceptación, verifica no trabajar directamente en main, revisa estado/diff/historial, actualiza current-feature.md, añade entrada al inicio de Histórico con resumen trabajo, cambia encabezado a # Feature actual, vacía Objetivos y Notas, conserva encabezados e histórico anterior, solicita confirmación antes de publicar, crea commit final, push, quita in-progress issue, añade done issue

Modificaciones adicionales:
- AGENTS.md: cargar condicionalmente skill feature-workflow cuando tarea implique feature/issue/current-feature.md
- context/current-feature-file-spec.md: especificación del formato del descriptor

Estados GitHub Issues:
- Labels: in-progress (mutuamente excluyente con done)
- Al iniciar: añadir in-progress
- Al finalizar: eliminar in-progress, añadir done
- done: implementación terminada, PR listo para merge
- No cerrar issue manualmente; PR usa Closes #<issue> para que GitHub lo cierre al merge

## Histórico

- Marcado issue #1 como in-progress: Inicializando el proyecto Laravel 13.x
- Aplicada etiqueta "in-progress" a la issue #1 en GitHub
- Feature completada: Proyecto Laravel 13.x inicializado con SQLite, dependencias instaladas, entorno configurado y migraciones ejecutadas
- Issue #1 cerrada en GitHub con etiqueta "done"
- PR creado: "Feature: Issue 1" (PR #2) para mergear la rama feature/issue-1 a main
- PR #2 fusionado exitosamente a main
- Rama feature/issue-1 eliminada local y remotamente después del merge