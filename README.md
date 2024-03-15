# routing
Some test for routing (pgrouting)



· docker pull pgrouting/pgrouting

crea el contenedor pgrouting , una contraseña para postgres y carpeta para compartir datos...sin la contraseña se apaga el contenedor
sudo docker run -d --name pgrouting -e POSTGRES_PASSWORD=tu_contraseña -v /home/martin/Documents/docker/gds:/data pgrouting/pgrouting





·para inspeccionar la ip para luego abrir pgadmin
sudo docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pgrouting
