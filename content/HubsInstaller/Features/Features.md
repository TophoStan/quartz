De Features pagina is na de home pagina de simpelste qua complexiteit.

``` ts
const [isLoading, setIsLoading] = useState(true);
const [features, setFeatures] = useState([]);
const [hasLSBeenChecked, setHasLSBeenChecked] = useState(false);
const [checkingBoxes, setCheckingBoxes] = useState(false);
```

`isLoading`: worden de features opgehaald
`features`: lijst van features
`hasLSBeenChecked`: is de Localstorage gecheckt 
`checkingBoxes`: wordt er opgehaald welke checkboxes aangevinkt moeten zijn, wanneer je een instantie wilt bijwerken


`features` wordt gevuld door een request te sturen naar de backend `api/features` waarmee alle features die opgeslagen staan in een MongoDB database gebruikt kunnen worden.

### `useEffect`
Op het moment van inladen van de pagina, wordt er gekeken of de waarde `replace` aanwezig is in `selectedHostingOptions` in `localStorage`. 

`fetchFeatures`: haalt de gekozen features op die op de draaiende deployment staat


### `getAllSelectedFeatures`
Deze functie haalt alle aangevinkte waardes op en stopt deze in `localStorage` op `selectedFeatures`.

![[Pasted image 20240124114831.png]]

### `FeatureCheckBox`
De checkboxes worden volgens een functie aangemaakt om code duplicatie te voorkomen.
Deze heeft nodig:
`name`:  de naam van de feature
`description`: een beschrijving van de feature
`image`: een link naar een afbeelding van de feature

