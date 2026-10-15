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

Se incluyen **212 imágenes, 53 por clase**: las 12 fotografías iniciales y
**200 fotografías adicionales de TrashNet (50 por material)**. Se conservan
las etiquetas originales: cardboard → carton, metal → metal,
plastic → plastico y glass → vidrio. No se incluyen paper ni trash.

| Material | Iniciales | Añadidas | Total |
|---|---:|---:|---:|
| Cartón | 3 | 50 | 53 |
| Metal | 3 | 50 | 53 |
| Plástico | 3 | 50 | 53 |
| Vidrio | 3 | 50 | 53 |

El conjunto permite la división estratificada 70/15/15: 148 imágenes de
entrenamiento, 32 de validación y 32 de test. Es una base para un experimento
inicial; **añadir imágenes no elimina el margen de error**. Todavía hay que
entrenar, evaluar y comprobar la generalización con fotografías nuevas.
TrashNet contiene tomas de envases parecidos y fondos poco variados. La
revisión de hashes y similitud no certifica que sean objetos independientes.
Antes de la evaluación definitiva, hay que agrupar las tomas del mismo
objeto en una sola partición y recopilar una prueba externa independiente.

## Archivos

- `Clasificador_Residuos.ipynb`: trabajo principal en Markdown y Python.
- `requirements.txt`: dependencias del entorno.
- `dataset/`: imágenes organizadas en las cuatro clases.
- `dataset/fuentes_carton.txt` y `dataset/fuentes_metal.txt`: autores,
  procedencia y licencias de las fotografías descargadas.
- `dataset/fuentes_trashnet.txt`: fuente, selección y licencia MIT de las
  200 nuevas imágenes.
- `dataset/seleccion_trashnet.csv`: archivo original, clase, URL de origen,
  dimensiones y SHA-256 de cada fotografía añadida.
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

1. Revisar las etiquetas y las fotos del mismo objeto o sesión antes de
   entrenar. Ampliar con objetos, fondos e iluminación diferentes; mantener
   las fotos relacionadas dentro de una sola partición.
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
archivos de fuentes; mantén esas atribuciones al compartirlas. Las seis
fotografías iniciales de plástico y vidrio, y los originales de papel,
fueron aportados por el grupo y no se les ha asignado una licencia adicional.

Las 200 imágenes con prefijo `trashnet_` proceden de
[TrashNet, de Gary Thung y Mindy Yang](https://github.com/garythung/trashnet),
descargado desde el [dataset oficial](https://huggingface.co/datasets/garythung/trashnet).
Se conserva su licencia y atribución en `dataset/fuentes_trashnet.txt`.
