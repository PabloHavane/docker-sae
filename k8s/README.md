# Structure Kustomize - Local vs Prod

## Organisation

```
k8s/
├── base/                 # Configuration commune (base)
├── overlays/
│   ├── local/           # Surcharges pour développement local
│   └── prod/            # Surcharges pour production
```

## Utilisation

### Local (développement)
```bash
# Preview
kustomize build k8s/overlays/local

# Déployer
kubectl apply -k k8s/overlays/local

# Vérifier
kubectl get pods -n cpn-app
```

### Production
```bash
# Preview
kustomize build k8s/overlays/prod

# Déployer
kubectl apply -k k8s/overlays/prod

# Vérifier
kubectl get pods -n cpn-app
```

## Différences Local vs Prod

### Replicas
- **Local**: 1 replica PHP-FPM, 1 web, 1 redis
- **Prod**: 2 replicas PHP-FPM, 2 web, 1 redis

### Ressources CPU/Memory
- **Local**: Ressources réduites (développement)
- **Prod**: Ressources augmentées (haute disponibilité)

### ConfigMaps
- **Local**: Configuration locale par défaut
- **Prod**: Configuration prod depuis `.env.prod`

### Labels & Annotations
- Chaque overlay ajoute des labels et annotations pour identifier l'environnement

## Ajouter des fichiers à la base

1. Placer les fichiers dans `k8s/base/`
2. Référencer dans `k8s/base/kustomization.yaml` sous `resources`
3. Les overlays hériteront automatiquement

## Surcharger une ressource

Pour modifier une ressource spécifiquement pour un environnement, ajouter un patch dans l'overlay correspondant.
