# Start Feature Command

Este comando inicia una feature a partir de un GitHub Issue.

## Funcionalidad

El comando `/start-feature` debe:

1. **Recibir el número del issue** como argumento
2. **Cargar el skill `feature-workflow`** para acceder a las reglas del workflow
3. **Consultar el issue** en GitHub, incluyendo:
   - Título
   - Descripción
   - Labels
   - Comentarios
   - Criterios de aceptación
4. **Actualizar `context/current-feature.md`** con la información de la feature:
   - Establecer el nombre de la feature (título del issue)
   - Definir los objetivos basada en la descripción y criterios de aceptación
   - Añadir notas contextuales incluyendo la referencia al issue
   - **Preservar el histórico existente** en el archivo
5. **Registrar el issue asociado** en la sección `## Notas` de `context/current-feature.md`
6. **Marcar el issue como `in-progress`** en GitHub:
   - Añadiendo la etiqueta `in-progress`
   - Añadiendo un comentario indicando que el trabajo ha comenzado
7. **Detenerse después de completar la fase de Inicio**
   - No implementar la feature
   - No crear commits
   - No crear un Pull Request
   - Esperar instrucciones explícitas del usuario para continuar

## Uso

```
/start-feature <issue-number>
```

Ejemplo:
```
/start-feature 3
```

## Comportamiento Detallado

Al ejecutarse, el comando realizará los siguientes pasos:

### Paso 1: Validación de Argumentos
- Verificar que se proporcione un número de issue válido
- Confirmar que el issue existe en el repositorio

### Paso 2: Carga de Skills
- Cargar el skill `feature-workflow` para acceder a las reglas del workflow

### Paso 3: Consulta del Issue
- Obtener el issue especificado desde GitHub API
- Extraer: título, cuerpo/descripción, labels, comentarios, estado

### Paso 4: Actualización del Descriptor de Feature
- Establecer el encabezado principal con el título de la feature
- Llenar la sección `## Objetivos` basada en la descripción y criterios de aceptación del issue
- Añadir/actualizar la sección `## Notas` con:
  - Referencia al issue: `- Issue: #<number>`
  - Información contextual relevante del workflow
- **Preservar completamente** la sección `## Histórico` existente
- Mantener el formato especificado en `context/current-feature-file-spec.md`

### Paso 5: Actualización del Issue en GitHub
- Añadir la etiqueta `in-progress` al issue (creándola si no existe)
- Añadir un comentario indicando: "Issue marcada como in-progress. Se ha comenzado a trabajar en la feature."
- No cambiar el estado del issue (mantenerlo como OPEN)

### Paso 6: Notificación y Detención
- Informar al usuario que la feature ha sido iniciada correctamente
- Mostrar un resumen de lo hecho:
  - Issue #<number> marcada como in-progress
  - Descriptor de feature actualizado en `context/current-feature.md`
- Detenerse explícitamente, esperando la siguiente instrucción del usuario