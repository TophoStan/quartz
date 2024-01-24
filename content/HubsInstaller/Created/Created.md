![[Pasted image 20240124132522.png]]


```ts
const [data, setData] = useState<KubernetesPostResponse>(null)
const [isLoading, setLoading] = useState(true)
const [isFetching, setFetching] = useState(false)
const [count, setCount] = useState(0)
const [error, setError] = useState(false)
const [isLocalstoragePresent, setIsLocalStoragePresent] = useState(true)
```

`data`: details als IP, domein een message van de deployment
`isLoading`: is er al een response terug vanuit de API
`isFetching`: wordt er helemaal niet meer gecalled naar de API
`count`: hoeveelheid keer dat de API is aangesproken 
`error`: error bericht van de response
`isLocalstoragePresent`: zijn de `localStorage` elementen aanwezig

### `useEffect`

`updateClient`: werkt de `hubs` deployment bij op de draaiende instantie. Conditie: `body.hosting.purpose = "create"`
`createHubsStack`: deployed een gloednieuwe stack naar het gekozen platform. Conditie: `body.hosting.purpose = "replace"`



