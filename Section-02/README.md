Cuando expones puertos para ejecutar la imagen en un contendor y que vea el puerto

## Esto dentro de la carpeta nodejs-app-finished

```bash
docker build -t node_section_02:1.0 .
```
Primero es el local port eñl segundo es el del contenedor

```bash
docker run -p 3000:80 node_section_02:1.0
```


## Dentro de la carpeta python-app-finish.
```bash
docker build -t python_section_02:1.0 .
```
```bash
docker run -it python_section_02:1.0
docker start -a -i python_section_02:1.0
```
Esta app es  consola. por eso it

# EXOSE & A Little Utility Functionality

In the last lecture, we started a container which also exposed a port (port 80).

I just want to clarify again, that EXPOSE 80 in the Dockerfile in the end is optional. It documents that a process in the container will expose this port. But you still need to then actually expose the port with -p when running docker run. So technically, -p is the only required part when it comes to listening on a port. Still, it is a best practice to also add EXPOSE in the Dockerfile to document this behavior.

As an additional quick side-note: For all docker commands where an ID can be used, you don't always have to copy / write out the full id.

You can also just use the first (few) character(s) - just enough to have a unique identifier.

So instead of
```bash
docker run abcdefg
```
you could also run
```bash
docker run abc
```
or, if there's no other image ID starting with "a", you could even run just:
```bash
docker run a
```
This applies to ALL Docker commands where IDs are needed.

Puedo arrancar varios procesos con run no.

```bash
docker start abcdefg
```

Podemos arrancar con detach mode and attach mode.

run arrancar como attach, escucha la salida del conenedor.

```bash
docker run -d abcdefg
```

```bash
docker attach abcdefg
```
Vovlemos a ver la salida del contenedor.


Attaching to an already-running Container
By default, if you run a Container without -d, you run in "attached mode".

If you started a container in detached mode (i.e. with -d), you can still attach to it afterwards without restarting the Container with the following command:

docker attach CONTAINER
attaches you to a running Container with an ID or name of CONTAINER.

## NOTA

```bash
docker run -p 3000:80 -d --rm node_section_02:1.0
```
--rm borra el contenedor cuando paramos todo


## Inspeccionar imagenes

```bash
docker image inspect node_section_02:1.0
```

Podemos copiar ficheros a un contenedor


```bash
docker cp /dummy/. container_name:/test
```
Podemos copiar ficheros desde un contenedor
```bash
docker cp container_name:/test/text.txt .
```

Cuando arrancamos un contendor le podemos dar nombre

```bash
docker run -p 3000:80 -d --rm --name node_test node_section_02:1.0
```


Attached, you find the code / project setup snapshots for this module.

You also find a cheat sheet / short summary for this module attached.

Also consider diving into the official docs, in addition to this course: https://docs.docker.com/