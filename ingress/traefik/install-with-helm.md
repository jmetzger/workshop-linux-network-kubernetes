# Install with helm

```
helm repo add traefik https://traefik.github.io/charts


helm upgrade --install traefik traefik/traefik --version 37.4.0 --create-namespace --namespace=ingress --skip-crds
# Use special crds helm chart instead, because it does not deploy crds for gateway-api by default
# We get an error on digitalocean doks
helm install ingress traefik/traefik-crds --version 1.12.0
```
