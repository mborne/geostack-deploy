
# Création d'un utilisateur et base de données PostgreSQL

## Procédure manuelle

* Créer la base de données avec l'extension PostGIS

```bash
# se connecter à la machine
ssh vagrant@vagrantbox-2
# switcher sur l'utilisateur postgres
sudo su postgres
# création d'un utilisateur geoserver
psql -c "CREATE USER geoserver WITH PASSWORD 'TheGeoServerP@ssword'"
# créer une base "gis" avec geoserver comme propriétaire
createdb -O geoserver gis
# ajouter l'extension postgis
psql -d gis -c "CREATE EXTENSION postgis"
```

* Ajouter la ligne suivante à `/etc/postgresql/14/main/pg_hba.conf` :

```
host    all             geoserver       0.0.0.0/0               scram-sha-256
```

* Recharger la configuration de PostgreSQL : `sudo service postgresql reload`

## Utilisation d'un playbook ansible

Nous pouvons aussi utiliser les modules [ansible Community.Postgresql](https://docs.ansible.com/ansible/latest/collections/community/postgresql/index.html#plugin-index) pour gérer ces points :

![postgres-db.yml](postgres-db.yml)

Pour tester : `psql -h vagrantbox-2 -U geoserver -d gis -W`
