


``` ts
  const [authUrl, setAuthUrl] = useState('');
  const [credentialsJson, setCredentialsJson] = useState({} as any);
  const [googleAuthCode, setGoogleAuthCode] = useState('');
  const [isAuthCodeInLocalStorage, setIsAuthCodeInLocalStorage] = useState(false);
  const [modules, setModules] = useState([]);
  const [canGoToFinalise, setCanGoToFinalise] = useState(false);
  const [selectedModules, setSelectedModules] = useState([] as []);
```

`authUrl`: het URL waarnaar de gebruiker wordt toegestuurd in de popup window
`credentialsJson`: het `credentials.json` bestand
`googleAuthCode`: de code die uit de popup window is gehaald
`isAuthCodeInLocalStorage`: is de `googleAuthCode` aanwezig in `localStorage`
`modules`: een lijst van modules
`canGoToFinalise`: kan er naar de volgende pagina genavigeerd worden
`selectedModules`: modules die geselecteerd zijn


`checkLocalStorage`: kijkt of `code` in `localStorage` aanwezig is.


### `useEffect`
Binnen het `useEffect` blok wordt er gekeken of er `code` in de URL aanwezig is. Indien dat zo is, bevind je jezelf in een pop window. 

`authGoogleDrive`: vraagt google gegevens op om toegang te krijgen tot de Drive. 
Conditie: `googelAuthCode != ''` & `modules.length

`fetchAllModules`: haalt alle Modules op door de bestanden van de google Drive door te lezen op de structuur die wordt doorgenomen in [[Module inhoud]]. Conditie: `wordt na authGoogleDrive` uitgevoerd.



### Flow

Geef het `credential.json`-bestand mee aan de input die je in [[Service account Drive]] hebt aangemaakt.

![[Pasted image 20240124115218.png]]


![[Schermafbeelding 2024-01-24 120246.png]]

![[Schermafbeelding 2024-01-24 120256.png]]


### `DownloadHubsPage`
``` ts
const [currentProgressState, setCurrentProgressState] = useState("");
const [hasStartedBuild, setHasStartBuild] = useState(false);
const [dockerImageName, setDockerImageName] = useState("")
```

`currentProgressState`: het huidige proces dat draait in de vorm van een string
`hasStartedBuild`: is Docker al bezig met builden
`dockerImageName`: de naam van de uiteindelijk gebouwde image

#### `useEffect`
Binnen dit blok wordt gekeken of `dockerImageName` een waarde heeft, indien dat zo is `setCanGoToFinalise(true)`
#### `cloneHubs`
Stuurt een request naar `api/modules/clone` om de aangevinkte modules te downloaden en te plaatsen in een hubs project


#### `valideSelectedModules`
Stuurt een request naar `api/modules/validate` om de aangevinkte modules te valideren op bestaan

#### `initiateHubsFilesProces`
Roept `cloneHubs` aan en zet `setHasStartedBuild` op `true`


