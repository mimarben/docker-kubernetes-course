# Docker compose



```bash
docker network create goals-net
```

## Database mongodb

```bash
docker run -d --name mongodb -v data:/data/db --network goals-net -e  MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=admin mongo
```

## Backend node.js
```bash
docker build -t goals-node .

docker run --name goals-backend -v /home/martinm/src/dev-src/docker-kubernetes-course/Section-05/backend:/app -v logs:/app/logs --rm -d -p 8080:8080 --network goals-net goals-node
```

el puerto 8080 esta abierto para el front end


## Frontend React SPA
```bash
docker build -t goals-react .

docker run --name goals-frontend --rm -d -p 3000:3000 -it goals-react 
```

no hace falta la network de docker porque react como todos los frontent se ejecuta en el navegador no en el contenedor


-it es muy importante, sino se para react sino se para el docker
```bash
docker run -v /home/martinm/src/dev-src/docker-kubernetes-course/Section-05/frontend/src:/app/src --name goals-frontend -rm p 3000:3000 -it goals-react 
```


