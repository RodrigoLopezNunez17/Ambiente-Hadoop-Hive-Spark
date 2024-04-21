# Hadoop-Spark-Hive
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


sudo docker cp -a /home/ubuntu/Datasets/. namenode:/home/Datasets    # Se copian todos los archivos dentro de la carpeta que creamos previamente. Notar que "/home/ubuntu/Datasets" es el path de la carpeta en donde tengo los archivos y con la sentencia "/." indico que quiero todos los archivos de dicha carpeta.


sudo docker exec -it namenode bash    # Volvemos a entar en el contenedor.


hdfs dfs -mkdir /data    # Se crea un directorio en HDFS llamado "/data".


hdfs dfs -put /home/Datasets/* /data    # Finalmente, copiamos y pegamos los archivos CSV a el directorio "/data"
```
