# Guía práctica: trabajo en equipo con Git y GitHub

**Ejercicio:** agregar frases al README de un repositorio compartido
**Repositorio:** ejercicio-equipo-readme (cuenta ModoBeta)
**Equipo:** Gonzalo, Guido, Abigail y Facundo

Esta guía explica, paso a paso, cómo se organiza un flujo de trabajo colaborativo real en GitHub, aunque el ejercicio en sí sea simple. La idea es que todo el equipo incorpore la mecánica (ramas, commits, pull requests, revisiones) que después va a usar en proyectos serios.

---

## 1. Configuración inicial del repositorio

Antes de empezar, alguien del equipo (quien tenga la cuenta de administrador) hace lo siguiente:

1. Crear el repositorio en GitHub con un nombre descriptivo, por ejemplo `ejercicio-equipo-readme`.
2. Elegir si va a ser público o privado. Para un ejercicio interno, lo más prolijo es privado.
3. Marcar la opción **Add a README file** al crearlo, para que exista un `README.md` inicial.
4. Ir a **Settings → Collaborators and teams → Add people** y agregar a cada integrante del equipo con rol **Write** (lectura + escritura). Esto le permite a cada uno subir cambios y crear Pull Requests, pero no modificar la configuración del repositorio.
5. Cada persona agregada recibe una invitación (por email o notificación en GitHub) que debe **aceptar** para tener acceso.

### 1.1 Habilitar que todos puedan crear y aprobar Pull Requests

Por default, cualquier colaborador con permiso **Write** ya puede crear Pull Requests y aprobar los de sus compañeros (GitHub no permite aprobar el propio PR, así que siempre hace falta que sea otra persona la que revise). Para que esta dinámica quede formalizada y sea obligatoria —y así todos practiquen la revisión antes del merge— conviene activar una regla de protección sobre la rama `main`:

1. Ir a **Settings → Branches → Add branch ruleset** (o **Add rule**, según la versión de la interfaz).
2. En "Branch name pattern" poner `main`.
3. Activar **Require a pull request before merging**.
4. Dentro de esa opción, activar **Require approvals** y poner **1** como cantidad mínima de aprobaciones.
5. Dejar desactivado "Require review from Code Owners" (no aplica para un equipo tan chico).
6. Guardar la regla.

Con esto, nadie —ni siquiera quien administra el repositorio— puede mergear directo a `main` sin que otra persona del equipo haya aprobado el Pull Request. Es la forma más simple de que los cuatro roten entre crear PRs y aprobar PRs de sus compañeros.

---

## 2. Primeros pasos de cada integrante del equipo

### 2.1 Aceptar la invitación
Revisar el email o las notificaciones de GitHub y aceptar la invitación al repositorio.

### 2.2 Clonar el repositorio en la máquina local
```bash
git clone https://github.com/ModoBeta/ejercicio-equipo-readme.git
cd ejercicio-equipo-readme
```

### 2.3 Configurar la identidad en Git (si no se hizo antes)
```bash
git config --global user.name "Nombre Apellido"
git config --global user.email "email@ejemplo.com"
```
Así cada commit queda firmado con el nombre real, y en el historial se ve quién hizo qué.

---

## 3. Ramas: por qué se usan y cómo se nombran

Trabajar todos directo sobre `main` funciona en un ejercicio de cinco minutos, pero no es la práctica real, y justamente el objetivo acá es aprender el flujo correcto: **nunca se trabaja directo sobre `main`, siempre se crea una rama nueva por tarea.**

### 3.1 Convención estándar de nombres de rama

La convención más usada en la industria combina un prefijo por tipo de trabajo con una descripción corta:

```
<tipo>/<id-ticket>-<descripcion-corta>
```

**Prefijos más comunes (`<tipo>`):**

| Prefijo | Uso |
|---|---|
| `feat/` o `feature/` | Nuevas funcionalidades (ej. `feat/user-profile`) |
| `fix/` o `bugfix/` | Corrección de errores en entornos de desarrollo o pruebas |
| `hotfix/` | Correcciones urgentes para aplicar directamente sobre producción |
| `refactor/` | Cambios en la estructura del código sin alterar su comportamiento ni agregar funciones |
| `docs/` | Modificaciones en la documentación (README, comentarios, wikis) |
| `chore/` | Tareas de mantenimiento, actualización de dependencias o ajustes de build/CI-CD |
| `test/` | Añadir o corregir pruebas unitarias e integrales |
| `release/` | Preparación de código para un nuevo despliegue o versión (ej. `release/v1.2.0`) |

**Reglas de formato:**

1. Usar siempre **kebab-case**: solo letras minúsculas separadas por guiones (`-`). Evitar espacios, guiones bajos (`_`) o mayúsculas.
2. Incluir el ID del ticket o issue cuando exista: si se usa Jira, Trello o GitHub Issues, antepone la ID a la descripción (ej. `feat/PRO-104-stripe-integration` o `fix/#42-auth-redirect`).
3. Ser descriptivo pero conciso: limitar la descripción a 2 o 4 palabras representativas.
4. Usar solo caracteres ASCII: evitar acentos, la letra `ñ` o símbolos especiales.

Para este ejercicio, como no hay tickets ni tipos de trabajo distintos, alcanza con una versión simplificada usando `docs/` (porque se está tocando documentación) más el nombre de cada uno:

| Persona | Nombre de rama sugerido |
|---|---|
| Gonzalo | `docs/gonzalo-agrega-frase` |
| Guido | `docs/guido-agrega-frase` |
| Abigail | `docs/abigail-agrega-frase` |
| Facundo | `docs/facundo-agrega-frase` |

### 3.2 Crear la rama y moverse a ella
Parado sobre `main` actualizado, cada uno ejecuta:

```bash
git checkout -b docs/gonzalo-agrega-frase
```
(reemplazando por el nombre de rama propio). Esto crea la rama nueva y posiciona ahí automáticamente.

---

## 4. Hacer el cambio: agregar una frase al README

1. Abrir el archivo `README.md` con cualquier editor (VS Code, por ejemplo).
2. Agregar una línea nueva con una frase (puede ser una frase random, una cita, lo que decida el grupo), al final del archivo.
3. Guardar el archivo.

### 4.1 Ver qué cambió
```bash
git status
git diff
```

### 4.2 Agregar el cambio al staging y confirmarlo
```bash
git add README.md
git commit -m "Agrega frase de Gonzalo al README"
```
El mensaje de commit debe ser corto y describir qué se hizo, en modo imperativo (Agrega, Corrige, Actualiza).

### 4.3 Subir la rama a GitHub
```bash
git push origin docs/gonzalo-agrega-frase
```
La primera vez que se sube una rama nueva, GitHub devuelve un link sugerido para crear el Pull Request directamente.

---

## 5. Pull Request: cómo se integra el cambio a `main`

1. En GitHub, entrar a la pestaña **Pull requests → New pull request**.
2. Elegir como base `main` y como comparación la rama recién subida.
3. Poner un título claro y, si hace falta, una descripción breve de qué se agregó.
4. Crear el Pull Request.
5. **Otro integrante del equipo** (nunca uno mismo) entra al PR, revisa el cambio y lo aprueba en **Files changed → Review changes → Approve**. Con la regla de protección activada en el paso 1.1, el botón de merge va a estar bloqueado hasta que exista al menos una aprobación.
6. Una vez aprobado, se hace clic en **Merge pull request**.
7. Borrar la rama ya mergeada (GitHub ofrece un botón **Delete branch** apenas se mergea, para mantener el repositorio limpio).

Conviene que el equipo rote quién aprueba cada PR, para que los cuatro pasen por la experiencia de revisar el trabajo de un compañero.

---

## 6. Mantenerse actualizado y resolver conflictos

Como los cuatro van a tocar el mismo archivo (`README.md`), es probable que aparezcan **conflictos de merge** si dos personas editan la misma línea. Para minimizarlo:

- Cada uno debe agregar su frase en una línea nueva, al final del archivo, sin reescribir líneas existentes.
- Antes de empezar a trabajar, conviene actualizar `main` local:
  ```bash
  git checkout main
  git pull origin main
  git checkout -b docs/nombre-agrega-frase
  ```

### 6.1 Si aparece un conflicto
1. GitHub (o Git en local) marca el archivo con conflicto.
2. Abrir el archivo y buscar las marcas `<<<<<<<`, `=======`, `>>>>>>>`.
3. Decidir qué líneas quedan (generalmente, ambas frases, una debajo de la otra).
4. Borrar las marcas de conflicto.
5. Guardar, `git add README.md`, `git commit`, y `git push` de nuevo.

Entender que los conflictos son normales y tienen solución es uno de los aprendizajes más valiosos del ejercicio.

---

## 7. Otros comandos útiles para conocer

Además del flujo básico (`add`, `commit`, `push`, `checkout -b`), vale la pena que todo el equipo se familiarice con estos comandos:

| Comando | Para qué sirve |
|---|---|
| `git status` | Muestra el estado actual: qué archivos cambiaron, cuáles están en staging, en qué rama se está parado. |
| `git log` | Muestra el historial de commits. Con `git log --oneline --graph --all` se ve un resumen visual de todas las ramas. |
| `git diff` | Muestra línea por línea qué cambió en los archivos, antes de hacer `add`. |
| `git branch` | Lista las ramas locales. Con `git branch -a` se ven también las remotas. |
| `git merge <rama>` | Mezcla los cambios de otra rama en la rama actual (esto es lo que hace GitHub internamente al mergear un PR; sirve saber hacerlo también desde la terminal). |
| `git fetch` | Trae información de las ramas remotas sin mezclarla todavía en el trabajo local (a diferencia de `git pull`, que trae y mezcla en un solo paso). |
| `git pull` | Trae los cambios del remoto y los mezcla en la rama local actual. |
| `git stash` | Guarda temporalmente cambios sin confirmar, para poder cambiar de rama sin perderlos ni tener que commitear a medias. |
| `git checkout <rama>` | Cambia a otra rama existente. |

---

## 8. Resumen del flujo (para tener a mano)

1. `git pull origin main` (actualizarse)
2. `git checkout -b docs/tu-nombre-agrega-frase` (crear rama)
3. Editar el README y agregar la frase
4. `git add README.md`
5. `git commit -m "Agrega frase de [tu nombre]"`
6. `git push origin docs/tu-nombre-agrega-frase`
7. Crear el Pull Request en GitHub
8. Otro integrante revisa y aprueba el PR
9. Mergear a `main`
10. Borrar la rama

---

## 9. Buenas prácticas para recordar

- Nunca trabajar directo sobre `main`: siempre rama nueva por tarea, con nombre siguiendo la convención `<tipo>/<descripcion-corta>`.
- Commits chicos y con mensajes claros, no "cambios varios".
- Actualizar `main` local antes de crear una rama nueva, para partir de la última versión.
- Un Pull Request por cambio, no acumular varias cosas distintas en uno solo.
- Aprobar el PR de un compañero (nunca el propio) antes de mergear: con la regla de protección activada, esto queda garantizado técnicamente, no solo por buena voluntad.
