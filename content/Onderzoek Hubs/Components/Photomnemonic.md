---
tags:
  - ce
  - hubs
---
# **Photomnemonic Configuration in Kubernetes**

This Kubernetes YAML configuration deploys the "photomnemonic" application as a Deployment and Service in the "hcce" namespace. "photomnemonic" is used for handling photo mnemonic services.

#### Deployment (photomnemonic)

A Deployment named "photomnemonic" is created with the following details:

- Replicas: 1
- Labels are set to "app: photomnemonic."
- Minimum Ready Seconds: 2
- The image used for the Deployment is "mozillareality/photomnemonic:stable-latest."

#### Service (photomnemonic)

A Service named "photomnemonic" is defined with the following configuration:

- Ports:
  - Name: photomnemonic
  - Port: 5000
  - TargetPort: 5000
- The Service selects Pods with the label "app: photomnemonic."

#### Secret (configs)

A Secret named "configs" is created in the "hcce" namespace. It contains a PostgreSQL connection string as a key-value pair:

- Key: PSQL
- Value: postgres://postgres:123456@pgbouncer/retdb

This connection string is used to connect to the PostgreSQL database.

If you have any questions or need further assistance, please feel free to reach out.

---
```YAML
########################################################################
######################   photomnemonic   ###############################
########################################################################
apiVersion: apps/v1
kind: Deployment
metadata:
  name: photomnemonic
  namespace: hcce
  annotations:
    cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
spec:
  replicas: 1
  selector:
    matchLabels:
      app: photomnemonic
  minReadySeconds: 2
  template:
    metadata:
      labels:
        app: photomnemonic
    spec:
      containers:
      - name: photomnemonic
        image: mozillareality/photomnemonic:stable-latest
        imagePullPolicy: IfNotPresent
---
apiVersion: v1
kind: Service
metadata:
  name: photomnemonic
  namespace: hcce
spec:
  ports:
  - name: photomnemonic
    port: 5000
    targetPort: 5000
  selector:
    app: photomnemonic
---
apiVersion: v1
kind: Secret
metadata:
  name: configs
  namespace: hcce
stringData:
  PSQL: postgres://postgres:123456@pgbouncer/retdb
---
```