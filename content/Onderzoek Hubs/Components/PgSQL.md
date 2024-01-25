---
tags:
  - ce
  - hubs
---
# **PgSQL Configuration in Kubernetes**

This Kubernetes YAML configuration deploys a PostgreSQL database as a Deployment and a Service in the "hcce" namespace.

### Service (pgsql)

A Service named "pgsql" is created with the following configuration:

- Name: pgsql
- Namespace: hcce
- Selector: Selects Pods with the label "app: pgsql."
- Ports:
  - Name: postgresql
  - Protocol: TCP
  - Port: 5432
  - TargetPort: 5432

### Deployment (pgsql)

A Deployment named "pgsql" is defined with the following details:

- Replicas: 1
- Labels are set to "app: pgsql."
- The image used for the PostgreSQL container is "postgres:12."
- Ports:
  - Name: postgresql
  - ContainerPort: 5432
- Environment Variables:
  - POSTGRES_USER: (Fetched from the "DB_USER" secret key)
  - POSTGRES_PASSWORD: (Fetched from the "DB_PASS" secret key)
  - POSTGRES_DB: (Fetched from the "DB_NAME" secret key)
- Volume Mounts:
  - Mounts the PostgreSQL data directory from the host at "/var/lib/postgresql/data."

#### Volumes

- A volume named "postgresql-data" is used to mount the PostgreSQL data directory.
- The volume is hosted at "/tmp/pgsql_data" on the host.

This configuration sets up a PostgreSQL database with the specified environment variables and volume mounts for data storage.

If you have any questions or need further assistance, please feel free to reach out.

---
```YAML
########################################################################
######################   pgsql   #######################################
########################################################################
apiVersion: v1
kind: Service
metadata:
  name: pgsql
  namespace: hcce
spec:
  selector:
    app: pgsql
  ports:
    - name: postgresql
      protocol: TCP
      port: 5432
      targetPort: 5432
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pgsql
  namespace: hcce
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pgsql
  template:
    metadata:
      labels:
        app: pgsql
    spec:
      containers:
        - name: postgresql
          image: postgres:12
          ports:
            - name: postgresql
              containerPort: 5432
          env:
            - name: POSTGRES_USER
              valueFrom:
                secretKeyRef:
                  name: configs
                  key: DB_USER
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: configs
                  key: DB_PASS
            - name: POSTGRES_DB
              valueFrom:
                secretKeyRef:
                  name: configs
                  key: DB_NAME
          volumeMounts:
            - name: postgresql-data
              mountPath: /var/lib/postgresql/data
      volumes:
        - name: postgresql-data
          hostPath:
            path: /tmp/pgsql_data
---
```