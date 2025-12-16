# BONUS 1
# git clone https://github.com/withaarzoo/3D-Rotate-Tube.git
# cd exo4/bonus

FROM caddy:latest

COPY 3D-Rotate-Tube /usr/share/caddy
# COPY ../3D-Rotate-Tube /usr/share/caddy # Je sais pas s'il est censé fonctionné ou pas mais VSC me l'a proposé
# automatiquement et ça ne fonctionne pas avec le docker build

EXPOSE 80

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]

# docker build -t rotate-tube-caddy-image .
# docker images
# docker run --name rotate-tube-caddy-container -p 8080:80 -d rotate-tube-caddy-image
# docker ps
# On ouvre un navigateur a l'adresse http://localhost:8080
# docker stop rotate-tube-caddy-container
# docker rm rotate-tube-caddy-container