docker build -t feedback-node .   

docker run -p 3000:80 -d --name feedback-app --rm feedback-node


 docker volume ls                                                                                                                                                  ✔ 
DRIVER    VOLUME NAME
local     4b8734d8418038038dbc063386bfb0c05f4e2449e4a0389657d2d755ff056496


docker run -p 3000:80 -d --name feedback-app --rm feedback-node:volumes                                                                                           ✔ 
fdb39857b7f21e1135d68c999e70697ee02461a574c34057ca7ef3595ce9bd43
      data-volumes-01-starting-setup  on    main !16  docker volume ls                                                                                                                                                  ✔ 
DRIVER    VOLUME NAME
local     4b8734d8418038038dbc063386bfb0c05f4e2449e4a0389657d2d755ff056496
      data-volumes-01-starting-setup  on    main !16  docker stop feedback-app                                                                                                                                          ✔ 
feedback-app
      data-volumes-01-starting-setup  on    main !16  docker volume ls                                                                                                                                     ✔  took 10s   
DRIVER    VOLUME NAME


## Con  esto adjuntamos el volumen con nombre donde queremos no anonimo como antes, que se pierde


docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes 

docker volume ls                                                                                                                                                  ✔ 
DRIVER    VOLUME NAME
local     feedback

Cuando arranquemos tenemos que usar el mismo nombre de volumen.


## Removing Anonymous Volumes
We saw, that anonymous volumes are removed automatically, when a container is removed.

This happens when you start / run a container with the --rm option.

If you start a container without that option, the anonymous volume would NOT be removed, even if you remove the container (with docker rm ...).

Still, if you then re-create and re-run the container (i.e. you run docker run ... again), a new anonymous volume will be created. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).

Now you just start piling up a bunch of unused anonymous volumes - you can clear them via docker volume rm VOL_NAME or docker volume prune.

-----
```bash
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/miguel/src/docker-kubernetes/Section-03/data-volumes":/app -v /app/node_modules feedback-node:volumes  
```

sin -v /app/node_modules

1- Crea la imagen e instala package.json, pero luego montamos nuestra carpeta de la app que no lo tiene y los borra, poniendo /app/node_modules al ser mas precisa docker monta un volumen sin nombre y respeta node_modules para que arranque todo bien.


Solo escritura
```bash
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/miguel/src/docker-kubernetes/Section-03/data-volumes":/app:ro -v /app/node_modules feedback-node:volumes
```

-v "/ruta/local":/ruta/en/contenedor:ro
Esto significa:

El contenedor puede leer los archivos en /ruta/en/contenedor.

El contenedor no puede escribir ni modificar esos archivos.

Tu máquina local sí puede modificar los archivos en /ruta/local (fuera del contenedor).


# Adding more to the .dockerignore File
You can add more "to-be-ignored" files and folders to your .dockerignore file.

For example, consider adding the following to entries:

Dockerfile
.git
This would ignore the Dockerfile itself as well as a potentially existing .git folder (if you are using Git in your project).

In general, you want to add anything which isn't required by your application to execute correctly.

# Envirotment variables


```bash
docker run -d -p 3000:8000 --rm --name feedback-app --env PORT 8000 -v feedback:/app/feedback -v "/home/miguel/src/docker-kubernetes/Section-03/data-volumes":/app:ro -v /app/node_modules feedback-node:volumes
```

Si tenemos un fichero .env

```bash
docker run -d -p 3000:8000 --rm --name feedback-app --env-file ./.env -v feedback:/app/feedback -v "/home/miguel/src/docker-kubernetes/Section-03/data-volumes":/app:ro -v /app/node_modules feedback-node:volumes
```


# Environment Variables & Security
One important note about environment variables and security: Depending on which kind of data you're storing in your environment variables, you might not want to include the secure data directly in your Dockerfile.

Instead, go for a separate environment variables file which is then only used at runtime (i.e. when you run your container with docker run).

Otherwise, the values are "baked into the image" and everyone can read these values via docker history <image>.

For some values, this might not matter but for credentials, private keys etc. you definitely want to avoid that!

If you use a separate file, the values are not part of the image since you point at that file when you run docker run. But make sure you don't commit that separate file as part of your source control repository, if you're using source control.


# Arguments

```Dockerfile
FROM node:22.20-alpine

ARG DEFAULT_PORT=80

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . .

ENV PORT=$DEFAULT_PORT

EXPOSE $PORT

#VOLUME [ "/app/feedback" ]

EXPOSE 80

CMD ["npm", "start"]
```

```bash
docker build -t feedback-node:dev --build-arg DEFAULT_PORT=81 .
```
