# Automatisation de déploiement de clusters avec CAPI

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
kubectl apply --server-side=true --recursive -f apps
```

Déployer les clusters :
```bash
helm install capi-clusters ./charts/capi-clusters -f ./charts/capi-clusters/values.yaml
```
