# Déploiement de GeoStack avec Kubernetes

## Pré-requis

* Installer kubectl et configurer l'accès au cluster :

```bash
export KUBECONFIG=/home/formation/k3s-deploy/output/kubeconfig.yml
kubectl get nodes
```

* [Installer CloudNativePG](https://cloudnative-pg.io/documentation/1.23/installation_upgrade/)

```bash
# Installation à l'aide des manifests
kubectl apply --server-side -f \
  https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/release-1.23/releases/cnpg-1.23.2.yaml

# Attente de la fin de l'installation
kubectl -n cnpg-system wait \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/name=cloudnative-pg \
  --timeout=90s
```

## Création du namespace

```bash
kubectl create namespace geostack
```

## Déploiement de PostgreSQL

Nous nous appuyons sur CloudNativePG pour déployer PostgreSQL à l'aide de [k8s/manifest/postgis-cluster.yaml](manifest/postgis-cluster.yaml) :

```bash
# déployer le cluster
kubectl -n geostack apply -f k8s/manifest/postgis-cluster.yaml

# suivre la création des pods par CloudNativePG
kubectl  -n geostack get pods -w

# vérifier l'état
kubectl -n geostack get pods
#NAME                READY   STATUS    RESTARTS   AGE
#postgis-cluster-1   1/1     Running   0          2m55s
#postgis-cluster-2   1/1     Running   0          102s
#postgis-cluster-3   1/1     Running   0          24s
```

## Déploiement de GeoServer

Nous utilisons le manifest [k8s/manifest/geoserver.yaml](manifest/geoserver.yaml) pour déployer l'[image hébergée sur GitHub Container Registry](https://github.com/mborne/docker-geoserver/pkgs/container/geoserver) :

```bash
# Déploiement de geoserver
kubectl -n geostack apply -f k8s/manifest/geoserver.yaml
# Inspection des ressources
kubectl -n geostack get pods,svc,ingress,pvc -l app=geoserver
```

Nous aurons accès à GeoServer sur l'URL http://geoserver.dev.quadtreeworld.net/geoserver/

## Import de données

Nous pouvons utiliser [k8s/import/job-import-naturalearth.yaml](import/job-import-naturalearth.yaml) pour lancer un job d'import des données :

```bash
# Création du job
kubectl -n geostack apply -f k8s/import/job-import-naturalearth.yaml
# Suivi du job
kubectl -n geostack get job -w
# Suivi des logs
kubectl  -n geostack logs job/import-naturalearth -f
```
