# Automatisation de déploiement de clusters avec CAPI

## Prérequis

1. Forker le repo
2. Créer un token github :
Settings -> Developper settings -> Personal access tokens -> Fine-grained tokens

Créer un token avec les droits :

- Repository access : le repo forké
- User permissions : rien
- Repository permissions :
  - Read access to metadata
  - Read and Write access to administration and code

3. Modifier les secrets :
- ./apps/databases/galera/secrets.enc.yaml
- ./apps/web/wordpress/secrets.enc.yaml
- ./infra/alerts/secrets.enc.yaml

## Déploiement de CAPI
Créer le cluster kind :
```bash
kind create cluster --config kind/capi.yaml
```

Déployer CAPD :
```bash
CLUSTER_TOPOLOGY=true clusterctl init --infrastructure docker --addon helm
```

Déployer les CluserResourceSet et Helm Proxies :
```bash
kubectl apply --recursive -f apps/*
```

Déployer les clusters :
```bash
helm install capi-clusters ./charts/capi-clusters -f ./charts/capi-clusters/values.yaml
```
