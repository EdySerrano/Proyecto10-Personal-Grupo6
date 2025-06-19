# **Sprint 1**
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
[Sprint 1 (Dia 3: 9/06/2025) Grupo 6 Proyecto 10 ](https://www.youtube.com/watch?v=ZwcuikAZ56w)
#### **Mi Rol**
  * Sprint 1: Me encargue de la elavoracion de los test para el archivo `config_modifier.py` y crear el esqueleto del workflow inicial (`pr_validation.yaml`):

    ```
        .
        ├───.github/
                └── workflows/
                    └── pr_validation
        ├─── tests/
                └── test_config_modifier.py
        └── README.md
    ```
## Contribucion:

* Desarrollar los test de `config_modifier.py` en `tests` para el [Issue #2](https://github.com/OliverHz28/PC3Proyecto10/issues/2)

    ```python
  # tests/test_config_modifier.py

  import pytest
  import json
  import sys
  import os

  sys.path.insert(0, os.path.abspath(
      os.path.join(os.path.dirname(__file__), '..', 'src')))

  from config_modifier import leer_json, incrementar_version

  @pytest.fixture
  # Preparamos un archivo temporal para pruebas
  def json_de_prueba(tmp_path):
      file_path = tmp_path / "config.json"
      data = {"version": 1.0, "name": "Test App"}
      with open(file_path, 'w') as f:
          json.dump(data, f)
      return file_path


  # Preparamos un archivo JSON valido para probar la lectura correcta
  def test_leer_json_valido(json_de_prueba):
      contenido = leer_json(json_de_prueba)
      assert contenido["version"] == 1.0
      assert contenido["name"] == "Test App"


  # Preparamos un archivo JSON con el campo 'version'
  # valido para probar el incremento de version
  def test_incrementar_version(json_de_prueba):
      nueva_version = incrementar_version(json_de_prueba)
      assert nueva_version == 2.0
      with open(json_de_prueba) as f:
          datos = json.load(f)
      assert datos["version"] == 2.0


  # Preparamos un archivo  con contenido invalido (no JSON)
  def test_leer_json_invalido(tmp_path):
      file_path = tmp_path / "invalido.json"
      file_path.write_text("esto no es json")
      with pytest.raises(ValueError):
          leer_json(file_path)


  # Preparamos un archivo JSON con campo 'version' de tipo incorrecto
  def test_incrementar_version_tipo_incorrecto(tmp_path):
      file_path = tmp_path / "version_invalida.json"
      with open(file_path, 'w') as f:
          json.dump({"version": "uno"}, f)
      with pytest.raises(TypeError):
          incrementar_version(file_path)

    ```
* Crear es esqueleto inicial del workflow en `pr_validation.py` segun el [Issue #3](https://github.com/OliverHz28/PC3Proyecto10/issues/3)

    ```yaml
    name: PR Validation  

    on:
      push:              
      pull_request:     

    jobs:
      placeholder:
        runs-on: ubuntu-latest   

        steps: # el job placeholder es uno vacio
          - name: Paso de ejemplo (Placeholder)  
            run: echo "Workflow placeholder"

          # Aqui se pueden agregar pasos siguientes, como:
          # - Validacion de formato de codigo (linter)
          # - Ejecucion de pruebas unitarias
          # - Validacion de la construccion del proyecto (build)
      
    ```

    #### [**Contribucciones**](CONTRIBUTIONS.md)
