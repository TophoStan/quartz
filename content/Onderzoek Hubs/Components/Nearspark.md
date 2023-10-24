
#hubs
# **Nearspark Configuration in Kubernetes**

This Kubernetes YAML configuration deploys the "nearspark" application as a Deployment and Service in the "hcce" namespace. "nearspark" is used for the "nearspark" service.

### Deployment (nearspark)

A Deployment named "nearspark" is created with the following details:

- Replicas: 1
- Labels are set to "app: nearspark."
- Minimum Ready Seconds: 15
- The image used for the Deployment is "mozillareality/nearspark:stable-latest."
- Strategy:
  - Type: RollingUpdate
  - Rolling Update Configuration:
    - Max Unavailable: 0
    - Max Surge: 1

### Service (nearspark)

A Service named "nearspark" is defined with the following configuration:

- Ports:
  - Name: http
  - Port: 5000
  - TargetPort: 5000
- The Service selects Pods with the label "app: nearspark."

This configuration is used to deploy and expose the "nearspark" application.

If you have any questions or need further assistance, please feel free to reach out.

---
```YAML
########################################################################
######################   nearspark   ###################################
########################################################################
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nearspark
  namespace: hcce
  annotations:
    cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
spec:
  replicas: 1 
  selector:
    matchLabels:
      app: nearspark
  minReadySeconds: 15
  strategy:
    type: RollingUpdate
    rollingUpdate: 
      maxUnavailable: 0
      maxSurge: 1
  template:
    metadata:
      labels:
        app: nearspark
    spec:
      containers:
      - name: nearspark
        image: mozillareality/nearspark:stable-latest
        ports:
        - containerPort: 5000
        imagePullPolicy: IfNotPresent
---
apiVersion: v1
kind: Service
metadata:
  name: nearspark
  namespace: hcce
spec:
  ports:
  - name: http
    port: 5000
    targetPort: 5000
  selector:
    app: nearspark        
```
