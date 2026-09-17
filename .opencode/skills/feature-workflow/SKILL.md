# Feature Workflow Skill

Este skill define el workflow general de desarrollo de features y sus fases.

## Fases del Workflow

El desarrollo de una feature se divide en tres fases explícitas:

### 1. **Inicio**
Prepara el contexto necesario para trabajar en la feature a partir de su issue y deja la issue en estado `in-progress`.

### 2. **Implementación y verificación**
Implementa la feature y comprueba que cumple su especificación y criterios de aceptación.

### 3. **Finalización**
Una vez verificada la feature, actualiza el contexto y deja la issue en estado `done`.

## Reglas Fundamentales

- Las fases son explícitas y están dirigidas por el usuario.
- El agente no debe avanzar a una fase posterior sin que el usuario lo solicite explícitamente.
- Después de completar cada fase, el agente debe detenerse y esperar instrucciones del usuario antes de continuar con la siguiente.

## Contexto por Fase

### Inicio
Durante la fase de inicio, el agente debe cargar este skill y:
1. Consultar el GitHub Issue asociado (título, descripción, labels, comentarios, criterios de aceptación)
2. Actualizar `context/current-feature.md` con la información de la feature
3. Preservar el histórico existente en `context/current-feature.md`
4. Registrar el issue asociado en la sección `## Notas`
5. Marcar el issue como `in-progress` en GitHub
6. Detenerse después de completar esta fase

### Implementación y verificación
Durante esta fase, el agente debe:
1. Implementar la feature según lo especificado
2. Verificar que cumple con la especificación y criterios de aceptación
3. No avanzar a la fase de finalización sin solicitud explícita del usuario

### Finalización
Durante la fase de finalización, el agente debe cargar este skill y:
1. Actualizar `context/current-feature.md` (añadir entrada al histórico, limpiar objetivos y notas)
2. Crear el commit de finalización y hacer push
3. Quitar la etiqueta `in-progress` y añadir `done` al issue en GitHub
4. Detenerse después de completar esta fase

## Relación entre Elementos

- Una **feature** corresponde a un **GitHub Issue**
- El descriptor de la feature activa se almacena en `context/current-feature.md`
- Este skill debe trabajar junto con el skill `pull-request` para el manejo de Pull Requests
- El flujo completo es: GitHub Issue → `/start-feature` → Implementación → `/create-pr` → Revisión → `/finish-feature` → PR listo para merge → Merge → GitHub cierra el issue