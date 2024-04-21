# Ambiente Hadoop-Hive
Se creará un ambiente en Docker dónde se utilizarán con herramientas utilizadas en Big Data.

## 1) Crear un ambiente Hadoop con un Master y un Slave.

Con el archivo "docker-compose-hdfs.yml" se levantan cuatro contenedores (Namenode, Datanode, Resourcenode, Nodemanager).
```
sudo docker-compose -f docker-compose-hdfs.yml up -d
```

Ahora, se requiere insertar nuestros archivos CSV dentro de nuestro ambiente Hadoop. Para ello:
```
sudo docker exec -it namenode bash    # Se entra en el contenedor "namenode".


cd /home    # Nos ubicamos en la carpeta "home".


mkdir Datsets    # Creamos una nueva carpeta llamada "Datasets"


exit    # Salimos del contenedor.


sudo docker cp -a /home/ubuntu/Datasets/. namenode:/home/Datasets    # Se copian todos los archivos dentro de la carpeta que creamos previamente. Notar que"/home/ubuntu/Datasets" es el path de la carpeta en donde tengo los archivos y con la sentencia "/." indico que quiero todos los archivos de dicha carpeta.


sudo docker exec -it namenode bash    # Volvemos a entar en el contenedor.


hdfs dfs -mkdir /data    # Se crea un directorio en HDFS llamado "/data".


hdfs dfs -put /home/Datasets/* /data    # Finalmente, copiamos y pegamos los archivos CSV a el directorio "/data"
```

## 2) Hive.
Ahora pasemos a crear y configurar nuestro entorno Hive. Teniendo en cuenta que lo haremos sobre nuestro entorno Hadoop. Para ello levantamos nuestros contenedores con el archivo "docker-compose-hdfs-hive.yml"
```
sudo docker-compose -f docker-compose-hdfs-hive.yml up -d
```
Una vez levantado, podemos crear nuestras tablas a partir de los archivos que introducimos en Hadoop. Para ello:
```
sudo docker-compose -f docker-compose-hdfs-hive.yml up -d    # Entramos al contendor.


hive       # Entramos a nuestro entorno Hive.
```
En este caso, el código para crear las tablas se incrustaron en el archivo "Tablas.hql".

## 3) MongoDB.
Ahora que ya se tiene hace falta un lugar dónde guardar las tablas previamente creadas. En este caso se usará MongoDB:
```
sudo docker pull mongo    # Descargar la imagen de MongoDB


sudo mkdir -p /mongodata    # Se crea un directorio que servirá como volumen de nuestro contenedor.


sudo docker run -it -v mongodata:/data/db -p 27017:27017 --name mongodb -d mongo
# Se levanta y configura un contenedor de MongoDB en el puerto 27017 (default) con el nombre mongodb.


sudo docker exec -it mongodb bash    # Se entra en el contenedor.


mongo    # Se inicia el MongoDB shell.
```


## 4) Spark.
Con el archivo "docker-compose-hdfs-hive-spark.yml" se levantan dos contenedores (spark-master y spark-worker-1).
```
sudo docker-compose -f docker-compose-hdfs-hive-spark up -d    # Se levantan los contenedores.


sudo docker exec -it spark-master bash    # Se entra en el contenedor "spark-master".


/spark/bin/pyspark --master spark://spark-master:7077    # Se inica PySpark
```
