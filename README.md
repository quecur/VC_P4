# VC_P4
# Entrega
Para la entrega de esta práctica, la tarea consiste en desarrollar un prototipo que procese uno (vídeo de ejemplo proporcionado) o varios vídeos (incluyendo vídeos de cosecha propia):

- Detecte y siga las personas y vehículos presentes.

- Detecte y lea las matrículas de los vehículos presentes.

- Cuente el total de cada clase.

- Vuelque a disco un vídeo que visualice los resultados.

- Genere un archivo CSV con el resultado de la detección y seguimiento. Se sugiere un formato con al menos los siguientes campos:

<fotograma, tipo_objeto, confianza, identificador_tracking, x1, y1, x2, y2, matrícula_en_su_caso, confianza, mx1, my1, mx2, my2, texto_matricula>

# Entrenamiento
Para el desarrollo de esta práctica, en primer lugar he seleccionado el conjunto de datos a entrenar para el modelo YOLOv11. Este conjunto de datos corresponde al sitio web de Roboflow y se puede encontrar en el siguiente enlace: [Roboflow License Plate Segmentation](https://universe.roboflow.com/projectyolo-qvrgr/license_plate_segmentation/browse?queryText=&pageSize=50&startingIndex=50&browseQuery=true). Una vez descargado el dataset, no fue necesario realizar el etiquetado de las imágenes, ya que estas ya se encontraban etiquetadas y en un formato compatible con YOLOv11 para ser entrenadas.

Con el conjunto de datos ya preparado, procedí a crear el entorno con los comandos especificados en la propia práctica, instalando las dependencias necesarias. A continuación, comencé a entrenar el modelo utilizando la CPU, pero esto resultó ser demasiado lento para mi ordenador, por lo que decidí hacer uso de la GPU para el entrenamiento.

La configuración de la GPU para este propósito me dio algunos problemas. Por un lado, probé dos versiones de CUDA diferentes. La última versión, aparentemente, no era compatible con el resto de dependencias, por lo que tuve que reducirla a la versión 11.6. Por otro lado, también hubo problemas con la versión de NumPy, que por algún motivo me generaba incompatibilidades, por lo que tuve que reducirla a una versión anterior. Después de todo esto, cuando finalmente parecía que iba a comenzar el entrenamiento, tuve errores con el tamaño de paginación configurado por defecto en mi equipo, es decir, con el tamaño de memoria virtual disponible para la GPU. Para solucionarlo, desinstalé algunos programas para aumentar la memoria disponible en el disco duro y modifiqué manualmente el tamaño de la paginación.

Solucionados todos estos problemas, el modelo finalmente comenzó a entrenarse para la detección de matrículas, con un aumento notable de la velocidad gracias al uso de la GPU. Esto me permitió entrenarlo varias veces y de diferentes formas, modificando algunos parámetros como el número de epochs y el tamaño del batch. Seleccioné el modelo que mejores resultados obtuvo y comencé a usarlo. Si bien la detección es mejorable, consideré que cumplía con los objetivos de esta práctica.


![image](https://github.com/user-attachments/assets/6497386e-b5d8-4659-bb8d-b5e0e740e548)

![image](https://github.com/user-attachments/assets/b4b427f1-bb7f-4132-a8d0-3a471f04fff5)

# Aplicación
Para el seguimiento de coches y personas, hago uso del modelo YOLOv11, mientras que para el seguimiento de matrículas utilizo el modelo entrenado específicamente para ese propósito. Empleando para ello en ambos casos la función model.track().


![image](https://github.com/user-attachments/assets/711931ec-a907-4449-a1b7-98ac002f462f)


Para la detección de números y letras en las matrículas, empleé la librería Tesseract, aunque los resultados obtenidos no fueron muy buenos. Si bien hay detección, esta es incorrecta. También hice uso de técnicas de umbralizado para mejorar la imagen en el momento de leer matrículas pero no fue suficiente para mejorar el resultado.
En la siguiente imagen se puede apreciar como efectivamente las matrículas las detecta con cierta precisión pero la detección en el interior le cuesta.


![image](https://github.com/user-attachments/assets/7c2f8f3b-3d07-402c-bba9-e54f72005c14)


Para contar el total de cada clase simplemente cada vez que se detecta un nuevo objeto de cada tipo se suma a su respectivo conteo, el problema es que tanto mi modelo como el modelo de yolov11 detecta varias veces el mismo objeto asumiendo que es un objeto diferente lo que produce que la cuenta sea bastante erronea al alza.


![image](https://github.com/user-attachments/assets/e810b7f1-f5b1-41d9-8f0a-8139b4d96904)


Para generar un video volcamos cada fotograma de salida del analisis del modelo en la variable out_video generada con la siguiente función de openCV


![image](https://github.com/user-attachments/assets/05354c73-e03f-4a52-a491-ff553712bfd0)


Y finalmente generamos un archivo CSV con la siguiente función cada vez que se realiza una detección. Separe la detección de coches de la detección de matrículas, por lo que en el archivo CSV cada vez que se detecta un coche la parte correspondiente a la matrícula aparece vacía. Esto es así porque al principio comencé utilizando el modelo de detección de matrículas solo cuando se detectaba un coche y en las coordenadas de esa región, sin embargo, no mejoraba mucho la detección respecto a utilizarlo en el frame completo pero si ralentizaba mucho la aplicación.


![image](https://github.com/user-attachments/assets/ac3b5dec-b234-4327-bafa-5df22135f62c)







