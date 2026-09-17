# Finish Feature Command

Este comando ejecuta la fase de Finalización de una feature en la rama actual.

## Funcionalidad

El comando `/finish-feature` debe:

1. **Obtener el número del issue** desde `$ARGUMENTS` o desde `context/current-feature.md`
2. **Comprobar que el issue corresponde a la feature actual**
3. **Comprobar que la implementación está verificada y cumple los criterios de aceptación**
4. **Comprobar que no se está trabajando directamente sobre `main`**
5. **Revisar el estado, diff e historial de Git**
6. **Actualizar `context/current-feature.md`**:
   - Añadir al principio de `## Histórico` una entrada de una línea que resuma el trabajo realizado
   - Cambiar el encabezado principal a `# Feature actual`
   - Vaciar `## Objetivos`
   - Vaciar `## Notas`
   - Conservar los encabezados y el histórico anterior
7. **Solicitar confirmación explícita** antes de publicar los cambios
8. **Crear el commit de finalización**
9. **Hacer `push` de la rama actual**
10. **Quitar `in-progress` del issue**
11. **Añadir `done` al issue**
12. **Detenerse** (no fusionar el Pull Request)

## Requisitos Previos

Antes de ejecutar este comando, se asume que:
- La feature ha sido implementada según lo especificado
- La implementación ha sido verificada contra los criterios de aceptación
- El usuario ha confirmado explícitamente que está listo para finalizar la feature

## Uso

```
/finish-feature <issue-number>
```

O si el número del issue está disponible en `context/current-feature.md`:
```
/finish-feature
```

Ejemplo:
```
/finish-feature 3
```

## Comportamiento Detallado

### Paso 1: Obtención del Número de Issue
- Si se proporciona como argumento: usar ese valor
- Si no se proporciona: intentar obtenerlo de `context/current-feature.md` buscando el patrón `- Issue: #<number>` en la sección `## Notas`
- Si no se puede determinar, fallar con un mensaje explicativo

### Paso 2: Verificaciones Preliminares
- **Correspondencia de issue**: Confirmar que el número obtenido corresponde a la feature descrita en `context/current-feature.md`
- **Estado de implementación**: Asumir que el usuario ya ha verificado que la implementation cumple con los criterios de aceptación (este comando no verifica técnicamente la implementation)
- **Rama actual**: Verificar que `git rev-parse --abbrev-ref HEAD` no devuelva `main`
- **Estado de Git**:
  - Revisar `git status` para entender qué cambios existen
  - Examinar `git diff` para ver cambios no staggeados
  - Revisar `git diff --staged` para ver cambios staggeados
  - Ver historial reciente con `git log --oneline -5`

### Paso 3: Preparación para Actualización del Descriptor
- Crear una entrada resumen para el histórico basada en el trabajo realizado
- Preparar los cambios para `context/current-feature.md`:
  - Nuevo encabezado principal: `# Feature actual`
  - Sección `## Objetivos`: vacía (pero manteniendo el encabezado)
  - Sección `## Notas`: vacía (pero manteniendo el encabezado)
  - Sección `## Histórico`: 
    - Nueva entrada al inicio: `[fecha] - Resumen del trabajo realizado en la feature #<number>`
    - Seguido por todo el histórico existente

### Paso 4: Solicitud de Confirmación Explícita
Mostrar al usuario:
- El número de issue que se va a finalizar
- Un resumen de los cambios que se van a committear (basado en `git status`)
- Los cambios exactos que se van a hacer en `context/current-feature.md`
- Preguntar explícitamente: "¿Desea proceder con la finalización de la feature #<number>?"

Esperar respuesta afirmativa clara antes de continuar.

### Paso 5: Ejecución de Cambios Locales
Si el usuario confirma:
- Aplicar los cambios a `context/current-feature.md` según lo preparado
- Stagear el archivo modificado: `git add context/current-feature.md`
- Si existen otros cambios relacionados con la feature:
  - Identificarlos cuidadosamente (evitando cambios ajenos)
  - Stagearlos explícitamente: `git add <rutas específicas>`
- Crear el commit de finalización:
  ```
  git commit -m "Finalizar feature #<number>: [resumen breve del trabajo realizado]"
  ```

### Paso 6: Push de Rama
- Ejecutar: `git push origin <nombre-de-rama-actual>`
- Esto actualizará la rama en el remoto

### Paso 7: Actualización del Issue en GitHub
- Obtener el nombre exacto de la etiqueta `in-progress` y `done` (pueden variar ligeramente)
- Quitar la etiqueta `in-progress` del issue:
  ```
  gh issue edit <issue-number> --remove-label "in-progress"
  ```
- Añadir la etiqueta `done` al issue:
  ```
  gh issue edit <issue-number> --add-label "done"
  ```
- Opcional: añadir un comentario de cierre:
  ```
  gh issue comment <issue-number> --body "Feature completada. La implementación ha sido verificada y cumple los criterios de aceptación."
  ```

### Paso 8: Resultado y Detención
Informar al usuario:
- Que la feature ha sido finalizada correctamente
- Que se ha creado el commit de finalización y se ha hecho push
- Que el issue ha sido actualizado (quita in-progress, añade done)
- Que si existía un Pull Request para la rama, este ha sido actualizado con el nuevo commit
- Recordatorio: el Pull Request no ha sido fusionado y el issue no ha sido cerrado (el cierre ocurrirá automáticamente al hacer merge mediante `Closes #<issue-number>`)
- Detenerse explícitamente, esperando la siguiente instrucción del usuario

## Caso Especial: Pull Request Existente

Este comando debe funcionar correctamente aunque ya exista un Pull Request para la rama:

- El `push` final (paso 6) actualizará automáticamente el Pull Request existente
- No se creará un nuevo Pull Request
- El Pull Request existente contendrá ahora el commit de finalización
- El cuerpo del Pull Request debería seguir conteniendo `Closes #<issue-number>` para que el issue se cierre al hacer merge

## Seguridad y Prevención de Errores

El comando incorpora varias verificaciones para prevenir errores comunes:

1. **Prevención de trabajo en main**: Bloquea explícitamente la ejecución si se está en la rama `main`
2. **Confirmación explícita**: Nunca procede sin autorización clara del usuario
3. **Identificación cuidadosa de changes**: Trata de incluir solo cambios relacionados con la feature
4. **Preservación del historial**: Nunca elimina el histórico de `context/current-feature.md`, solo lo actualiza
5. **Validación de issue**: Verifica que el issue especificado corresponda realmente a la feature actual