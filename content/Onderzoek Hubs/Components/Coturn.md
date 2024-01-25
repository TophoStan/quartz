---
tags:
  - ce
  - hubs
---
# **Coturn Configuration in Kubernetes**

This Kubernetes YAML configuration deploys the Coturn application in the "hcce" namespace. Coturn is a crucial component of your infrastructure, and this configuration file provides the necessary details for its deployment.

### Deployment (coturn)

The Deployment resource for Coturn is defined in this section. Key deployment details include:

- Replicas set to 1 for a single instance.
- RollingUpdate strategy for updates.
- HostNetwork enabled for direct access to the host's network.
- Environment variables, including the `REALM` and `PSQL` settings.

#### Environment Variables

The Coturn Deployment relies on the following environment variables for its configuration:

- `REALM`: Specifies the realm for Coturn.
- `PSQL`: Accesses a secret "configs" to retrieve the PSQL configuration.

### Service (coturn)

This Service resource exposes the Coturn application within the Kubernetes cluster. It defines a port for secure access to Coturn.

### ConfigMaps

The configuration also includes two ConfigMaps, "haproxy-tcp-config" and "haproxy-config," which store configuration settings for the Coturn service.

### TLS Certificate

A Secret named "cert-hcce" is used to store the TLS certificate for secure communication with Coturn.

For additional information about Coturn, you can refer to the [Coturn documentation](https://example.com/coturn-docs).

If you have any questions or need further assistance, please feel free to reach out.

---

``` YAML
########################################################################
######################   coturn   ######################################
########################################################################
apiVersion: apps/v1
kind: Deployment
metadata:
  name: coturn
  namespace: hcce
spec:
  replicas: 1
  selector:
    matchLabels:
      app: coturn                                          
  minReadySeconds: 15
  strategy:
    type: RollingUpdate    
  template:
    metadata:
      labels:
        app: coturn
    spec:
      hostNetwork: true
      containers:
      - name: coturn
        image: mozillareality/coturn:stable-latest
        imagePullPolicy: Always
        ports:
        - hostPort: 5349
          containerPort: 5349
        env:
        - name: REALM
          value: turkey
        - name: PSQL
          valueFrom:
            secretKeyRef:
              name: configs
              key: PSQL
---
apiVersion: v1
kind: Service
metadata:
  name: coturn
  namespace: hcce
spec:
  ports:
  - name: https-coturn
    port: 5349
    targetPort: 5349
  selector:
    app: coturn
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: haproxy-tcp-config
  namespace: hcce
data:
  5349:
    hcce/coturn:5349
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: haproxy-config
  namespace: hcce
data:
  global-config-snippet: |
    tune.bufsize 33792
  backend-config-snippet: |
    option forwardfor 
    option http-pretend-keepalive
  ssl-redirect: "true"
  timeout-client: 30m
  timeout-client-fin: 1h
  timeout-server: 30m
  timeout-server-fin: 1h
  timeout-connect: 3s
  #access logging -- can be enabled at runtime
  syslog-server: 'address:stdout, format: raw, facility:daemon'
---
apiVersion: v1
kind: Secret
metadata:
  name: cert-hcce
  namespace: hcce
type: kubernetes.io/tls
data:
  tls.crt: 
  tls.key: 
---
```