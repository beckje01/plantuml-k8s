# PlantUML Server K8S Base Manifest


These are the base manifests to deploy [Plant UML Server](https://hub.docker.com/r/plantuml/plantuml-server) to k8s. 


## Notes

Expected to used with Kustomizer. You can setup a local file and add ingress or other items needed.

## Example

Setting up a local deployment that also has a

kustomization.yaml
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization


namespace: plantuml

commonLabels:
  app: plantuml

resources:
  - github.com/beckje01/plantuml-k8s/manifests?ref=v0.1
  - ingress.yaml
```

ingress.yaml
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: plantuml
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
  - host: plantuml.lab.example
    http:
      paths:
      - backend:
          serviceName: plantuml-server
          servicePort: 8080
```

Build:
```
kustomize build ./ | kubectl apply -f -
```