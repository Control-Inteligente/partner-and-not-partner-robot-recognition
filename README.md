# Reconocimiento de robots compañeros y no compañeros

Programa orientado a reconocer robots humanoides presentes en un encuentro de futbol
clasificandolos entre compañeros y enemigos.


## Requerimientos

Librerias           |  Hardware
------------------- | -------------------------
pyrealsense2        |  Realsense deepth camera
cv2                 |
darkflow            |
tensorflow 1.14     |
numpy               |

## ¿Cómo funciona?

El programa utiliza la arquitectura [Tiny Yolo](https://pjreddie.com/darknet/yolo/), arquitectura
orientada a dispositivos de pocos recusos.
Esta arquitectura puede ser entrenada con un dataset utilizando transferencia de aprendizaje
con los pesos del dataset [coco](https://cocodataset.org/#home).

Para obtener el resultado deseado, el problema se divide en 3 subproblemas
1. Eliminacion de fondo
2. Reconocimiento de robots
3. Clasificación de robots( compañeros y enemigos)

### Problema principal

El programa tiene como entrada la imagen del campo de juego y como salida da la imagen 
con los compañeros reconocidos
![transicion](https://user-images.githubusercontent.com/57047153/92072962-08570300-ed78-11ea-80f5-ad413a387e2e.png)


### Eliminacion de fondo

Para dar una imagen facil de procesar al bloque de reconocimiento, se hace una discriminacion de fondo.
La discriminacion se hace para evitar errores con robots presentes, que no sean parte del juego.
Esta se hace a partir de una distancia minima a la cual la camara debera ver.
Para hacer este proceso, se usa la camara de profundidad [Realsense](https://www.intelrealsense.com/stereo-depth/?utm_source=intelcom_website&utm_medium=button&utm_campaign=day-to-day&utm_content=D400_learn-more_button), obteniendo de esta las distancias
por pixel. A partir de estas distancias, la mara pinta de un color solido los pixeles que tengan
un valor mayor, siendo este color diferente de azul y rojo.

![DiscriminacionPT](https://user-images.githubusercontent.com/57047153/92073302-f3c73a80-ed78-11ea-8c80-5125759f2442.png)


### Reconocimiento de robots

Una vez teniendo la discriminacion de fondo, se hace uso de los pesos obtenidos del entrenamiento previo
de nuestra arquitectura.
Estos pesos ayudan a identificar a todos los robots presentes en la imagen.


![ConReconocimiento](https://user-images.githubusercontent.com/57047153/92073517-7819bd80-ed79-11ea-9ed6-dcfdc93ec350.png)


### Clasificación de robots

Para clasificar a los robots, se usan marcas de colores, estas marcas pueden ser de cualquier forma
y estar en cualquier parte del robot, lo necesario es que sea de color rojo y azul.
Una vez reconocidos los robots, se reduce la imagen a n numeros de imagenes, donde n es el
numero total de robots reconocidos.
Estas subimagenes se aplica un filtrado de color basado en el campo de colores HSV.
De esta forma, se escribe una etiqueta, "amigo" para el color rojo y "enemigo" para el color auzl.
Las subimagenes con las etiquetas puestas, se sobreponen en la imagen sin fondo, teniendo como 
resultado el campo de vision completo con todos los robots reconocidos y etiquetados.

![Final](https://user-images.githubusercontent.com/57047153/92073810-33daed00-ed7a-11ea-8a73-789793558541.png)
