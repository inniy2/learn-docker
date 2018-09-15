## Operation containers  
- - - -  
```bash
docker container run --rm mongo

# Press ctrl + c

docker container ls -a

docker container run -d --name mongo mongo

docker container ls

docker container logs mongo

docker container stop mongo

docker container ls -a

docker container rm mongo

docker container ls -a

# -p publish ( expose port host_os_port:inside_port)
docker container run -d --name mongo -p 8081:27017 mongo

# -f force ( docker stop & rm )
docker container rm -f mongo

# publish delegation ( docker choose expose port)
docker container run -d --name mongo -p 27017 mongo

# -v volumn persistance ( host_os_dir:inside_dir)
docker container run -d --name mongo -p 27017 -v /tmp/mongo:/data/db mongo

# inspect volums in the container
docker container inspect mongo | grep Volumes -A5

# inspect mounts in the container
docker container inspect mongo | grep Mount -A20

# ? Tutorial says - it should be seen files even without creating the target directory
ls -al /tmp/mongo
```