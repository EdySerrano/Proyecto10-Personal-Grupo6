# **Sprint 2**
* **Nombre:** Serrano Arostegui, Edy Saul
* **Proyecto:**  Pull requests y revision de codigo Automatizada con Hooks y Linters **(Proyecto 10)**
* **Grupo:** #6
* **Repositorio del proyecto:** https://github.com/OliverHz28/PC3Proyecto10.git


* **Instrucciones:** 

```
git clone https://github.com/OliverHz28/PC3Proyecto10.git
```
## **Descripcion:**
Repositorio donde documento mis contribuciones al `Proyecto 10: Pull requests y revisión de código Automatizada con Hooks y Linters` en sus respectivas ramas.

#### Demostracion en video
[Sprint 2 (Dia 8: 17/06/2025) Grupo 6 Proyecto 10 ](https://www.youtube.com/watch?v=CXj9d7sZ-J0)
#### **Mi Rol**
  * Sprint 2: Me encargue de:
    * La simulacion del pull_request local en el workflow (`pr_validation.yaml`) con `act` ejecutandoce en un contenedor de docker.
    * La mejora del archivo `lint_all.sh` sobre el manejo de errores.
    * Extender el workflow de Actions para Pull Request usando `matrix strategy` en `pr_validation.yaml`.

    ```
        .
        ├───.github/
                └── workflows/
                    └── pr_validation
        ├─── scripts/
                └── lint_all.sh
    ```
## Contribucion (solo codigo):

* Configurar `pr_validation.yaml` para ejecutar el `act` localmente segun lo especificado en el [Issue #21](https://github.com/OliverHz28/PC3Proyecto10/issues/21)

    ```yaml
    name: PR Validation Pipeline

    on:
    pull_request:
        branches: [ main, develop ]

    jobs:
    validate-pr:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout código
            uses: actions/checkout@v3

        - name: Configurar Python
            uses: actions/setup-python@v4
            with:
            python-version: '3.9'

        - name: Instalar dependencias
            run: |
            pip install pytest pytest-cov flake8 bandit
            sudo apt-get update && sudo apt-get install -y shellcheck

        - name: Ejecutar linting
            run: |
            chmod +x ./scripts/lint_all.sh
            ./scripts/lint_all.sh


        - name: Ejecutar pruebas
            run: pytest --cov=src --cov=scripts --cov-fail-under=80
            
    ```

#

* Mejorar el `lint_all.sh` para un mejor manejo de errores segun el [Issue #20](https://github.com/OliverHz28/PC3Proyecto10/issues/20)

    ```python
    #!/usr/bin/env bash

    cd "$(dirname "$0")/.." || exit

    # Colores
    GREEN="\033[1;32m"
    RED="\033[1;31m"
    YELLOW="\033[1;33m"
    NC="\033[0m" #No Color

    errores=0
    start_time=$(date +%s)

    check_tool() {
        if ! command -v "$1" &>/dev/null; then
            echo -e "${YELLOW} Herramienta $1 no esta instalada. Se omitira.${NC}"
            return 1
        fi
        return 0
    }

    echo "Iniciando verificacion de código con linters..."
    echo "==============================================="


    if check_tool flake8; then
        echo "*********************"
        echo "Ejecutando flake8"
        if flake8 src/ tests/ --max-line-length=88 --select=E,W,F; then
            echo -e "${GREEN} No se encontraron errores con flake8${NC}"
        else
            echo -e "${RED} flake8 encontró errores${NC}"
            errores=1
        fi
    fi

    if check_tool shellcheck; then
        echo "*********************"
        echo "Ejecutando shellcheck"
        if shellcheck scripts/*.sh hooks/*; then 
            echo -e "${GREEN} No se encontraron errores con shellcheck${NC}"
        else
            echo -e "${RED} shellcheck encontró errores${NC}"
        errores=1
        fi
    fi

    echo "*********************"
    echo "Ejecutando tflint"
    if [ -d "iac" ]; then
    if tflint --enable-all iac/; then
        echo "No se encontraron errores con tflint"
    else
        echo "tflint encontro errores"
        errores=1
    fi
    else
    echo "No se encontro el directorio de IaC"
    fi

    if check_tool bandit; then
        echo "*********************"
        echo "Ejecutando bandit"
        if bandit -r src/ > bandit_report.txt; then
            echo -e "${GREEN} No se encontraron errores con bandit${NC}"
        else
            echo -e "${RED} bandit encontró vulnerabilidades${NC}"
            errores=1
            cat bandit_report.txt
        fi
    fi

    echo "==============================================="
    end_time=$(date +%s)
    duration=$((end_time - start_time))

    echo -e "${YELLOW} Resumen de linting:${NC}"
    echo " Tiempo de ejecución: ${duration}s"

    if [ $errores -eq 1 ]; then
        echo -e "${RED} Se encontraron errores de linting. Revisa los mensajes anteriores.${NC}"
        exit 1
    else
        echo -e "${GREEN} Todos los linters pasaron correctamente. ¡Buen trabajo!${NC}"
        exit 0
    fi
      
    ```
#
    
* Extender`pr_validation.yaml` usando `matrix strategy` y ejecutar localmente el pull request con `act` usando `act pull_request`, todo esto especificado en el  [Issue #19](https://github.com/OliverHz28/PC3Proyecto10/issues/19)

    ```yaml
    name: PR Validation Pipeline

    on:
    pull_request:
        branches: [ main, develop ]
    workflow_dispatch:
        inputs:
        pr_ids:
            description: 'Lista de PR IDs simulados (["13","12"])'
            required: true
            default: '["local"]'

    jobs:
    validate-pr:
        runs-on: ubuntu-latest

        strategy:
        fail-fast: false
        matrix:
            python-version: [3.9]
            pr_id: ${{ github.event_name == 'workflow_dispatch'
                        && fromJson(inputs.pr_ids)
                        || fromJson('["local"]') }}
        steps:
        - name: Checkout código
            uses: actions/checkout@v2

        - name: Configurar Python
            uses: actions/setup-python@v4
            with:
            python-version: ${{ matrix.python-version }}

        - name: Instalar dependencias
            run: |
            pip install pytest pytest-cov flake8 bandit
            sudo apt-get update && sudo apt-get install -y shellcheck

        - name: Ejecutar linting
            run: |
            chmod +x ./scripts/lint_all.sh
            ./scripts/lint_all.sh


        - name: Ejecutar pruebas
            run: pytest --cov=src --cov=scripts --cov-fail-under=80

        - name: Preparar carpeta pr_simulation
            run: |
            mkdir -p pr_simulation/${{ matrix.pr_id }}

        - name: Ejecutar validacion (check_pr.py)
            working-directory: scripts
            run: |
            python3 check_pr.py

    ```
    #### [**Contribucciones**](CONTRIBUTIONS.md)
