Muy útil  para no instalar nada en tu pc


docker build -t node-util .


docker run -it node-util npm init 
docker run -it -v /home/miguel/src/docker-kubernetes-course/Section-07:/app  node-util npm init


añadimos ENTRYPOINT [ "npm" ] al docker

creamos una imagen nueva

docker build -t node-util-npm .

docker run -it -v /home/miguel/src/docker-kubernetes-course/Section-07:/app  node-util-npm init