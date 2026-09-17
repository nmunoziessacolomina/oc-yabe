# Create PR Command

Este comando prepara y crea el Pull Request de una feature.

## Funcionalidad

El comando `/create-pr` debe:

1. **Recibir el número del issue** como argumento
2. **Cargar los skills necesarios** (especialmente `pull-request`)
3. **Comprobar la rama actual y el estado del repositorio**:
   - Verificar que no se esté trabajando directamente sobre `main`
   - Confirmar que se esté en una rama feature adecuada
   - Revisar `git status` para entender el estado actual
4. **Identificar los cambios pertenecientes a la tarea**:
   - Diferenciar entre cambios de la feature actual y cambios ajenos
   - Enfocarse únicamente en archivos relacionados con la feature
5. **Ejecutar las verificaciones necesarias**:
   - Revisar el diff propuesto
   - Verificar que los cambios sean apropiados para la feature
   - Confirmar que no se incluyan accidentalmente trabajos no relacionados
6. **Mostrar los cambios que se van a publicar**:
   - Presentar un resumen claro de los cambios identificados
   - Mostrar el diff si es apropiado y no demasiado extenso
7. **Solicitar confirmación explícita**:
   - Preguntar al usuario si desea proceder con los cambios mostrados
   - Esperar respuesta afirmativa antes de continuar
8. **Crear el commit cuando existan cambios sin publicar**:
   - Si hay cambios staggeados o modificados sin commit:
     - Crear un commit descriptivo relacionado con la feature
     - Usar convenciones de commit apropiadas
9. **Hacer `push` de la rama**:
   - Publicar los commits en el remoto
   - Actualizar la rama feature en el servidor
10. **Crear el Pull Request contra `main`**:
    - Usar GitHub CLI para crear el PR
    - Establecer `main` como rama base
    - Usar la rama feature actual como rama head
    - Incluir un título apropiado (usualmente el título de la feature/issue)
    - Incluir en el cuerpo: `Closes #<issue-number>`
    - Añadir descripción adicional si es necesario
11. **Informar de la URL del Pull Request y detenerse**:
    - Proveer el enlace directo al PR creado
    - Indicar que el PR está listo para revisión
    - Detenerse esperando instrucciones adicionales

## Limitaciones Importantes

- **No debe fusionar el Pull Request**: El merge es responsabilidad del usuario o procesos establecidos
- **No debe cerrar manualmente el issue**: El cierre ocurre automáticamente vía `Closes #<issue-number>` al hacer merge
- **Si ya existe un Pull Request para la rama, no debe crear otro**:
  - Detectar si ya hay un PR asociado a la rama feature
  - En su lugar, actualizar el PR existente con los nuevos cambios
  - Esto permite que el PR reciba nuevos commits después de su creación inicial

## Uso

```
/create-pr <issue-number>
```

Ejemplo:
```
/create-pr 3
```

## Comportamiento Detallado

### Paso 1: Validación y Preparación
- Validar que se proporcione un número de issue
- Cargar el skill `pull-request`
- Verificar que el issue corresponde a la feature actual (comparar con `context/current-feature.md`)

### Paso 2: Análisis del Repositorio
- Confirmar que no se esté en la rama `main`
- Obtener el nombre de la rama actual
- Ejecutar `git status --porcelain` para ver cambios
- Identificar archivos modificados, añadidos, eliminados

### Paso 3: Filtrado de Changes
- Excluir cambios obviamente ajenos (por ejemplo, en directorios no relacionados)
- Enfocarse en cambios que claramente pertenecen a la feature
- Si hay dudas, consultar al usuario antes de proceder

### Paso 4: Presentación de Cambios
- Mostrar resumen de archivos que se van a afectar
- Si el diff es manejable, mostrarlo completo
- Si es muy extenso, ofrecer mostrarlo por partes o resumirlo

### Paso 5: Confirmación Explícita
- Esperar respuesta clara del usuario (sí/no, y/n, confirmar/cancelar)
- No proceder sin autorización explícita

### Paso 6: Procesamiento de Cambios Sin Commit
Si hay cambios modificados o eliminados sin staggear:
```
git add <rutas explícitas de archivos relacionados>
git commit -m "Descripción clara del trabajo realizado en la feature"
```
Si hay cambios staggeados pero sin commit:
```
git commit -m "Descripción clara del trabajo realizado en la feature"
```

### Paso 7: Push de Rama
```
git push origin <nombre-de-rama-actual>
```

### Paso 8: Creación/Actualización de Pull Request
Primero, verificar si ya existe un PR para esta rama:
```
gh pr list --head <nombre-de-rama-actual> --state open
```

Si existe un PR abierto:
- Actualizarlo con los nuevos cambios (el push ya actualizó el PR)
- Confirmar que el cuerpo contiene `Closes #<issue-number>`
- Si no lo contiene, actualizar la descripción para incluirlo

Si no existe un PR abierto:
- Crear nuevo PR:
  ```
  gh pr create \
    --title "<Título de la feature/issue>" \
    --body "Descripción de la feature\\n\\nCloses #<issue-number>" \
    --base main \
    --head <nombre-de-rama-actual>
  ```

### Paso 9: Resultado y Detención
- Informar la URL del Pull Request
- Indicar si se creó nuevo PR o se actualizó uno existente
- Recordar que el PR está listo para revisión y que incluirá automáticamente el cierre del issue mediante `Closes #<issue-number>`
- Detenerse esperando instrucciones adicionales