## Using docker compose  
- - - -  
```bash
# Version check
docker-compose --version

# Up help
docker-compose up --help

# Get demo
cd ~/git

git clone https://github.com/vfarcic/go-demo.git

cd go-demo

cat docker-compose.yml

docker-compose up -d

docker-compose ps

# | jq '.' will present information with jason format
docker container inspect go-demo_app_1 | jq '.'

# Read inspect info to get auto assigned host port
PORT=$(docker container inspect go-demo_app_1 | jq -r '.[0].NetworkSettings.Ports."8080/tcp"[0].HostPort')

echo $PORT

# Check
curl localhost:$PORT/demo/hello

docker-compose ps

# compose will stop and rm the container in the project
docker-compose down

docker-compose ps
```