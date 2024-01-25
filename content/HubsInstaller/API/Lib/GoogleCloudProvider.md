
### **Methoden**
1. **`getDeploymentDetails(body: any): Promise<RunningClusterDetails>`**
    
    - Haalt details op over de implementatie van een cluster.
    - Parameters:
        - `body`: Bevat informatie over de service-account, cluster-ID, en namespace.
    - Retourneert een belofte (Promise) van `RunningClusterDetails`.
2. **`addSSLToDomain({ domain, namespace, service_account, email, clusterId }): Promise<void>`**
    
    - Voegt SSL toe aan een domein in een opgegeven namespace en cluster.
    - Parameters:
        - `domain`: Het domein waar SSL aan wordt toegevoegd.
        - `namespace`: De Kubernetes-namespace.
        - `service_account`: Informatie over het service-account.
        - `email`: Het e-mailadres voor SSL.
        - `clusterId`: De ID van het cluster.
    - Retourneert een belofte van `void`.
3. **`isServiceAccountValid(service_account: any): boolean`**
    
    - Controleert of een service-account geldig is op basis van de aanwezigheid van vereiste velden.
    - Parameters:
        - `service_account`: Het service-account dat wordt gecontroleerd.
    - Retourneert een booleaanse waarde.
4. **`applyHubsClientDeployment(body: KubernetesStackRequest): Promise<InstanceResponse>`**
    
    - Past de implementatie van een Hubs-client toe op een cluster.
    - Parameters:
        - `body`: Bevat informatie over functies, service-account, cluster-ID, namespace en domein.
    - Retourneert een belofte van `InstanceResponse`.
5. **`createNewHubsStackDeployment(body: KubernetesStackRequest): Promise<InstanceResponse>`**
    
    - Creëert een nieuwe implementatie van een Hubs-stack in een cluster.
    - Parameters:
        - `body`: Bevat informatie over functies, service-account, cluster-ID, namespace, domein en variabele opties.
    - Retourneert een belofte van `InstanceResponse`.
6. **`createNamespaceInCluster(body: KubernetesNamespaceRequest): Promise<any>`**
    
    - Creëert een nieuwe Kubernetes-namespace in een cluster.
    - Parameters:
        - `body`: Bevat informatie over cluster-ID, service-account en de gewenste namespace.
    - Retourneert een belofte van `any`.
7. **`getClusterDetails(body: KubernetesClusterDetailsRequest): Promise<ClusterDetailsResponse>`**
    
    - Haalt details op over clusters in het opgegeven service-account en cluster-ID.
    - Parameters:
        - `body`: Bevat informatie over het service-account en cluster-ID.
    - Retourneert een belofte van `ClusterDetailsResponse`.
8. **`listClusters(body: KubernetesListClustersRequest): Promise<ClusterDetails[]>`**
    
    - Lijst alle clusters op in het opgegeven service-account.
    - Parameters:
        - `body`: Bevat informatie over het service-account.
    - Retourneert een belofte van een array met `ClusterDetails`.
9. **`createCluster(body: any): Promise<any>`**
    
    - Creëert een nieuw GKE-cluster.
    - Parameters:
        - `body`: Bevat informatie over het gewenste cluster en het service-account.
    - Retourneert een belofte van het aangemaakte cluster.
10. **`private async authenticate(cluster: string, zone: string): Promise<any>`**
    
    - Authenticeert met het opgegeven cluster in de aangegeven zone.
    - Parameters:
        - `cluster`: Naam van het cluster.
        - `zone`: Zone waarin het cluster zich bevindt.
    - Retourneert een belofte van geauthenticeerde informatie.
11. **`private fillContextOptions(clusterName: string, k8sCredentials: K8sCredentials): any`**
    
    - Vult de opties in die nodig zijn voor het maken van een nieuwe Kubernetes-context.
    - Parameters:
        - `clusterName`: Naam van het cluster.
        - `k8sCredentials`: Referentie naar de Kubernetes-credentials.
    - Retourneert een object met de vereiste opties.
12. **`private async initialGoogleClusterAuth(keyFile: GoogleKeyFile, clusterId: string)`**
    
    - Voert de initiële authenticatie uit met Google Cloud voor het opgegeven cluster.
    - Parameters:
        - `keyFile`: Informatie over het Google-service-account.
        - `clusterId`: ID van het cluster.
    - Retourneert een belofte van geauthenticeerde clusterinformatie.