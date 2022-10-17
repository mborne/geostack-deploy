# Déploiement de GeoStack avec ansible

## Description

Illustration du déploiement de GeoServer et PostgreSQL avec Ansible.

## L'inventaire ansible

Nous avons ici un seul inventaire [inventory/vagrantbox](inventory/vagrantbox/README.md). En situation réelle, nous aurions plutôt `inventory/geostack_qlf` et `inventory/geostack_prod`.

## Les playbooks

### Déploiement de PostgreSQL

Le déploiement est réalisé à l'aide du playbook [postgres.yml](postgres.yml) comme suit :

```bash
ansible-playbook -i inventory/vagrantbox postgres.yml
```

Nous observons le résultat suivant :

![ansible-playbook postgres.yml](docs/ansible-playbook-postgres.png)

Nous notons que le playbook fait appel à un seul rôle `postgres`. Le fichier [roles/postgres/README.md](roles/postgres/README.md) explique son fonctionnement.

### Création d'une base de données

Manuellement, nous procéderions ainsi :

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

Nous noterons que nous pourrions aussi utiliser un module [Community.Postgresql](https://docs.ansible.com/ansible/latest/collections/community/postgresql/index.html#plugin-index) d'ansible pour créer cette base avec un playbook `create-db.yml` par exemple.


### Déploiement de GeoServer

Le déploiement est réalisé à l'aide du playbook [geoserver.yml](geoserver.yml) comme suit :

```bash
ansible-playbook -i inventory/vagrantbox geoserver.yml
```

Nous observons le résultat suivant :

![ansible-playbook geoserver.yml](docs/ansible-playbook-geoserver.png)

Le fichier [roles/geoserver](roles/geoserver/README.md) explique le fonctionnement du rôle utilisé par ce playbook.


Nous pouvons tester en accédant à http://vagrantbox-1:8080/geoserver/ (admin/geoserver) et en vérifiant l'état du service geoserver :

```bash
# se connecter à la machine
ssh vagrant@vagrantbox-1
# vérifier que le service est "active (running)"
sudo service geoserver status
# inspecter au besoin les logs
sudo journalctl -u geoserver -f
```

## Ressources

* [docs.ansible.com - Best Practices - Alternative Directory Layout](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_best_practices.html#alternative-directory-layout) utilisé pour structurer

