# routing
Some test for routing (pgrouting)



· docker pull pgrouting/pgrouting

crea el contenedor pgrouting , una contraseña para postgres y carpeta para compartir datos...sin la contraseña se apaga el contenedor
sudo docker run -d --name pgrouting -e POSTGRES_PASSWORD=tu_contraseña -v /home/martin/Documents/docker/gds:/data pgrouting/pgrouting





·para inspeccionar la ip para luego abrir pgadmin
sudo docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pgrouting





Para la carga de la ruta... Descargar en osm.
En la powershell de W o consola de linux
```  osm2pgrouting --file "map.osm" --conf "D:/martin/pgrouting/pto_sta_cruz/mapconfig.xml" -h localhost -p 5432 -d city_routing -U postgres -W 0000 --clean
 ``` 

#Consulta básica
 ``` select * from pgr_bdDijkstra (
	'select gid AS id, source, target, cost, length AS cost FROM ways',
  137, 78, true);  ``` 
  
 #Consulta con elcosto en metros
 ``` select * from pgr_bdDijkstra (
	'select gid AS id, source, target, cost, ST_Length(the_geom::geography) AS cost FROM ways',
  137, 78, true);  ``` 
