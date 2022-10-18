# Le playbook postgres.yml pour le déploiement de PostgreSQL

## Utilisation

Le déploiement est réalisé à l'aide du playbook [postgres.yml](postgres.yml) comme suit :

```bash
ansible-playbook -i inventory/vagrantbox postgres.yml
```

Nous observons le résultat suivant :

![ansible-playbook postgres.yml](docs/ansible-playbook-postgres.png)

## Détail du fonctionnement

Nous notons que le playbook [postgres.yml](postgres.yml) fait appel à un seul rôle `postgres`. Le fichier [roles/postgres/README.md](roles/postgres/README.md) explique son fonctionnement.

