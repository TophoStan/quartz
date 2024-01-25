---
tags:
  - kubernetes
  - ce
---
**Wat is Kubernetes**
Kubernetes(K8S) is een open-source orkestratie platform dat is ontwikkeld door Google, maar wordt nu onderhouden door Cloud Native Computing Foundation. Kubernetes is een tool waarmee je deployment kan automatiseren, schalen kan automatiseren en ook het managen van containerized applicaties, zoals Docker. Het is een technologie die door veel cloud providers wordt ondersteund als Azure, Amazon en Google. Dit zorgt ervoor dat het aantrekkelijk is om hierin te investeren door de flexibiliteit van platform die het aanbied aan de ontwikkelaar.

**Cluster**
Kubernetes werkt met clusters, elk cluster **Kan** bestaan uit 1 of meerdere pods. In een Pod zitten vaak 1 of meerdere Docker containers. 

**Pod**
Kubernetes maakt gebruik van zogeheten pods. Bij een Pod kan je denken aan een Docker container. In een Pod draaien naast jou eigen container, containers van Kubernetes zelf voor zaken als DNS. (*Tip: het is een best-practice om per pod 1 container te draaien*) Om zo een Pod aan te sturen zal je de Docker container moeten uploaden naar een *Container registry*, zoals DockerHub, AzureCR. Op het moment dat je de Container wilt gaan draaien moet je het volgende weten. Voor het draaien heb je een *Deployment* en een *Service* nodig. Een deployment is je Docker image bijvoorbeeld. Een Service is een abstractie op je deployment die ervoor zorgt dat je applicatie kan communiceren binnen en buiten je applicatie.

Services en Deployments worden gedefinieerd in een YAML bestand. Hieronder staan voorbeelden hiervan. Het is vergelijkbaar met een Docker Compose bestand. Er wordt in gedefinieerd wat de naam is, maar ook onderwerpen zoals replicatie aantal en techniek kunnen aangegeven worden. Er zijn een hoop default waarden waar je geen rekening mee hoeft te houden, maar als je iets expliciet wilt aangeven kan dat dus ook.
``` YAML
# reticulum-deployment
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: reticulum
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: reticulum
    template:
      metadata:
        labels:
          app: reticulum
      spec:
        containers:
        - name: reticulum
          image: acrhubs.azurecr.io/local-reticulum
          ports:
          - containerPort: 4000
          env:
            - name: HUBS_CLIENT_INTERNAL_HOSTNAME
              value: '20.76.130.207'
            - name: HUBS_SERVICE_NAME
              value: '20.76.130.207'
            - name: DIALOG_HOSTNAME
              value: '20.76.130.207'
            - name: DIALOG_PORT
              value: "4443"
            - name: DIALOG_PORT23
              value: "4443"
---
# reticulum-service
  apiVersion: v1
  kind: Service
  metadata:
    name: reticulum
  spec:
    type: ClusterIP
    ports:
      - port: 4000
        targetPort: 4000  
    selector:
      app: reticulum
```


**Ingress**
Ingress is een service die je kan aanmaken om routes te exposen naar buiten het cluster netwerk. Het lost het probleem op dat je voor elke deployment een Load Balancer aanmaakt. Een Load balancer is tevens een dure resource, omdat het een publiek adres vereist en ook veel andere resources kost. 
Een implementatie van een Ingress service met nginx is mogelijk en ook het meest populair. Hier een snippet.
``` YAML
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: nginx
  rules:
   - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld-one
            port:
              number: 80
      - path: /hello
        pathType: Prefix
        backend:
          service:
            name: aks-helloworld-two
            port:
              number: 80
```
Nginx exposed alleen de ports 80 (HTTP) en 443(HTTPS) standaard. Om andere poorten naar het publieke adres te exposen moet je een ConfigMap aanmaken en deze toepassen op de Ingress controller. 


Hier een mooie schets van een Microsoft meneer van een overzicht van een applicatie. 
![[Pasted image 20231003102852.png]]



**Voorbeelden van commando's die je kan gebruiken**
`az` is de prefix van de Azure CLI
`kubectl` is de prefix van de Kubernetes CLI

Creeeren van een Azure Kubernetes Cluster
``` shell
az aks create 
    --resource-group myResourceGroup 
    --name myAKSCluster 
    --node-count 2 
    --generate-ssh-keys 
    --attach-acr acrhubs
```
Verbind met een cluster
``` shell
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

**Commando's die veel worden gebruikt**
``` shell
kubectl get pod # shows all pods 
```
![[Pasted image 20231009105437.png]]
``` shell
kubectl logs -f reticulum-coole-reeks # logt dus alles van Reticulum
```
``` shell
kubectl apply -f C:\dev\MozillaHubs\LocalEnv\hubs\azure.yaml # past YAML bestand toe op verbonden cluster
```



### **Lens**
Lens is een GUI voor Kubernetes Clusters. Super handig, alleen is er nog niet veel gebruik van gemaakt door mij en kost een zakelijke abonnement geld. Als je de Kubectl CLI al in hebt gesteld met je online cluster op bijvboorbeld Azure, kan je gelijk aan de gang gaan.