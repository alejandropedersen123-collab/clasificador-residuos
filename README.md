# Clasificador de residuos

Proyecto en Jupyter con explicaciones en Markdown y código Python para
clasificar un residuo principal como `carton`, `metal`, `plastico` o `vidrio`.
Utiliza ResNet-18 con transfer learning y una simulación de la decisión
robótica según la clase y la confianza.

El trabajo completo está en la rama **`clasificador-residuos`**.

## Estado del trabajo

El notebook contiene la preparación de datos, entrenamiento, validación,
evaluación, guardado del modelo y predicción de nuevas imágenes. **Todavía no
hay un modelo entrenado ni métricas experimentales.**

Se incluyen **12 imágenes de ejemplo, tres por clase**. Permiten comprobar
la estructura y la lectura de imágenes, pero no permiten la división
estratificada 70/15/15 configurada con las cuatro clases en cada partición.
El notebook se detiene en ese punto hasta ampliar el dataset. No se debe
interpretar este conjunto pequeño como una evaluación fiable del modelo.

## Archivos

- `Clasificador_Residuos.ipynb`: trabajo principal en Markdown y Python.
- `requirements.txt`: dependencias del entorno.
- `dataset/`: imágenes organizadas en las cuatro clases.
- `dataset/fuentes_carton.txt` y `dataset/fuentes_metal.txt`: autores,
  procedencia y licencias de las fotografías descargadas.
- `pruebas/entrega_companero_2026-10-15/`: originales aportados por el grupo.
  Las imágenes de papel permanecen fuera del dataset porque papel no es una
  de las cuatro clases. Las de plástico y vidrio duplican las incorporadas
  al dataset: **no deben usarse como test independiente**.

Las carpetas `modelos/` y `resultados/` se crean al ejecutar el notebook y
sus archivos generados se excluyen de Git.

## Abrir el proyecto

En GitHub, selecciona la rama `clasificador-residuos` y usa **Code → Download
ZIP**, o clona el repositorio y cambia a esa rama con:

```bash
git switch clasificador-residuos
```

Abre la carpeta completa en VS Code. Desde una terminal de PowerShell
situada en esa carpeta, crea un entorno e instala las dependencias:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m ipykernel install --user --name clasificador-residuos --display-name "Python - Clasificador Residuos"
```

Si ya tienes un entorno con estas dependencias, puedes usarlo seleccionando
su intérprete como kernel. No es necesario reinstalarlas en cada ejecución.

Abre `Clasificador_Residuos.ipynb` y selecciona **Python - Clasificador
Residuos**. También puedes abrir Jupyter desde la raíz del proyecto:

```powershell
.\.venv\Scripts\python.exe -m jupyter notebook Clasificador_Residuos.ipynb
```

## Continuar el trabajo en grupo

1. Ampliar el dataset con fotografías correctamente etiquetadas. Evitar que
   fotos del mismo objeto o sesión aparezcan en distintas particiones.
2. Reiniciar el kernel y ejecutar desde el principio para comprobar el
   entorno, el dataset y la división en train, validación y test.
3. Activar `ENTRENAR = True` cuando los datos estén preparados. La primera
   carga de ResNet-18 preentrenada puede descargar los pesos de ImageNet.
4. Ajustar el modelo usando validación y reservar test para la evaluación
   final. `EVALUAR_TEST` y el fine-tuning están desactivados por defecto.
5. Guardar los resultados reales y probar imágenes nuevas que no hayan
   participado en el entrenamiento ni en la selección del modelo.

El proyecto puede funcionar en CPU; utiliza CUDA solo si PyTorch la detecta.
La selección del dispositivo es automática.

## Procedencia de las imágenes

Las fotografías de cartón y metal conservan las licencias indicadas en sus
archivos de fuentes; mantén esas atribuciones al compartirlas. Las imágenes
de plástico, vidrio y papel fueron aportadas por el grupo y no se les ha
asignado una licencia adicional.
