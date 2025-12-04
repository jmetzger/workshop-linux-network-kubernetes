# https - mit letsencrypt in ingress (traefik) 

## Prerequisites 

```
ingress - example with with apple and banana deployment has been created already
```

## Schritt 1: cert-manager installieren 

```
helm repo add jetstack https://charts.jetstack.io
helm install cert-manager jetstack/cert-manager \
--namespace cert-manager --create-namespace \
--version v1.12.0 \
--set installCRDs=true
```

## Schritt 2: Create ClusterIssuer (gets certificates from Letsencrypt)

```
# cluster-issuer.yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your-email@example.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
    - http01:
        ingress:
          class: traefik
```

```
kubectl get clusterissuer letsencrypt-prod
kubectl describe clusterissuer letsencrypt-prod 
```

## Schritt 3: Ingress-Objekt mit TLS erstellen 

```
cd
mkdir -p manifests/abi
cd manifests/abi
# falls datei schon da ist 
mv ingress.yaml ingress.yaml.bkup 
nano ingress-tls.yaml 
```

```
# Ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: traefik
  tls:
  - hosts:
    - <euer-name>.app.do.t3isp.de
    secretName: example-tls
  rules:
  - host: "<euer-name>.app.do.t3isp.de"
    http:
      paths:
        - path: /apple
          pathType: Prefix
          backend:
            service:
              name: apple-service
              port:
                number: 80
        - path: /banana
          pathType: Exact
          backend:
            service:
              name: banana-service
              port:
                number: 80
```

```
kubectl apply -f .
```






## Schritt 4: Herausfinden, ob Zertifikate erstellt werden 

```
kubectl describe certificate example-tls
kubectl get cert example-tls
kubectl get secret example-tls
# cr = certificaterequest 
kubectl get cr example-tls
```




## Ref: 

  * https://hbayraktar.medium.com/installing-cert-manager-and-nginx-ingress-with-lets-encrypt-on-kubernetes-fe0dff4b1924
