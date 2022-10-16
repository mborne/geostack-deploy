# Rôle ansible pour l'installation de PostgreSQL

## Points clés

* [tasks/main.yml](tasks/main.yml) correspond à la [procédure d'installation](https://www.postgresql.org/download/linux/ubuntu/) :
  * Configuration du dépôt APT PostgreSQL (`/etc/apt/sources.list.d/pgdg.list`)
  * Ajout de la clé GPF pour le dépôt
  * Installation des packages
* [defaults/main.yml](defaults/main.yml) permet de définir les valeurs par défaut du playbook (ici, les versions)

