---
tags:
  - google
---




Google Cloud Platform, het platform van Google waarop de "cloud" zich bevind. 

Googles structuur gaat als volgt:

Project
	Google Kubernetes Engine
		Cluster
			Namespaces
				Pods

De Google Kubernetes Engine (GKE) is een managed Kubernetes instantie van Google net als AKS van Azure.

## Deployen van je kubernetes
1. Download de Google Cloud CLI en gcloud-auth-plugin component
2.   Aanmaken van een cluster: 
``` bash
gcloud container clusters create boldlyCluster --zone europe-west4-a --node-locations europe-west4-a
```
3. Verbinden met je cluster
``` bash
gcloud container clusters get-credentials autopilot-cluster-1 --zone europe-west4
```
4. Test je verbinding
``` bash
kubectl get namespaces
```
![[Pasted image 20231030101755.png]]