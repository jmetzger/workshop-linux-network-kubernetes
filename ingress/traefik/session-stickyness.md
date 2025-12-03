# Session Stickyness 

  * When you want to use session stickyness you have to use IngressRoute (CRD from traefik), instead auf Ingress
  * This is an advancement of Ingress, specifically from traefik

```
apiVersion: traefik.io/v1alpha1
kind: IngressRoute
metadata:
  name: app-ingressroute
spec:
  entryPoints:
    - web
  routes:
    - match: Host(`app.example.com`)
      kind: Rule
      services:
        - name: app-v1
          port: 80  # <-- Das fehlt!
          sticky:
            cookie:
              name: traefik-sticky-cookie
              secure: true
              httpOnly: true
```

## Reference: 

  * https://doc.traefik.io/traefik/reference/routing-configuration/kubernetes/crd/http/ingressroute/
