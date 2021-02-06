# Open Speed Test K8S Base Manifest


These are the base manifests to deploy [Open Speed Test](http://openspeedtest.com/) to k8s. 


## Notes

Expected to used with Kustomizer. You can setup a local file and add ingress or other items needed.

## Example

Setting up a local deployment that also has a

kustomization.yaml
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization


namespace: ost

commonLabels:
  app: ost

resources:
  - github.com/beckje01/ost-k8s/manifests?ref=v0.2
  - ingress.yaml
```

ingress.yaml
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ost-ui
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
  - host: ost.lab.example
    http:
      paths:
      - backend:
          serviceName: ost
          servicePort: 8080
```

Build:
```
kustomize build ./ | kubectl apply -f -
```