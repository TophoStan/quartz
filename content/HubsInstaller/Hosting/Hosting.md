De hosting pagina wordt gekenmerkt door het feit dat het in de `hosting` folder zit. De `page.tsx`-pagina bestaat uit 2 kern onderdelen: `RadioButton.tsx` en `LocalHostRadioButton.tsx`. Deze bestanden bevatten de radio button's die nodig zijn om te kunnen kiezen tussen de verschillende platformen. Op het moment is `RadioButton` verantwoordelijk voor het proces achter `Google` en `Azure`. De verantwoordelijkheid van `Localhost` wordt overgedragen aan `LocalhostRadioButton`. 


``` ts
const [currentHosting, setCurrentHosting] = useState('');
const [saveFunc, setSaveFunc] = useState();
const [canGoToNextPage, setCanGoToNextPage] = useState(false);

const [currentNamespace, setCurrentNamespace] = useState('')
const [currentCluster, setCurrentCluster] = useState('')
const [purpose, setPurpose] = useState('')
const [serviceAccount, setServiceAccount] = useState(null);
```

`currentHosting`: gekozen platform
`saveFunc`: functie die de gekozen waardes opslaat in `localStorage`
`canGoToNextPage`: voldoen alle velden aan eisen
`currentNamespace`: geselecteerde namespace
`currentCluster`: geselecteerd cluster
`purpose`: gekozen doel
`serviceAccount`: JSON-bestand dat ingevoerd is


### `RadioButton`
Radiobutton bevat de logica achter de conditionele verschijningen van bepaalde elementen op de frontend. De variabelen die gebruikt worden, worden met `useState()` gebruikt.

``` ts
const [clusters, setClusters] = useState([]);
const [isLoading, setLoading] = useState(true)
const [namespaces, setNamespaces] = useState([])
const [namespacesLoading, setNamespacesLoading] = useState(true)
const [error, setError] = useState(false)
const [clusterData, setClusterData] = useState(null)
```

`clusters`: alle opgehaalde clusters gerelateerd aan het meegegeven service account 
`isLoading`: wordt er data opgehaald
`namespaces`: alle namespaces van het gekozen cluster
`namespacesLoading`: wordt namespace data opgehaald
`error`: is er een error
`clusterData`: huidige geselecteerde cluster.

#### `useEffect`
Binnen het useEffect code blok worden meerdere onderdelen van de app uitgevoerd.

`fetchNamespaces`: Namespaces worden opgehaald, mits het doel is om een draaiende namespace aan te passen. Conditie : `purpose == 'replace' && !namespaces`

`fetchClusters`: clusters gerelateerd aan het meegegeven service account worden opgehaald. Conditie: `serviceAccount && currentCluster == '' && (props.currentHosting == value)` 


#### `CustomFileInput`
`CustomFileInput.tsx` is het bestand waarin de logica met betrekking tot het service account zich bevind. Conditie: `props.currentHosting == value` 

Op het moment dat een JSON-bestand ingevoerd is wordt de tekst eruit gehaald en in zowel `Localstorage` als de `jsonFile` `useState()` variabel. 




#### `ClusterSelect`
Na een valide `service acount` te hebben ingevoerd met draaiende clusters. Komt `ClusterSelect` onderaan, mits deze een response heeft gekregen van de API. Conditie: `serviceAccount != null` & `isLoading = false`

`isLoading = true`
![[Pasted image 20240124103157.png]]


`isLoading = false`
![[Pasted image 20240124102953.png]]


`clusters.length == 0`
![[Pasted image 20240124101733.png]]


#### `GoalRadioButtons`
De `GoalRadioButtons` bevat 2 `input[type=radio]`: replace en create.

`namespaces.length > 0`
![[Pasted image 20240124103455.png]]

Op het moment van het klikken van een van de goals. Wordt de waarde in de `purpose` state gezet.


Het moment dat er geen namespaces aanwezig op het cluster, is het slechts mogelijk om `Create a new instance` aan te klikken.


##### `ReplaceNamespace`
Op het moment dat een namespace geselecteerd is, wordt de `currentNamespace` gezet naar de naam van de gekozen namespace. Ook wordt deze in `Localstorage` gezet.
`purpose = replace`
![[Pasted image 20240124103718.png]]

##### `CreateNamespaceInput`
Op elke invoer van een toets, wordt de variabelen `currentNamespace` aangepast en ook in `Localstorage` gezet. Ook zit hier validatie op dat er geen CAPS in mogen staan en diverse andere tekens die niet toegestaan zijn met een `regex`, 
``` ts
namespace.match(/^[a-z0-9]([-a-z0-9]*[a-z0-9])?$/) && serviceAccount != null)
```

`purpose = create`
![[Pasted image 20240124103735.png]]

Na het invoeren van alle velden gaat de `Features` knop van grijs

![[Pasted image 20240124104301.png]]

naar

![[Pasted image 20240124104309.png]]
