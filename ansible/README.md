# Déploiement de GeoStack avec ansible

## Description

Illustration du déploiement de GeoServer et PostgreSQL avec Ansible.

## L'inventaire ansible

Nous avons ici un seul inventaire [inventory/vagrantbox/hosts](inventory/vagrantbox/hosts) avec un fichier [inventory/vagrantbox/README.md](inventory/vagrantbox/README.md) expliquant comment créer les machines avec vagrant.

En situation réelle, nous aurions plutôt `inventory/geostack_qlf` et `inventory/geostack_prod`.

## Les principaux playbooks

* [Le playbook postgres.yml pour le déploiement de PostgreSQL](postgres.md)
* [Le playbook geoserver.yml pour le déploiement de GeoServer](geoserver.md)

## Quelques compléments

* [Création d'une base de données PostgreSQL](postgres-db.md)

## Ressources

* [docs.ansible.com - Best Practices - Alternative Directory Layout](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_best_practices.html#alternative-directory-layout) utilisé pour structurer

