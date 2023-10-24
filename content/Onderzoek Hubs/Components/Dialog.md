#hubs
# **Dialog Configuration in Kubernetes**

This Kubernetes YAML configuration deploys the Dialog application in the "hcce" namespace. Dialog is a critical component of your infrastructure, and this configuration file provides essential details for its deployment.

### Deployment (dialog)

The Deployment resource for Dialog is defined in this section. Key deployment details include:

- Replicas set to 1 for a single instance.
- RollingUpdate strategy with specific parameters for updates.
- HostNetwork enabled for direct access to the host's network.
- Environment variable configured for `perms_key` sourced from a secret for security.

### Service (dialog)

This Service resource exposes the Dialog application within the Kubernetes cluster. It defines ports for secure access to Dialog, including ports 4443 and 7000.

For additional information about Dialog, you can refer to the [Dialog documentation](https://example.com/dialog-docs).

If you have any questions or need further assistance, please feel free to reach out.

---
``` YAML
########################################################################
######################   dialog   ######################################
########################################################################  
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dialog
  namespace: hcce
  annotations:
    cluster-autoscaler.kubernetes.io/safe-to-evict: "false"
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dialog
  minReadySeconds: 15
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: dialog
    spec:
      hostNetwork: true
      containers:
      - name: dialog
        image: mozillareality/dialog:stable-latest
        imagePullPolicy: Always
        ports:        
        - hostPort: 4443
          containerPort: 4443
        env:
        - name: perms_key
          valueFrom:
            secretKeyRef:
              name: configs
              key: PERMS_KEY
---
apiVersion: v1
kind: Service
metadata:
  name: dialog
  namespace: hcce
spec:
  clusterIP: None
  ports:
  - name: https-dialog
    port: 4443
    targetPort: 4443
  - name: https-dialog-adm
    port: 7000
    targetPort: 7000
  selector:
    app: dialog
---
```