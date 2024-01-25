Je kan deze tabel opnieuw genereren met het `generateMDTable.js` bestand.

| Endpoint | Tag | HTTP Verb | Endpoint Summary |
|----------|-----|-----------|------------------|
| /api/features | Features | <span style="color: white; background-color: green;">GET</span> | Get all features |
| /api/google/auth | Google | <span style="color: white; background-color: blue;">POST</span> | Authenticate the user |
| /api/google | Google | <span style="color: white; background-color: blue;">POST</span> | Authenticate and get authorization URL |
| /api/kubernetes/clusters/{cluster_id} | Namespaces | <span style="color: white; background-color: blue;">POST</span> | Get details of a Kubernetes cluster |
| /api/kubernetes/clusters/{cluster_id} | Namespaces | <span style="color: white; background-color: orange;">PUT</span> | Create a namespace in a Kubernetes cluster |
| /api/kubernetes/clusters | Clusters | <span style="color: white; background-color: blue;">POST</span> | List of Kubernetes clusters |
| /api/kubernetes/clusters | Clusters | <span style="color: white; background-color: orange;">PUT</span> | Create a Kubernetes cluster |
| /api/kubernetes/deployment/[namespace] | Deployments | <span style="color: white; background-color: blue;">POST</span> | Retrieves the detailed information of a running cluster. |
| /api/kubernetes/deployments | Deployments | <span style="color: white; background-color: blue;">POST</span> | Create a new deployment in a Kubernetes cluster. |
| /api/kubernetes/deployments | Deployments | <span style="color: white; background-color: orange;">PUT</span> | Update the hubs client deployment in a Kubernetes cluster. |
| /api/modules/clone | Modules | <span style="color: white; background-color: blue;">POST</span> | Builds a Docker image and returns its name. |
| /api/modules | Modules | <span style="color: white; background-color: blue;">POST</span> | Retrieves all the files from a specific Google Drive folder. |
| /api/modules/validate | Modules | <span style="color: white; background-color: blue;">POST</span> | Checks for modules and their versions in Google Drive. |
