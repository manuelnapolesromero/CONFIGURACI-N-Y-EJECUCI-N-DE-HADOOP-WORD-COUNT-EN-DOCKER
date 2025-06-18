# **CONFIGURACION-Y-EJECUCION-DE-HADOOP-WORD-COUNT-EN-DOCKER**
Este README detalla el proceso para configurar un entorno Hadoop en Docker, ejecutar comandos HDFS, gestionar archivos y realizar un ejemplo de "word count" en Hadoop.

**Resumen del Proyecto**

Este proyecto tiene como objetivo principal demostrar la configuración y operación básica de un clúster Hadoop utilizando Docker. Cubre todos los pasos, desde el despliegue del entorno Docker hasta la ejecución de un trabajo de "word count", incluyendo la interacción fundamental con el sistema de archivos distribuido HDFS. También se documentan los desafíos comunes enfrentados durante el proceso y sus respectivas soluciones colaborativas, ofreciendo una guía completa para la replicación.

**Requisitos Previos**

Antes de comenzar, asegúrate de tener instalados los siguientes programas en tu máquina:

Docker Desktop: Necesario para ejecutar los contenedores Docker que conformarán el clúster Hadoop.

Git: Imprescindible para clonar este repositorio y gestionar el código.
Pasos de Configuración y Ejecución
Sigue estos pasos para replicar el entorno Hadoop y ejecutar el ejemplo de Word Count.
1. Clonar el Repositorio
Primero, clona este repositorio en tu máquina local y navega hasta la carpeta del proyecto:

Bash


git clone <URL_DEL_REPOSITORIO>
cd Docker_Hadoop # Asegúrate de que el nombre de la carpeta sea correcto


2. Acceder al Nodo Maestro de Hadoop
Una vez que los contenedores Docker estén levantados (usando docker-compose up -d), puedes acceder al terminal del contenedor namenode:

Bash


docker exec -it namenode bash


3. Descargar y Ubicar Archivos Necesarios
Descarga el archivo JAR de ejemplos de Hadoop y un archivo de texto (por ejemplo, un libro) que se utilizará para el conteo de palabras.
Archivo JAR de Ejemplos:
Descarga hadoop-mapreduce-examples-2.7.1-sources.jar (o la versión que necesites) y guárdalo en la misma carpeta donde tienes tu configuración de Docker Hadoop (ej. Docker_Hadoop/).
Archivo de Texto (Libro):
Descarga un libro o cualquier archivo .txt (ejemplo: "No, mis días en la librería Morisaki.txt") y guárdalo en la misma carpeta que el archivo JAR.
4. Ejecutar Comandos HDFS
Una vez dentro del contenedor namenode, puedes empezar a interactuar con HDFS.
Crear un directorio de entrada en HDFS:

Bash


hdfs dfs -mkdir -p /user/root/input


5. Copiar Archivos al Nodo Maestro
Desde tu máquina local (fuera del contenedor Docker), copia los archivos descargados al directorio /tmp del contenedor namenode:

Bash


docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp/
docker cp "No, mis días en la librería Morisaki.txt" namenode:/tmp/


6. Mover Archivos dentro de HDFS
Ahora, dentro del terminal del contenedor namenode, mueve el archivo de texto del directorio /tmp a tu directorio de entrada en HDFS:

Bash


hdfs dfs -put /tmp/"No, mis días en la librería Morisaki.txt" /user/root/input/


7. Ejecutar el Ejemplo Word Count
Una vez que el archivo de texto esté en HDFS, puedes ejecutar el ejemplo de Word Count:

Bash


hadoop jar /tmp/hadoop-mapreduce-examples-2.7.1-sources.jar wordcount /user/root/input/"No, mis días en la librería Morisaki.txt" /user/root/output/contador


Nota: Asegúrate de que el nombre del archivo JAR sea el correcto y que el nombre del archivo de texto coincida.
8. Ver el Resultado del Word Count
Para ver el resultado del trabajo de Word Count, usa el siguiente comando:

Bash


hdfs dfs -cat /user/root/output/contador/part-r-00000


Nota: El nombre del archivo de salida puede variar, pero generalmente es part-r-00000.
9. Listar Archivos en HDFS
Puedes listar los archivos en tu directorio de usuario en HDFS para verificar la existencia de los archivos de entrada y salida:

Bash


hdfs dfs -ls /user/root


10. Copiar Archivo de Salida al Sistema Local
Para obtener el archivo de resultados del Word Count en tu máquina local (fuera del contenedor Docker):

Bash


docker cp namenode:/user/root/output/contador/part-r-00000 /home/tu_usuario/Documentos/Docker_Hadoop/libreria_Morisaki_wc.txt


Asegúrate de reemplazar /home/tu_usuario/Documentos/Docker_Hadoop/ con la ruta de tu directorio local deseado y el nombre del archivo de salida.
Dificultades y Soluciones
Durante el proceso, se encontraron varias dificultades comunes que fueron resueltas colaborativamente:
Acceso al Nodo Maestro y Reenvío de Puertos
Problema: Dificultad para acceder al nodo maestro después de docker-compose up y para configurar el reenvío de puertos para visualizar el panel de control.
Solución: Se utilizó el comando docker exec -it namenode bash para acceder al terminal del contenedor namenode. Se clarificó que el reenvío de puertos no fue crítico para la ejecución del ejemplo de Word Count en la terminal.
Errores de Permisos y Sintaxis de Comandos
Problema: Errores de "permission denied" y sintaxis incorrecta al ejecutar comandos HDFS (ej. hdfs dfs -mkdir, hdfs dfs -put).
Solución: Se identificó la necesidad de usar sudo en algunos casos (aunque en este contexto, se corrigió principalmente la sintaxis y el usuario correcto del contenedor). La corrección colaborativa de espacios, guiones y mayúsculas/minúsculas en los comandos HDFS fue clave.
Ubicación de Archivos
Problema: Dificultad para ubicar la carpeta correcta de Docker Hadoop para guardar los archivos descargados y para copiar archivos entre el sistema local y el contenedor.
Solución: Se siguió una guía paso a paso para descargar y mover los archivos a la ubicación correcta en el sistema de archivos local y luego se utilizaron comandos docker cp precisos para transferirlos al contenedor.
Subida a GitHub
Problema: Errores de autenticación persistentes, problemas con la configuración del repositorio local y errores al usar comandos git push.
Solución: Se intentó configurar las credenciales de Git (git config --global user.name, user.email). Se realizaron múltiples intentos de git pull, git add., git commit, y git push. El problema principal se atribuyó a errores de contraseña o autenticación con GitHub. 
Nota: Solicitar retroalimentación para  continuar con la subida.
