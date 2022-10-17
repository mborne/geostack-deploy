# Rôle ansible pour l'installation de PostgreSQL

## Points clés

* [tasks/main.yml](tasks/main.yml) correspond à la [procédure d'installation](https://www.postgresql.org/download/linux/ubuntu/) :
  * Configuration du dépôt APT PostgreSQL (`/etc/apt/sources.list.d/pgdg.list`)
  * Ajout de la clé GPF pour le dépôt
  * Installation des packages
* [defaults/main.yml](defaults/main.yml) permet de définir les valeurs par défaut du playbook (ici, les versions)

## Modules ansible utilisés

* [ansible.builtin.apt](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html) pour **installer les paquets systèmes**.
* [ansible.builtin.apt_key](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_key_module.html) pour **gérer les clés publiques des dépôts APT**.
* [ansible.builtin.template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) pour générer des fichiers à partir des variables.
