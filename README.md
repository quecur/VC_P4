# VC_P4
# Entrega
Para la entrega de esta práctica, la tarea consiste en desarrollar un prototipo que procese uno (vídeo ejemplo proporcionado) o varios vídeos (incluyendo vídeos de cosecha propia)):


- detecte y siga las personas y vehículos presentes
  
- detecte y lea las matrículas de los vehículos presentes
  
- cuente el total de cada clase
  
- vuelque a disco un vídeo que visualice los resultados
  
- genere un archivo csv con el resultado de la detección y seguimiento. Se sugiere un formato con al menos los siguientes campos:
  
<fotograma, tipo_objeto, confianza, identificador_tracking, x1, y1, x2, y2, matrícula_en_su_caso, confianza, mx1,my1,mx2,my2, texto_matricula>

# Desarrollo
Para el desarrollo de esta práctica en primer lugar he seleccionado el conjunto de datos a entrenar por el modelo yolov11, este corresponde al sitio web de roboflow y se puede encontrar en el siguiente enlace https://universe.roboflow.com/projectyolo-qvrgr/license_plate_segmentation/browse?queryText=&pageSize=50&startingIndex=50&browseQuery=true. Una vez descargado el dataset no ha sido necesario realizar el etiquetado de las imagenes para poder realizar el entrenamiento puesto que estas ya han sido descargadas en un formato que permita ser entrenado por yolov11 con su correspondiente etiquetado, separación en conjunto de datos de entrenamiento, validación, test y su archivo con extensión yaml.


Una vez preparado el conjunto de datos procedi a crear el entorno con los comandos especificados en la propia práctica instalando las dependencias necesarias. A continuación procedí a entrenar el modelo con el uso de la CPU, esto resultó ser demasiado lento para mi ordenador y decidi hacer uso de la GPU para el entrenamiento. 
La configuración de la gpu para este propósito me dio algunos problemas, por un lado prove dos versiones de CUDA diferentes, la última por lo visto, no era compatible con el resto de dependecias así que tuve que reducirla a la version 11.6, por otra parte, lo mismo sucedió con la versión de numpy que por algún motivo tambien me daba incompatibilidades y tuve que bajarla. Hecho todo esto cuando por fin parecía que iba a comenzar el entrenamiento tuve errores con el tamaño de paginación configurado en mi equipo por defecto, es decir, el tamaño de memoria virtual disponible para la GPU, por lo que desinstale algunos programas para aumentar la memoria disponible en el disco duro y modifique manualmente el tamaño de paginación. 


Hecho todo esto el modelo finálmente comenzo a entrenarse para la detección de mátriculas con un aumento de velocidad notable gracias al uso de la GPU, esto me permitió entrenarlo varias veces y de diferentes formas modificando algunos parámetros como el número de epochs y el tamaño de los lotes. Seleccioné el modelo que mejores resultados obtuvo y comence a usarlo. Si bien la detección es mejorable considero que es decente para los objetivos de esta práctica. 


Para el seguimiento de coches y personas hago uso del modelo yolov11, mientras que, para el seguimiento de mátriculas hago uso del nuevo modelo. 
Para la detección de numeros y letras empleo la liberia tesseract sin obetener muy buenos resultados.



