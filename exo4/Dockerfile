# git clone https://github.com/withaarzoo/3D-Rotate-Tube.git
# cd exo4

FROM nginx:latest

COPY 3D-Rotate-Tube /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]

# docker build -t rotate-tube-image .
# docker images
# docker run --name rotate-tube-container -p 8080:80 -d rotate-tube-image
# docker ps
# On ouvre un navigateur a l'adresse http://localhost:8080
# docker stop rotate-tube-container
# docker rm rotate-tube-container