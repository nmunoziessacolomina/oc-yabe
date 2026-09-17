# Pull Request Skill

Este skill define las reglas y comprobaciones necesarias para preparar y publicar un Pull Request.

## Requisitos Mínimos

Al crear o actualizar un Pull Request, el agente debe:

### 1. Identificar los archivos modificados por la tarea
- Utilizar `git status --porcelain` o similar para identificar cambios
- Enfocarse únicamente en los archivos relacionados con la feature actual
- Evitar incluir cambios ajenos a la tarea

### 2. Evitar incluir cambios ajenos
- Revisar cuidadosamente el diff antes de hacer `git add`
- Utilizar rutas explícitas al hacer `git add` cuando sea posible
- Verificar que los cambios estén relacionados con la feature en curso

### 3. Revisar el estado, diff e historial de Git
- Comprobar `git status` para ver el estado actual
- Revisar `git diff` para ver los cambios propuestos
- Verificar el historial reciente con `git log --oneline -10`

### 4. Verificar los cambios antes de publicarlos
- Mostrar al usuario los cambios que se van a publicar
- Solicitar confirmación explícita antes de proceder
- Permitir al usuario revisar y approbar los cambios

### 5. Utilizar rutas explícitas al hacer `git add`
- Cuando sea posible, especificar archivos individuales en lugar de usar `git add .`
- Esto reduce el riesgo de incluir accidentalmente cambios no deseados

### 6. Hacer `commit` y `push` de la rama
- Crear commits descriptivos que expliquen qué se ha implementado
- Hacer push de la rama feature al remoto

### 7. Crear el Pull Request contra la rama base
- El Pull Request debe ir contra la rama `main` (o rama base especificada)
- Utilizar el comando apropiado de GitHub CLI para crear el PR

### 8. Relacionar el Pull Request con el issue mediante `Closes #<issue-number>`
- Incluir `Closes #<issue-number>` en el cuerpo del Pull Request
- Esto permite que GitHub cierre automáticamente el issue cuando se haga merge

### 9. Solicitar confirmación explícita antes de realizar operaciones externas
- Antes de hacer push, crear PR, o cualquier operación que afecte al remoto
- Esperar aprobación explícita del usuario

### 10. No cerrar ni fusionar manualmente el issue o el Pull Request
- El cierre del issue debe ocurrir automáticamente mediante `Closes #<issue-number>`
- El merge debe ser realizado por el usuario o mediante procesos de revisión establecidos

## Casos Especiales

### Creación inicial del Pull Request
Cuando no existe previamente un PR para la rama:
- Seguir todos los pasos mencionados arriba
- Crear el PR desde cero

### Rama con commits publicados previamente
Cuando ya existe un Pull Request para la rama:
- Identificar los nuevos commits desde la última publicación
- Solo incluir esos nuevos cambios en las operaciones
- Actualizar el PR existente con el push (no crear uno nuevo)
- Asegurarse de que el cuerpo del PR siga conteniendo `Closes #<issue-number>`

## Flujo de Trabajo Típico

1. Usuario solicita crear/actualizar PR para una feature
2. Agente carga este skill y verifica el estado del repositorio
3. Agente identifica los cambios relacionados con la feature
4. Agente muestra los cambios y solicita confirmación explícita
5. Si hay cambios sin publicar:
   - Agente crea commit(s) descriptivo(s)
   - Agente hace push de la rama
6. Agente crea o actualiza el Pull Request:
   - Contra rama `main`
   - Con `Closes #<issue-number>` en el cuerpo
   - Con título y descripción apropiados
7. Agente informa la URL del Pull Request y se detiene
8. Usuario revisa el PR y proporciona feedback
9. Según sea necesario, se repite el proceso para incorporar cambios