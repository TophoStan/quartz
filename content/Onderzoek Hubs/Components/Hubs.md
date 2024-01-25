---
tags:
  - ce
  - hubs
---
# **Hubs Configuration in Kubernetes**

This Kubernetes YAML configuration deploys the Hubs application in the "hcce" namespace. Hubs is an essential component of your infrastructure, and this configuration file provides the necessary details for its deployment.

### Deployment (hubs)

The Deployment resource for Hubs is defined in this section. Key deployment details include:

- Replicas set to 1 for a single instance.
- RollingUpdate strategy with specific parameters for updates.
- Environment variables configured for Hubs with settings like `thumbnail_server`, `base_assets_path`, and others.
- Liveness probe configured to check the health of the Hubs application over HTTPS.

#### Environment Variables

The Hubs Deployment relies on the following environment variables for its configuration:

- `turkeyCfg_thumbnail_server`: Sets the thumbnail server URL.
- `turkeyCfg_base_assets_path`: Defines the base path for assets.
- `turkeyCfg_non_cors_proxy_domains`: Lists non-CORS proxy domains.
- `turkeyCfg_reticulum_server`: Specifies the Reticulum server URL.
- `turkeyCfg_cors_proxy_server`: Sets the CORS proxy server.
- `turkeyCfg_shortlink_domain`: Defines the shortlink domain.
- `turkeyCfg_tier`: Specifies the tier (e.g., "p1").

### Service (hubs)

This Service resource exposes the Hubs application within the Kubernetes cluster. It defines a port for HTTPS access to Hubs and annotations for SSL configuration. The Service selects pods labeled with "app: hubs."

Make sure to adjust the environment variable values to match your specific configuration requirements.

For additional information about Hubs, you can refer to the [Hubs documentation](https://example.com/hubs-docs).

If you have any questions or need further assistance, please feel free to reach out.

---
``` YAML
########################################################################
######################   hubs   ########################################
########################################################################
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hubs
  namespace: hcce
  annotations:
    cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hubs
  minReadySeconds: 15
  strategy:
    type: RollingUpdate
    rollingUpdate: 
      maxUnavailable: 0
      maxSurge: 1
  template:
    metadata:
      labels:
        app: hubs
    spec:
      containers:
      - name: hubs
        image: mozillareality/hubs:stable-latest
        imagePullPolicy: IfNotPresent
        env:
        - name: turkeyCfg_thumbnail_server
          value: nearspark.reticulum.io
        - name: turkeyCfg_base_assets_path
          value: https://assets.mouseparts.eu/hubs/
        - name: turkeyCfg_non_cors_proxy_domains
          value: "mouseparts.eu,assets.mouseparts.eu"
        - name: turkeyCfg_reticulum_server
          value: mouseparts.eu
        - name: turkeyCfg_cors_proxy_server
          value: cors.mouseparts.eu
        - name: turkeyCfg_shortlink_domain
          value: mouseparts.eu
        - name: turkeyCfg_tier
          value: p1
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
  name: hubs
  namespace: hcce
  annotations:
    haproxy.org/server-ssl: "true"
spec:
  clusterIP: None
  ports:
  - name: https-hubs
    port: 8080
    targetPort: 8080
  selector:
    app: hubs
---
```