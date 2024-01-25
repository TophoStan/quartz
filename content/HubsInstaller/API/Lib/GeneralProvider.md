
1. **Imports:**
    
    - Importeert verschillende modules en klassen, waaronder Kubernetes API's zoals `AppsV1Api`, `CoreV1Api`, `NetworkingV1Api`, `RbacAuthorizationV1Api`, en andere hulpmiddelen zoals `js-yaml`, `fs`, en aangepaste interfaces.
2. **Constructor:**
    
    - Initialiseert de klasse met verschillende Kubernetes API's (AppsV1Api, CoreV1Api, NetworkingV1Api, RbacAuthorizationV1Api).
3. **Methoden:**
    
    - **createNamespace:** Maakt een nieuw Kubernetes namespace aan.
        
    - **createDeployment:** Creëert een nieuw Kubernetes deployment in een specifieke namespace.
        
    - **createKubernetesFromYaml:** Creëert Kubernetes-resources op basis van YAML-bestanden, zoals deployments, services, configmaps, secrets, ingress, en serviceaccounts.
        
    - **replaceDeployment:** Vervangt een bestaande deployment in een namespace, waarbij de features worden bijgewerkt.
        
    - **checkRoleBindings:** Controleert of de rolbindings al bestaan; zo niet, worden ze gemaakt.
        
    - **listNamespaces:** Lijst alle namespaces op in een cluster, met uitsluiting van de standaard namespaces.
        
    - **createNewHubsStack:** Creëert een nieuwe Hubs-stack op basis van YAML-sjablonen, met specifieke opties en features. Deze methode maakt ook gebruik van de `fillVariablesInYamlTemplate` functie die in [[yamlTemplateFunctions]] verder wordt besproken.
		
		
        
    - **applyHubsClientDeployment:** Past een Hubs-clientdeploymentscript toe op een bestaande namespace.
        
    - **getLoadBalancerService:** Haalt de LoadBalancer-service op voor een specifieke namespace.
        
    - **isPodTerminated:** Controleert of een pod is beëindigd in een namespace.
        
    - **addSSLToDomain:** Voegt SSL-certificaten toe aan specifieke domeinen in een namespace.
        
    - **useCertbot:** Gebruikt Certbot om SSL-certificaten te verkrijgen en toe te passen op domeinen in een namespace.
        
    - **getDeploymentDetails:** Haalt details op van een implementatie in een specifieke namespace, inclusief features, domein, IP en configuratie-opties.
        
    - **getImageFromDeployment:** Haalt de image-informatie op van een specifieke deployment in een namespace.
        
4. **Variabelen:**
    
    - Private variabelen, zoals `kubernetesAppsApi`, `kubernetesCoreApi`, `kubernetesNetworkingApi`, en `kubernetesRbacApi`.