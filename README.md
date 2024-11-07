# VC_P4
# Entrega
Para la entrega de esta práctica, la tarea consiste en desarrollar un prototipo que procese uno (vídeo de ejemplo proporcionado) o varios vídeos (incluyendo vídeos de cosecha propia):

- Detecte y siga las personas y vehículos presentes.

- Detecte y lea las matrículas de los vehículos presentes.

- Cuente el total de cada clase.

- Vuelque a disco un vídeo que visualice los resultados.

- Genere un archivo CSV con el resultado de la detección y seguimiento. Se sugiere un formato con al menos los siguientes campos:

<fotograma, tipo_objeto, confianza, identificador_tracking, x1, y1, x2, y2, matrícula_en_su_caso, confianza, mx1, my1, mx2, my2, texto_matricula>

# Desarrollo
Para el desarrollo de esta práctica, en primer lugar he seleccionado el conjunto de datos a entrenar para el modelo YOLOv11. Este conjunto de datos corresponde al sitio web de Roboflow y se puede encontrar en el siguiente enlace: [Roboflow License Plate Segmentation](https://universe.roboflow.com/projectyolo-qvrgr/license_plate_segmentation/browse?queryText=&pageSize=50&startingIndex=50&browseQuery=true). Una vez descargado el dataset, no fue necesario realizar el etiquetado de las imágenes, ya que estas ya se encontraban etiquetadas y en un formato compatible con YOLOv11 para ser entrenadas.

Con el conjunto de datos ya preparado, procedí a crear el entorno con los comandos especificados en la propia práctica, instalando las dependencias necesarias. A continuación, comencé a entrenar el modelo utilizando la CPU, pero esto resultó ser demasiado lento para mi ordenador, por lo que decidí hacer uso de la GPU para el entrenamiento.

La configuración de la GPU para este propósito me dio algunos problemas. Por un lado, probé dos versiones de CUDA diferentes. La última versión, aparentemente, no era compatible con el resto de dependencias, por lo que tuve que reducirla a la versión 11.6. Por otro lado, también hubo problemas con la versión de NumPy, que por algún motivo me generaba incompatibilidades, por lo que tuve que reducirla a una versión anterior. Después de todo esto, cuando finalmente parecía que iba a comenzar el entrenamiento, tuve errores con el tamaño de paginación configurado por defecto en mi equipo, es decir, con el tamaño de memoria virtual disponible para la GPU. Para solucionarlo, desinstalé algunos programas para aumentar la memoria disponible en el disco duro y modifiqué manualmente el tamaño de la paginación.

Solucionados todos estos problemas, el modelo finalmente comenzó a entrenarse para la detección de matrículas, con un aumento notable de la velocidad gracias al uso de la GPU. Esto me permitió entrenarlo varias veces y de diferentes formas, modificando algunos parámetros como el número de epochs y el tamaño del batch. Seleccioné el modelo que mejores resultados obtuvo y comencé a usarlo. Si bien la detección es mejorable, consideré que cumplía con los objetivos de esta práctica.

Para el seguimiento de coches y personas, hago uso del modelo YOLOv11, mientras que para el seguimiento de matrículas utilizo el modelo entrenado específicamente para ese propósito.

Para la detección de números y letras en las matrículas, empleé la librería Tesseract, aunque los resultados obtenidos no fueron muy buenos. Si bien hay detección, esta es incorrecta.




