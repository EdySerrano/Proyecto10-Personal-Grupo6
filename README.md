
# Proyecto 10: Pull Requests y Revisión de Código Automatizada con Hooks y Linters

 # Datos del Alumno:
* **Nombre:** Serrano Arostegui, Edy Saul
* **Proyecto:**  Pull requests y revision de codigo Automatizada con Hooks y Linters **(Proyecto 10)**
* **Grupo:** #6
* **Repositorio del proyecto:** https://github.com/OliverHz28/PC3Proyecto10.git

## 1. Propósito del proyecto
El objetivo es construir un **repositorio local** que emule un flujo de _Pull Request_ (PR) de principio a fin y una **revision de codigo totalmente automatizada** mediante _Git hooks_, _linters_, pruebas y flujos de **GitHub Actions** (ejecutados en local con `act`).  
La meta es demostrar como asegurar la calidad y la seguridad del codigo **antes** de fusionarlo a la rama principal, minimizando el error humano y reforzando las buenas practicas de en la cultura _DevOps_.

---

## 2. Sprints

### 2.1 Sprint 1 – Fundamentos 
**Objetivos especificos**
1. Crear la estructura inicial de carpetas (`src/`, `scripts/`, `tests/`, etc.).
2. Implementar dos _Git hooks_ locales:
   * **`commit-msg`** → obliga a seguir el formato `^[A-Z]{3,5}-\d+: .+`.
   * **`pre-push`** → ejecuta `scripts/lint_all.sh`; aborta el `push` si hay errores.
3. Desarrollar `lint_all.sh` que orquesta:
   * `flake8 src/` con reglas estrictas.
   * `shellcheck scripts/*.sh` para Bash.
   * `tflint` sobre `iac/` (si existe).
4. Programar `src/config_modifier.py` y 2 tests unitarios en `tests/`.
5. Sembrar un _workflow_ vacio `pr_validation.yaml` en `.github/workflows/`.
6. Generar 4 commits iniciales (`PROY‑001` → `PROY‑004`).

**Herramientas clave**  
Git, hooks, Bash, flake8, shellcheck, tflint, pytest, act (solo instalacion).

---

### 2.2 Sprint 2 – Validaciones CI/CD y Simulación de Pull Requests
**Objetivos específicos**
1. Completar `scripts/check_pr.py` para validar titulo, changelog, commits, lint y tests; generar `pr_report.md`.
2. Extender `pr_validation.yaml`:
   * Job `validate-pr` con `matrix` de IDs de PR.
   * Pasos: `lint_all.sh` → `pytest --cov` (≥ 80 %) → `check_pr.py`.
3. Crear ramas `feature/AUTO_INCR_VERSION` y `feature/ADD_LOGGING`.
4. Simular al menos 2 PRs en `pr_simulation/` y ejecutarlos localmente con `act pull_request`.
5. Ajustar codigo/fallos hasta obtener reporte “OK” en todas las secciones.

**Herramientas clave**  
Python, pytest-cov, flake8, shellcheck, tflint, act (con matrix strategy implementada).

---

### 2.3 Sprint 3 – Seguridad y Documentacion Final
**Objetivos especificos**
1. **Seguridad:** Añadir job `security-scan` que ejecute `bandit -r src/` y `shellcheck`; fallar en vulnerabilidades criticas.
2. **Lint avanzado:** Actualizar `.flake8` (e.g. `max-line-length = 100`, reglas de equipo).
3. **Deteccion de duplicados:** Integrar logica en `check_pr.py` para señalar bloques repetidos y sugerir mejoras.
4. **Documentacion y Videos:** Completar `README.md`, ejemplos PR válidos/invalidos, guías de `act`, y grabar video final.
5. **Cierre de Kanban:** Mover issues a _Done_ y simular _merge_ final.

**Herramientas clave**  
bandit, shellcheck, Python, flake8 personalizado, act, Markdown (README.md) y video.

---

## 3. Estructura del repositorio (estado final)

```
.
├── src/
│ ├── config_modifier.py
│ ├── logger.py
│ └── init.py
├── scripts/
│ ├── lint_all.sh
│ └── check_pr.py
├── tests/
│ ├── test_config_modifier.py
│ └── test_logger.py
├── pr_simulation/
│ ├── 201/
│ │ ├── pr_201_title.txt
│ │ ├── pr_201_body.md
│ │ ├── commits.txt
│ │ ├── pr_report.md
│ │ └── logs/
│ │ ├── lint.log
│ │ └── tests.log
│ └── ...
├── .github/
│ └── workflows/
│ └── pr_validation.yaml
├── .flake8
├── .gitignore
├── CHANGELOG.md
└── README.md

```

---

## 4. Principales herramientas y su papel
| Herramienta | Rol en el proyecto |
|------------|---------------------|
| **Git hooks** (`commit-msg`, `pre-push`) | Prevencion temprana de errores y estandarizacion del flujo de commits/pushes. |
| **flake8** | Estilo y calidad de codigo Python. |
| **shellcheck** | Buenas practicas y seguridad en scripts Bash. |
| **bandit** | Analisis de vulnerabilidades en Python. |
| **pytest / pytest-cov** | Pruebas unitarias y medicion de cobertura (≥ 80 %). |
| **act** | Ejecucion local de workflows GitHub Actions. |
| **Markdown** | Documentacion, PR bodies, changelog y reportes. |

---

## 5. Flujo de trabajo resumido
1. Desarrollador crea una rama *feature/* y realiza commits:
   * Hook `commit-msg` valida formato.
2. Al hacer `git push`:
   * Hook `pre-push` ejecuta `lint_all.sh`; bloquea si falla.
3. Se crea una carpeta `pr_simulation/<id>/` y se registra el set de commits/titulo/body.
4. Pipeline `act pull_request` dispara `pr_validation.yaml`:
   1. Lint (`lint_all.sh`).
   2. Tests (`pytest --cov`).
   3. Seguridad (`bandit`, `shellcheck`) – Sprint 3.
   4. Validacion personalizada (`check_pr.py`).
5. Si todas las fases marcan **OK**, se aprueba fusionar localmente; de lo contrario, se corrigen fallos.

---

## 6. Sobre los Issues
* Crear issues por cada tarea y poblarlas en el tablero Kanban.
* Configurar entorno Python v3.9, instalar dependencias (`pip install -r requirements.txt`).
* Iniciar Sprint 1 siguiendo la guia y grabar el video de demostracion.


**Recordatorio del proyecto:** 

> _“La automatización no reemplaza la revision humana, pero garantiza que el ojo humano llegue a ver solo codigo de alta calidad.”_

## Link de los sprints individuales:
Se van a colocar en cada rama:
* Sprint-1:
* Sprint-2:
* Sprint-3:

