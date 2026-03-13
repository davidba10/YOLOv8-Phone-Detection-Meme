# YOLOv8 Meme Detector

Proyecto de Computer Vision en tiempo real que detecta cuando una persona está usando el móvil frente a la cámara y muestra automáticamente un meme en pantalla.

El sistema utiliza **YOLOv8** para detección de objetos y **OpenCV** para captura de vídeo, visualización y control de ventanas.

Cuando el modelo detecta simultáneamente:

- una **persona**
- un **teléfono móvil**

y el teléfono está **dentro del bounding box de la persona**, el sistema interpreta que la persona está mirando el móvil y lanza un **meme visual en pantalla**.

El objetivo del proyecto es demostrar cómo construir **aplicaciones interactivas de visión por computador en tiempo real** combinando detección de objetos con lógica de eventos.

---

# Contenido del repositorio

- `vision_meme_detector.ipynb` → notebook con toda la implementación
- `imagenes/` → carpeta con los memes que aparecen cuando se detecta el móvil
- `README.md` → descripción del proyecto

---

# Tecnologías utilizadas

- Python
- OpenCV
- Ultralytics YOLOv8
- Numpy
- Webcam en tiempo real

---

# Arquitectura del sistema

El sistema sigue una pipeline sencilla pero muy efectiva:

```
Webcam
↓
Captura de frame con OpenCV
↓
Inferencia con YOLOv8
↓
Detección de personas y móviles
↓
Análisis espacial de bounding boxes
↓
Evento lógico (persona usando móvil)
↓
Mostrar meme en pantalla
```

La clave del sistema no es solo detectar objetos, sino **interpretar la relación entre ellos**.

---

# Modelo de detección

El proyecto utiliza el modelo preentrenado:

```
yolov8n.pt
```

Características del modelo:

- versión **nano**
- extremadamente rápido
- ideal para aplicaciones en tiempo real
- entrenado sobre el dataset **COCO**

Clases utilizadas:

```
0 → persona  
67 → teléfono móvil
```

---

# Lógica de detección

Primero el modelo detecta todos los objetos del frame.

Luego el sistema separa:

- personas detectadas
- móviles detectados

Después se comprueba si el **centro del móvil está dentro del bounding box de la persona**.

Esto se calcula con:

```python
cmx, cmy = (mx1 + mx2)//2, (my1 + my2)//2
```

Si el punto está dentro del área de la persona:

```
px1 < cmx < px2
py1 < cmy < py2
```

entonces el sistema interpreta que:

```
la persona está usando el móvil
```

---

# Sistema de memes

Cuando se detecta el evento:

```
persona + móvil
```

el sistema:

1. selecciona un meme de la carpeta `imagenes`
2. abre una ventana llamada

```
ALERTA_MEME
```

3. muestra el meme en pantalla

Los memes se recorren de forma circular:

```python
indice_meme = (indice_meme + 1) % len(lista_memes)
```

Esto permite rotar entre varios memes sin repetir siempre el mismo.

---

# Control temporal

Para evitar que el meme cambie demasiado rápido, se utiliza un control de tiempo:

```python
if (tiempo_ahora - ultimo_cambio > 2.0)
```

Esto hace que:

- el meme solo cambie **cada 2 segundos**
- el sistema sea visualmente estable

---

# Comportamiento del sistema

## Usa el móvil

persona detectada  
móvil detectado  
móvil dentro del bounding box  

→ aparece el meme.

---

## Deja de mirar el móvil

El sistema detecta que ya no se cumple la condición y:

```python
cv.destroyWindow("ALERTA_MEME")
```

La ventana del meme se cierra automáticamente.

---

# Visualización

El sistema muestra dos ventanas.

### Cámara principal

```
Camara Principal
```

posicionada a la izquierda.

---

### Ventana del meme

```
ALERTA_MEME
```

posicionada a la derecha.

Esto evita que ambas ventanas se solapen.

---

# Cómo ejecutarlo

## Instalar dependencias

```bash
pip install ultralytics opencv-python matplotlib
```

---

## Ejecutar el notebook

Clonar el repositorio:

```bash
git clone https://github.com/davidba10/YOLOv8-Meme-Detector.git
cd YOLOv8-Meme-Detector
```

Abrir el notebook:

```bash
jupyter notebook vision_meme_detector.ipynb
```

---

# Qué demuestra este proyecto

Este proyecto demuestra habilidades en:

- Computer Vision aplicada
- detección de objetos con YOLOv8
- procesamiento de vídeo en tiempo real
- manipulación de bounding boxes
- lógica espacial entre objetos
- integración OpenCV + Deep Learning
- construcción de sistemas interactivos

Más importante aún: demuestra cómo pasar de un **modelo de detección** a una **aplicación funcional real**.

---

# Posibles mejoras

Algunas mejoras naturales para futuras versiones:

- añadir sonido al detectar el móvil
- usar tracking de personas
- detectar distancia entre cara y móvil
- añadir overlay de bounding boxes
- detectar múltiples personas
- añadir interfaz gráfica
- convertir el sistema en aplicación desktop
- integrar detección de postura o atención

---

# Conclusión

Este proyecto demuestra cómo un modelo de detección de objetos puede integrarse en una aplicación interactiva de visión por computador en tiempo real.

A partir de un modelo YOLOv8 preentrenado, el sistema no solo detecta objetos, sino que interpreta la relación entre ellos para inferir un comportamiento: el uso del teléfono móvil.

Esto ilustra cómo los sistemas de visión artificial pueden utilizarse para construir interfaces inteligentes, capaces de reaccionar dinámicamente al comportamiento humano captado por la cámara.

Más allá del aspecto humorístico de mostrar memes, el mismo principio puede aplicarse a contextos reales como:

- monitorización de distracciones
- sistemas educativos
- análisis de comportamiento
- seguridad
- interacción humano-máquina
