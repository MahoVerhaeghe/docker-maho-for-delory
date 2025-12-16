# BONUS 2

FROM nginx:latest

RUN apt-get update && apt-get install -y git \ 
    && rm -rf /usr/share/nginx/html/* \
    && git clone https://github.com/withaarzoo/3D-Rotate-Tube.git /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]

# Les commandes Docker restent inchangées :
# docker build -t rotate-tube-nogit-image .
# docker images
# docker run --name rotate-tube-nogit-container -p 8080:80 -d rotate-tube-nogit-image
# docker ps
# On ouvre un navigateur a l'adresse http://localhost:8080
# docker stop rotate-tube-nogit-container
# docker rm rotate-tube-nogit-container