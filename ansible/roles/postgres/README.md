# Rôle ansible pour l'installation de PostgreSQL

## Points clés

* [tasks/main.yml](tasks/main.yml) correspond à la [procédure d'installation](https://www.postgresql.org/download/linux/ubuntu/) :
  * Configuration du dépôt APT PostgreSQL (`/etc/apt/sources.list.d/pgdg.list`)
  * Ajout de la clé GPF pour le dépôt
  * Installation des packages
* [defaults/main.yml](defaults/main.yml) permet de définir les valeurs par défaut du playbook (ici, les versions)
* [handlers/main.yml](handlers/main.yml) permet de rédémarrer PostgreSQL uniquement en cas de modification de `postgresql.conf` (c.f. `notify: restart postgresql` qui sera appelé uniquement à l'ajout de `listen_addresses = '*'`)

## Modules ansible utilisés

* [ansible.builtin.apt](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html) pour **installer les paquets systèmes**.
* [ansible.builtin.apt_key](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_key_module.html) pour **gérer les clés publiques des dépôts APT**.
* [ansible.builtin.template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) pour générer des fichiers à partir des variables.
* [ansible.builtin.lineinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html) pour s'assurer de la présence de la ligne `listen_addresses = '*'` dans `postgresql.conf` (**idempotence**)
