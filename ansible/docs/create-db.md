# Procédure de création d'un utilisateur et d'une base de données PostgreSQL

Cette procédure illustre la création d'un utilisateur **geoserver** propriétaire d'une base de données **gis** avec l'extension postgis activée :

```bash
# se connecter à la machine
ssh vagrant@vagrantbox-2
# switcher sur l'utilisateur postgres
sudo su postgres
# créer un utilisateur geoserver
psql -c "CREATE USER geoserver WITH PASSWORD 'TheGeoServerP@ssword'"
# créer une base "gis" avec geoserver comme propriétaire
createdb -O geoserver gis
# ajouter l'extension postgis
psql -d gis -c "CREATE EXTENSION postgis"
```

En complément, il convient d'autoriser les accès cette base de données en ajoutant la ligne suivante à `/etc/postgresql/15/main/pg_hba.conf` :

```
host    all             geoserver       0.0.0.0/0               scram-sha-256
```

Pour que ce changement soit pris en compte, il conviendra de recharger la configuration de PostgreSQL comme suit :

```bash
sudo service postgresql reload
```

