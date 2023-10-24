#hubs
# **Ingress Configuration in Kubernetes**

This Kubernetes YAML configuration defines two Ingress resources in the "hcce" namespace for routing external traffic to the respective services.

### Ingress (ret)

The Ingress named "ret" is created with the following configuration:

- Name: ret
- Namespace: hcce
- Annotations:
  - kubernetes.io/ingress.class: haproxy
  - haproxy.org/response-set-header: (Header configurations)
  - haproxy.org/path-rewrite: (Path rewrite configuration)
- TLS Configuration for multiple hosts with secret names.
- Rules and path routing for various host and path combinations.

### Ingress (dialog)

The Ingress named "dialog" is configured as follows:

- Name: dialog
- Namespace: hcce
- Annotations:
  - kubernetes.io/ingress.class: haproxy
  - haproxy.org/server-ssl: "true"
  - haproxy.org/load-balance: "url_param roomId"
- TLS Configuration for a specific host.
- Routing rules and path configuration.

These Ingress configurations set up routing and SSL termination for external access to the defined services in the "hcce" namespace.

If you have any questions or need further assistance, please feel free to reach out.

---
```YAML
######################################################################################
###################################### ingress #######################################
######################################################################################
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ret
  namespace: hcce
  annotations:
    kubernetes.io/ingress.class: haproxy
    haproxy.org/response-set-header: |
      access-control-allow-origin "https://mouseparts.eu"
    haproxy.org/path-rewrite: /api-internal(.*) /_drop_
spec:
  tls:
  - hosts:
      - mouseparts.eu
    secretName: cert-mouseparts.eu
  - hosts:
      - assets.mouseparts.eu
    secretName: cert-assets.mouseparts.eu
  - hosts:
      - stream.mouseparts.eu
    secretName: cert-stream.mouseparts.eu
  - hosts:
      - cors.mouseparts.eu
    secretName: cert-cors.mouseparts.eu
  rules:
  - host: mouseparts.eu
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: ret
            port: 
              number: 4001
  - host: assets.mouseparts.eu
    http:
      paths:
      - path: /files/
        pathType: Prefix
        backend:
          service:
            name: ret
            port: 
              number: 4001
      - path: /http
        pathType: ImplementationSpecific  # haproxy's "Begin with"
        backend:
          service:
            name: ret
            port: 
              number: 4001
      - path: /hubs
        pathType: Prefix
        backend:
          service:
            name: hubs
            port: 
              number: 8080
      - path: /spoke
        pathType: Prefix
        backend:
          service:
            name: spoke
            port: 
              number: 8080
  - host: cors.mouseparts.eu
    http:
      paths:
      - path: /files/
        pathType: Prefix
        backend:
          service:
            name: ret
            port: 
              number: 4000
      - path: /http
        pathType: ImplementationSpecific
        backend:
          service:
            name: ret
            port: 
              number: 4000                  
      - path: /hubs
        pathType: Prefix
        backend:
          service:
            name: hubs
            port: 
              number: 8080
      - path: /spoke
        pathType: Prefix
        backend:
          service:
            name: spoke
            port: 
              number: 8080
      - path: /nearspark
        pathType: ImplementationSpecific
        backend:
          service:
            name: nearspark
            port: 
              number: 5000   
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: dialog
  namespace: hcce
  annotations:
    kubernetes.io/ingress.class: haproxy
    haproxy.org/server-ssl: "true"
    haproxy.org/load-balance: "url_param roomId"
spec:
  tls:
  - hosts:
      - stream.mouseparts.eu
    secretName: cert-stream.mouseparts.eu
  rules:
  - host: stream.mouseparts.eu
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: dialog
            port: 
              number: 4443          
---
```