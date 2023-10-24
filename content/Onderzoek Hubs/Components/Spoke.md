#hubs
# **Spoke Configuration in Kubernetes** 

This Kubernetes YAML configuration deploys the Spoke application in the "hcce" namespace. Spoke is an integral component of your infrastructure, and this configuration file provides essential details for its deployment.

### Deployment (spoke)

The Deployment resource for Spoke is defined in this section. Key deployment details include:

- Replicas set to 1 for a single instance.
- RollingUpdate strategy with specific parameters for updates.
- Environment variables configured for Spoke with settings like `thumbnail_server`, `base_assets_path`, and others.
- Liveness probe configured to check the health of the Spoke application over HTTPS.

#### Environment Variables

The Spoke Deployment relies on the following environment variables for its configuration:

- `turkeyCfg_thumbnail_server`: Sets the thumbnail server URL.
- `turkeyCfg_base_assets_path`: Defines the base path for assets.
- `turkeyCfg_non_cors_proxy_domains`: Lists non-CORS proxy domains.
- `turkeyCfg_reticulum_server`: Specifies the Reticulum server URL.
- `turkeyCfg_cors_proxy_server`: Sets the CORS proxy server.
- `turkeyCfg_shortlink_domain`: Defines the shortlink domain.
- `turkeyCfg_hubs_server`: Specifies the Hubs server URL.

### Service (spoke)

This Service resource exposes the Spoke application within the Kubernetes cluster. It defines a port for HTTPS access to Spoke and annotations for SSL configuration. The Service selects pods labeled with "app: spoke."

For additional information about Spoke, you can refer to the [Spoke documentation](https://example.com/spoke-docs).

If you have any questions or need further assistance, please feel free to reach out.

---
``` YAML
########################################################################
######################   spoke   ########################################
########################################################################
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spoke
  namespace: hcce
  annotations:
    cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
spec:
  replicas: 1 
  selector:
    matchLabels:
      app: spoke
  minReadySeconds: 15
  strategy:
    type: RollingUpdate
    rollingUpdate: 
      maxUnavailable: 0
      maxSurge: 1
  template:
    metadata:
      labels:
        app: spoke
    spec:
      containers:
      - name: spoke
        image: mozillareality/spoke:stable-latest
        imagePullPolicy: IfNotPresent
        env:
        - name: turkeyCfg_thumbnail_server
          value: nearspark.reticulum.io
        - name: turkeyCfg_base_assets_path
          value: https://assets.mouseparts.eu/spoke/
        - name: turkeyCfg_non_cors_proxy_domains
          value: "mouseparts.eu,assets.mouseparts.eu"
        - name: turkeyCfg_reticulum_server
          value: mouseparts.eu
        - name: turkeyCfg_cors_proxy_server
          value: cors.mouseparts.eu
        - name: turkeyCfg_shortlink_domain
          value: mouseparts.eu
        - name: turkeyCfg_hubs_server
          value: mouseparts.eu
        livenessProbe:
          httpGet:
            path: https://localhost/healthz
            port: 8080
            scheme: HTTPS
          initialDelaySeconds: 20
          timeoutSeconds: 1
          periodSeconds: 120
---
apiVersion: v1
kind: Service
metadata:
  name: spoke
  namespace: hcce
  annotations:
    haproxy.org/server-ssl: "true"
spec:
  clusterIP: None
  ports:
  - name: https-spoke
    port: 8080
    targetPort: 8080
  selector:
    app: spoke
---
```