![[Pasted image 20240124124952.png]]

```ts
const [domain, setDomain] = useState('')
const [isFetching, setFetching] = useState(false)
```

`domain`: het ingevulde domein 
`isFetching`: worden eerder ingevulde waardes opgehaald

### `SelectedHosting`

![[Pasted image 20240124125351.png]]

```ts
const [hostingOptions, setHostingOptions] = useState({} as any);
```
`hostingOptions`: het object dat de opties uit het hosting proces bevat
#### useEffect
`setHostingOptions` wordt aangeroepen, mits `hostingOptions.hosting != undefined`

### `InstalledFeatures`

![[Pasted image 20240124125811.png]]

``` ts
const [features, setFeatures] = useState({} as any);
const [featuresIsNotEmpty, setFeaturesIsNotEmpty] = useState(false);
```

`features`: lijst met aangevinkte features
`featuresIsNotEmpty`: is de lijst van features leeg

#### useEffect
In dit blok worden de features uit `localStorage` gehaald, mits `featureIsNotEmpty = false`

### InstalledModules

![[Pasted image 20240124130150.png]]

``` ts
let selectedModules!: ModuleTemplateProps[];
let selectedModulesIsNotEmpty: boolean = false;
```

`selectedModules`: lijst van aangevinkte modules
`selectedModulesIsNotEmpty`: is de lijst van modules leeg



### OptionalFields

![[Pasted image 20240124130318.png]]

``` ts
const [ADM_EMAIL, setADM_EMAIL] = useState("default@mail.com")
const [DB_USER, setDB_USER] = useState("postgres")
const [DB_PASS, setDB_PASS] = useState("123456")
const [DB_NAME, setDB_NAME] = useState("retdb")
const [DB_HOST, setDB_HOST] = useState("pgbouncer")
const [DB_HOST_T, setDB_HOST_T] = useState("pgbouncer-t")
const [PGRST_DB_URI, setPGRST_DB_URI] = useState("postgres://postgres:123456@pgbouncer/retdb")
const [PSQL, setPSQL] = useState("postgres://postgres:123456@pgbouncer/retdb")
const [SMTP_SERVER, setSMTP_SERVER] = useState("")
const [SMTP_PORT, setSMTP_PORT] = useState("")
const [SMTP_USER, setSMTP_USER] = useState("")
const [SMTP_PASS, setSMTP_PASS] = useState("")


const [reticulumImage, setReticulumImage] = useState("mozillareality/ret:stable-latest")
const [postgrestImage, setPostgrestImage] = useState("mozillareality/postgrest:stable-latest")
const [postgresImage, setPostgresImage] = useState("postgres:12")
const [pgbouncerImage, setPgbouncerImage] = useState("mozillareality/pgbouncer:stable-latest")
const [pgbouncerTImage, setPgbouncerTImage] = useState("mozillareality/pgbouncer:stable-latest")
const [hubsImage, setHubsImage] = useState("mozillareality/hubs:stable-latest")
const [spokeImage, setSpokeImage] = useState("mozillareality/spoke:stable-latest")
const [nearSparkImage, setNearSparkImage] = useState("mozillareality/nearspark:stable-latest")
const [speelycaptorImage, setSpeelycaptorImage] = useState("mozillareality/speelycaptor:stable-latest")
const [photomnemonicImage, setPhotomnemonicImage] = useState("mozillareality/photomnemonic:stable-latest")
const [dialogImage, setDialogImage] = useState("mozillareality/dialog:stable-latest")
const [coturnImage, setCoturnImage] = useState("mozillareality/coturn:stable-latest")
```

`ADM_EMAIL`: The email that is automatically granted administrator privileges from the start
`DB_USER`: The username credential for your Postgres database
`DB_PASS`: The password credential for your Postgres database
`DB_NAME`: The name of your Postgres database
`DB_HOST`: The hostname of your database
`DB_HOST_T`: The hostname of your pgbouncer
`PGRST_DB_URI`: The URI of your postgrest database
`PSQL`: The URI of your postgrest database
`SMTP_SERVER`: The address of the SMTP server
`SMTP_PORT`: The address of the SMTP server
`SMTP_USER`: The username of the SMTP server
`SMTP_PASS`: The password of the SMTP server

`reticulumImage`: the image name for reticulum
`postgrestImage`: the image name for postgrest
`postgresImage`: the image name for postgres
`pgbouncerImage`: the image name for pgbouncer
`pgbouncerTImage`: the image name for pgbouncer-t
`hubsImage`: the image name for hubs
`spokeImage`: the image name for spoke
`nearSparkImage`: the image name for nearspark
`speelycaptorImage`: the image name for speelycaptor
`photomnemonicImage`: the image name for photomnemonic
`dialogImage`: the image name for dialog
`coturnImage`: the image name for coturn

#### `useEffect`
In dit blok worden de hierboven staande waardes in `localStorage` gezet. Ook wordt er gekeken, of `replace` aanwezig is in `localStorage`. Als dat zo is, worden de waardes van de draaiende instantie opgehaald.


#### `OptionalField`
Deze klasse bevat een simpele input die bij een `onChange`-event de waarde zet naar de ingevulde waarde

Uitzonderingen hierop zijn `PGRST_DB_URI` en `PSQL`, omdat dit samengestelde waardes zijn.

![[Pasted image 20240124132145.png]]



