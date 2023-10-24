#hubs
### **cbb rbac**

This Kubernetes YAML configuration defines Role-Based Access Control (RBAC) settings for the "cbb-sa" ServiceAccount in the "hcce" namespace.

#### ServiceAccount (cbb-sa)

A ServiceAccount named "cbb-sa" is created in the "hcce" namespace for use in RBAC configurations.

#### RoleBinding (cbb-rb--hcce)

A RoleBinding named "cbb-rb--hcce" is established in the "hcce" namespace, linking the "cbb-sa" ServiceAccount to the "edit" ClusterRole. This allows the ServiceAccount to edit resources in the namespace.

### **certbotbot**

This Kubernetes YAML configuration deploys the Certbotbot application as a Pod in the "hcce" namespace. Certbotbot is used for managing SSL certificates automatically.

#### Pod (certbotbot-http)

The Pod named "certbotbot-http" is configured with the following details:

- Labels are set to "app: certbotbot-http."
- The Pod restart policy is set to "Never."
- The "cbb-sa" ServiceAccount is associated with the Pod.
- The image used for the Pod is "mozillareality/certbotbot_http:dev-47."

#### Environment Variables

The Certbotbot Pod relies on environment variables for its configuration, including:

- `DOMAIN`: Specifies the domain to manage SSL certificates for.
- `NAMESPACE`: Specifies the namespace for certificate management.
- `CERT_NAME`: Specifies the name of the certificate to manage.

For additional information about Certbotbot, you can refer to the [Certbotbot documentation](https://example.com/certbotbot-docs).

If you have any questions or need further assistance, please feel free to reach out.

---
```YAML
########################################################################
######################   cbb rbac  #####################################
########################################################################
apiVersion: v1
kind: ServiceAccount
metadata:
  name: cbb-sa
  namespace: hcce
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: cbb-rb--hcce
  namespace: hcce
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: edit
subjects:
- kind: ServiceAccount
  name: cbb-sa
  namespace: hcce
---
########################################################################
#########################   certbotbot   ###############################
########################################################################
apiVersion: v1
kind: Pod
metadata:
  name: certbotbot-http
  namespace: hcce
  labels:
    app: certbotbot-http
spec:
  restartPolicy: Never
  serviceAccountName: cbb-sa
  containers:
  - name: myapp
    image: mozillareality/certbotbot_http:dev-47
    env:
    - name: "DOMAIN"
      value: "cors.mouseparts.eu"
    - name: "NAMESPACE"
      value: "hcce"
    - name: "CERT_NAME"
      value: "cert-cors.mouseparts.eu"
```