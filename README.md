# Pruebas para realizar ruteo con diferentes herramientas

## pgrouting

### Para bajar una imagen

#### Instalación
 ``` 
· docker pull pgrouting/pgrouting
 ``` 

crea el contenedor pgrouting , una contraseña para postgres y carpeta para compartir datos...sin la contraseña se apaga el contenedor
sudo docker run -d --name pgrouting -e POSTGRES_PASSWORD=tu_contraseña -v /home/martin/Documents/docker/gds:/data pgrouting/pgrouting



·para inspeccionar la ip para luego abrir pgadmin
sudo docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pgrouting


#### Creación de red
La forma más facil es utilizar una red a partir de data de OSM. Vamos a necesitar el programa osm2pgrouting

1. Descargamos un polígono con la información de calles desde la web de osm
2. En la powershell de W o consola de linux
   
```
 osm2pgrouting --file "map.osm" --conf "D:/martin/pgrouting/pto_sta_cruz/mapconfig.xml" -h localhost -p 5432 -d city_routing -U postgres -W 0000 --clean
 ```

#### Algoritmo de DIJKSTRA

##### Consulta básica
 ```
select * from pgr_bdDijkstra (
	'select gid AS id,
		source,
		target,
		cost,
		length AS cost
	FROM ways',
			  86, 100, true); 
   ```
Basicamente a la tabla ways (la que tiene los ejes) le estamos diciendo que calcule el costo entre el eje con source 100 al target 86. El resultado que da es en grados ya que la tabla está en wgs84. 
La query toma el método directo. En el caso de querer un camino indirecto hay que poner false a lo último (reemplazar el true)

![imagen](https://github.com/martinfernandoortiz/routing/assets/38224115/92cb4851-3833-4541-8e28-f13baad0865d)

## uno a muchos
  ```
select * from pgr_bdDijkstra (
   'select gid AS id,
   	source,
   	target,
   	cost,
   	length AS cost
   FROM ways',
   		  86, ARRAY[100,12], true);
  ```

       
 #Consulta con elcosto en metros
 ``` select * from pgr_bdDijkstra (
	'select gid AS id, source, target, cost, ST_Length(the_geom::geography) AS cost FROM ways',
  137, 78, true);  ``` 
