---
tags:
  - ce
  - hubs
---
# **Haproxy Configuration in Kubernetes**

This Kubernetes YAML configuration deploys the HAProxy application in the "hcce" namespace. HAProxy is a critical component of your infrastructure, and this configuration file provides essential details for its deployment.

#### Deployment (haproxy)

The Deployment resource for HAProxy is defined in this section. Key deployment details include:

- Replicas set to 1 for a single instance.
- Configuration options for HAProxy, including ports and settings.
- SecurityContext to ensure safe operation.
- Environment variables for configuration and runtime details.

##### Environment Variables

The HAProxy Deployment relies on the following environment variables for its configuration:

- `TZ`: Specifies the time zone.
- `POD_NAME`: Automatically retrieves the pod name.
- `POD_NAMESPACE`: Automatically retrieves the pod namespace.

#### Service (lb)

This Service resource defines a LoadBalancer for HAProxy. It allows external traffic with local traffic policy and maps ports to HAProxy services.

#### ServiceAccount (haproxy-sa)

A ServiceAccount named "haproxy-sa" is created for the HAProxy Deployment.

#### ClusterRole and ClusterRoleBinding

The ClusterRole "haproxy-cr" and ClusterRoleBinding "haproxy-rb" grant necessary permissions to the HAProxy ServiceAccount.

For additional information about HAProxy, you can refer to the [HAProxy documentation](https://example.com/haproxy-docs).

If you have any questions or need further assistance, please feel free to reach out.

---
``` YAML
######################################################################################
###################################### haproxy #######################################
######################################################################################
kind: Deployment
apiVersion: apps/v1
metadata:
  name: haproxy
  namespace: hcce
  labels:
    app: haproxy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: haproxy
  template:
    metadata:
      labels:
        app: haproxy
        name: haproxy
    spec:
      # hostNetwork: true
      serviceAccountName: haproxy-sa
      terminationGracePeriodSeconds: 60
      containers:
      - name: haproxy
        image: haproxytech/kubernetes-ingress:1.8.5@sha256:09b59bc272e3aec5ca5b706774ed788c4bb4f184bb1d7ab99660a2b7773b0668
        args:
        - --configmap=hcce/haproxy-config
        - --https-bind-port=4443
        - --http-bind-port=8080
        - --configmap-tcp-services=hcce/haproxy-tcp-config
        - --ingress.class=haproxy
        - --log=warning #error warning info debug trace
        - --default-ssl-certificate=hcce/cert-mouseparts.eu
        securityContext:
          runAsUser:  1000
          runAsGroup: 1000
          capabilities:
            drop:
              - ALL
            add:
              - NET_BIND_SERVICE
        livenessProbe:
          httpGet:
            path: /healthz
            port: 1042
        env:
        - name: TZ
          value: "Etc/UTC"
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
---
apiVersion: v1
kind: Service
metadata:
  name: lb
  namespace: hcce
spec:
  type: LoadBalancer
  externalTrafficPolicy: Local
  selector:
    app: haproxy
  ports:
  - name: http
    port: 80
    targetPort: 8080
  - name: https
    port: 443
    targetPort: 4443
  - name: dialog
    port: 4443
    targetPort: 4443
  - name: turn
    port: 5349
    targetPort: 5349
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: haproxy-sa
  namespace: hcce
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: haproxy-cr
rules:
  - apiGroups:
    - ""
    resources:
    - configmaps
    - nodes
    - pods
    - namespaces
    - events
    - serviceaccounts
    - services
    - endpoints    
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - "extensions"
    - "networking.k8s.io"
    resources:
    - ingresses
    - ingresses/status
    - ingressclasses
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - "extensions"
    - "networking.k8s.io"
    resources:
    - ingresses/status
    verbs:
    - update
  - apiGroups:
    - ""
    resources:
    - secrets
    verbs:
    - get
    - list
    - watch
    - create
    - patch
    - update
  - apiGroups:
    - core.haproxy.org
    resources:
    - '*'
    verbs:
    - get
    - list
    - watch
    - update
  - apiGroups:
    - "discovery.k8s.io"
    resources:
    - '*'
    verbs:
    - get
    - list
    - watch      
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: haproxy-rb
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: haproxy-cr
subjects:
- kind: ServiceAccount
  name: haproxy-sa
  namespace: hcce 

```