# Déploiement de GeoStack avec docker compose

Illustration de l'utilisation de docker compose pour démarrer une stack applicative à partir d'un fichier [docker-compose.yml](docker-compose.yml) s'appuyant sur :

* L'image officielle [postgis/postgis](https://hub.docker.com/r/postgis/postgis) pour le déploiement PostgreSQL
* L'image [mborne/docker-geoserver](https://github.com/mborne/docker-geoserver#readme) construite avec GitHub actions.

Nous remarquerons la présence d'un fichier **[.env](.env)** définissant les **valeurs par défaut des variables d'environnement** utilisée dans [docker-compose.yml](docker-compose.yml)


## Démarrage de la pile applicative

```bash
# Modifier le mot de passe PostgreSQL
export POSTGRES_PASSWORD=ChangeIt
# Démarrer la pile applicative
docker compose up -d
# Vérifier que les services ont bien démarrés
docker compose ps
# Accéder aux journaux applicatifs
docker compose logs -f postgis
docker compose logs -f geoserver
```

## Accès aux services

* Accéder à GeoServer sur le port 18080 :
  * Ouvrir http://localhost:18080/geoserver/
  * Se connecter avec admin/geoserver
* Accéder à PostGIS sur le port 15432 : `psql -h localhost -p 15432 -U postgres`


## Mise en oeuvre d'un reverse proxy

Nous remarquerons dans [docker-compose.yml](docker-compose.yml) la présence de labels sur le service GeoServer configurant l'exposition avec [Traefik](https://doc.traefik.io/traefik/) sur http://gs-geoserver.dev.localhost/geoserver/

Un fichier [config/traefik.yml](config/traefik.yml) minimaliste est mis à disposition pour permettre le déploiement de [Traefik](https://doc.traefik.io/traefik/) comme suit :

```bash
docker run -d --name=traefik --restart=unless-stopped \
    --network geostack \
    -p 80:80 -p 443:443 -p 8080:8080 \
    -v $PWD/config/traefik.yml:/etc/traefik/traefik.yml \
    -v /var/run/docker.sock:/var/run/docker.sock \
    traefik:v2.10
```

Nous noterons que :

* Les ports 80, 443 et 8080 doivent être libre sur la machine (utiliser par exemple `sudo lsof -i :80` pour trouver le programme utilisant le port dans le cas contraire)
* `--network geostack` permet de s'assurer que le conteneur [traefik](https://doc.traefik.io/traefik/) a bien accès au conteneur geoserver
* `-v /var/run/docker.sock:/var/run/docker.sock` donne accès à l'API docker au conteneur traefik
