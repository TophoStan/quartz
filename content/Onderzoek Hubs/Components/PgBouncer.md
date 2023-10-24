#hubs
# **pgbouncer Configuration in Kubernetes**

This Kubernetes YAML configuration deploys "pgbouncer" as a Deployment and Service in the "hcce" namespace. "pgbouncer" is a connection pooler for PostgreSQL databases.

### Deployment (pgbouncer)

A Deployment named "pgbouncer" is created with the following details:

- Replicas: 1
- Labels are set to "app: pgbouncer."
- Minimum Ready Seconds: 2
- The image used for the Deployment is "mozillareality/pgbouncer:stable-latest."
- Environment Variables:
  - MAX_CLIENT_CONN: "10000"
  - DB_USER: (Fetched from the "DB_USER" secret key)
  - DB_PASSWORD: (Fetched from the "DB_PASS" secret key)
  - DB_HOST: "pgsql"

### Service (pgbouncer)

A Service named "pgbouncer" is defined with the following configuration:

- Ports:
  - Name: http
  - Port: 5432
  - TargetPort: 5432
- The Service selects Pods with the label "app: pgbouncer."

### Deployment (pgbouncer-t)

Another Deployment named "pgbouncer-t" is created with similar configuration as "pgbouncer," but with a different label ("app: pgbouncer-t"). This Deployment is used for "transaction" pool mode.

### Service (pgbouncer-t)

A Service named "pgbouncer-t" is defined with the same configuration as the "pgbouncer" Service.

This configuration is used to deploy and expose "pgbouncer" and "pgbouncer-t" for PostgreSQL connection pooling.

If you have any questions or need further assistance, please feel free to reach out.

---
```YAML
####################################################################################
################################### pgbouncer ######################################
####################################################################################
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pgbouncer
  namespace: hcce
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pgbouncer
  minReadySeconds: 2
  template:
    metadata:
      labels:
        app: pgbouncer
    spec:
      containers:
      - image: mozillareality/pgbouncer:stable-latest
        imagePullPolicy: IfNotPresent
        name: pgbouncer
        env:
        - name: MAX_CLIENT_CONN
          value: "10000"
        - name: DB_USER
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_USER
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_PASS
        - name: DB_HOST
          value: pgsql
---
apiVersion: v1
kind: Service
metadata:
  name: pgbouncer
  namespace: hcce
spec:
  ports:
  - name: http
    port: 5432
    targetPort: 5432
  selector:
    app: pgbouncer
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pgbouncer-t
  namespace: hcce
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pgbouncer-t
  minReadySeconds: 2
  template:
    metadata:
      labels:
        app: pgbouncer-t
    spec:
      containers:
      - image: mozillareality/pgbouncer:stable-latest
        imagePullPolicy: IfNotPresent
        name: pgbouncer-t
        env:
        - name: MAX_CLIENT_CONN
          value: "10000"
        - name: DB_USER
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_USER          
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_PASS
        - name: DB_HOST
          value: pgsql
        - name: POOL_MODE
          value: transaction
---
apiVersion: v1
kind: Service
metadata:
  name: pgbouncer-t
  namespace: hcce
spec:
  ports:
  - name: http
    port: 5432
    targetPort: 5432
  selector:
    app: pgbouncer-t
---
```