# Rôle ansible pour l'installation de GeoServer

## Points clés

* [tasks/main.yml](tasks/main.yml) est adapté à partir de la [procédure d'installation de geoserver](https://docs.geoserver.org/latest/en/user/installation/linux.html) comme suit :
  * Création d'un utilisateur et d'un groupe geoserver
  * Téléchargement et extraction du [ZIP Platform Independent Binary](https://geoserver.org/release/stable/) dans `/opt/geoserver`
  * Préparation d'un répertoire `/var/geoserver_data_dir` pour le stockage des données GeoServer
  * Configuration d'un service systemd pour le démarrage de geoserver (intérêt redémarrage et gestion des logs)
* [defaults/main.yml](defaults/main.yml) permet de définir les valeurs par défaut du playbook (la version geoserver)

## Cas des mises à jour

* Ce playbook ne supporte pas les mises à jour de version GeoServer (il faudrait télécharger `/opt/geoserver-{{ geoserver_version }}` et faire un lien symbolique de `/opt/geoserver` sur `/opt/geoserver-{{ geoserver_version }}`)
* L'alternative serait la création d'un paquet debian (par exemple avec [FPM](https://fpm.readthedocs.io/)) pour pouvoir mettre à jour `/opt/geoserver` avec `dpkg -i geoserver-2.21.1.deb`.
* La solution optimale sera la création d'un dépôt debian (par exemple avec nexus) pour pouvoir mettre à jour avec `apt-get upgrade`.

## Intérêt du service systemd

L'utilisation d'un service systemd permet de :

* Contrôler de manière simple le bon fonctionnement du service (`sudo service geoserver status`)
* Simplifier la gestion des logs du service (`sudo journalctl -u geoserver -f`)
* S'assurer du redémarrage du service en cas de redémarrage de la machine

## Remarques

Il reste de nombreux points non satisfaisants :

* Le répertoire `/var/geoserver_data_dir` devrait être sauvegardé.
* Le mot de passe `geoserver` du compte `admin` GeoServer devrait être remplacé par un mot de passe généré.
* ...



