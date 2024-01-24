
Op deze pagina wordt de frontend doorgenomen, met betrekking tot hoe een gebruiker de app zou gebruiken. Voor meer gedetailleerde informatie zijn er specifieke hoofdstukken per pagina beschikbaar.
## Frontend
Na het opstarten van de applicatie kan je daar naartoe gaan, als deze lokaal staat kan je naar http://localhost:3000. Waar je vervolgens naar de home pagina wordt gestuurt op `/app/`. 

![[Pasted image 20231122150150.png]]

### Hosting
Na het klikken op de knop rechts onderin, wordt je doorgestuurd naar de `hosting` pagina. 
![[Pasted image 20231122150836.png]]

Als je klikt op 1 van de input knopjes. Krijg je nog een knop te zien, deze knop is een `input` die alleen `.json` bestanden accepteert. Maar dit kunnen niet zomaar `.json` bestanden zijn. Het JSON bestand moet een service account representeren. Een service account is een account dat je aanmaakt bij je hosting provider. Dit account is dan een account moet speciale toegang tot bepaalde onderdelen van je online machines.

Voorbeeld van een mogelijk service account, in dit geval van Azure
```json
{                                                        
  "appId": "128638u12y3iou12osadjf98sadfsad89",       
  "password": "NF)*(#Y@*&RH#(*YHHJDSHFKJSJF*)",
  "tenant": "jhasd-fsdafsa0-asdfassaf--asdf"       
}                                                        
```
Voorbeeld Google 
```json
{
  "type": "service_account",
  "project_id": "johndoeproject",
  "private_key_id": "8y238fg874325r973hjsdfljkhjsdfg8",
  "private_key": "-----BEGIN PRIVATE KEY-----AoGBAK6y7uiNLvCrztcz7ZLc\nX2p22mCBBX7ZFyIkRMl+AKf3oTaR5JCZUKxO+ELWGWF1Nxf40IdQEIQmXTycqsch\nQvezDw6NY/dNyzXt2luKaX3b7IPJY0ULI+uJGzD87tDBNLI7BZrnXNLy9H2Spf9h\nyYwBSWqwBkyclgOLHxjxfXxU\n-----END PRIVATE KEY-----\n",
  "client_email": "johndoe@mail.iam.gserviceaccount.com",
  "client_id": "1231245",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/installerapi%40johndoe-mail.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
}
```


Vervolgens na het geven van het correcte JSON bestand, krijg je weer een invoerveld. Je kan hier een cluster kiezen, mits deze aanwezig is en het opgegeven service_account valide was.

![[Pasted image 20231122154532.png]]

Er is een API endpoint beschikbaar, POST `api/cluster`, om een cluster programmatisch aan te maken. Alleen lijkt het mij niet verstandig om dit programmatisch te laten gebeuren aangezien het een resource is dat met hevige complexe opties die meegestuurd kunnen worden, vandaar dat ik slechts een endpoint ontwikkeld heb. Maar deze is slechts voor cluster 1 configuratie geschikt.

Vervolgens is het mogelijk om een doel te kiezen, wat je wilt bereiken met deze installatie dus. 
![[Pasted image 20231122155103.png]]

Je kan een instantie vervangen of een nieuwe aanmaken. Wat er gebeurt bij het vervangen is dat de features die aangeklikt zijn worden toegepast op de Hubs Client, mits deze verandert zijn anders blijft de draaiende deployment hetzelfde. Bij het aanmaken van een instantie wordt er dus een hele stack aangemaakt. Realiseer wel dat je niet teveel hubs-stacks moet draaien op 1 cluster met weinig resources.

Vervolgens kan je kiezen welke deployment je wil aanpassen op basis van een zogeheten `namespace`. Een namespace is een label dat een resource toegereikt krijgt om zich te onderscheiden van andere onderdelen/ hubs-stacks. 
![[Pasted image 20231122155414.png]]
Je kan ook een namespace aanmaken, maar deze moet wel in allemaal lowercase zijn zonder speciale tekens!

![[Pasted image 20231122155456.png]]

Vervolgens kan je doorklikken als alle velden voldoen aan de criteria

![[Pasted image 20231123105850.png]]


### Features
Nadat je valide gegevens ingevuld hebt op de hosting-pagina, kan je door naar de features-pagina.
![[Pasted image 20240116101905.png]]

Op deze pagina is een lijst met checkboxes aanwezig. Een checkbox staat voor een ingebouwd onderdeel van de Hubs Client. Wanneer een checkbox aangevinkt is, wordt deze dus aangezet in de hubs client. Je ingevulde keuzes worden net als bij de hosting in Local Storage gezet, om deze later in de applicatie weer op te halen. Je keuze wordt pas in localstorage gezet wanneer je naar de volgende pagina gaat.

**Modules**
![[Pasted image 20240116102327.png]]
De module pagina bestaat uit een lijst van, je raad het, modules. De naam van de module met een beschrijving staan daarin verwerkt, met een versie. Na uiteindelijk op finalise te drukken, worden de modules gedownload van Drive en worden deze na het clonen van een Hubs Repo daarin gezet met import statements in externe bestanden als dat nodig is. Meer details in [[Module oplossing]] hoe dit process in zijn werking gaat. 

**Summary**

![[Pasted image 20240116102651.png]]
Vervolgens kom je op de summary-pagina, waarop je je domein moet invoeren die je voor je Mozilla Hubs stack wilt gebruiken. Het gebruik van een domein is een vereiste! Daarnaast krijg je een overzicht van je ingevulde keuzes bij de hosting, features en modules pagina's. 

**Created**
Na zeker te weten je instantie aan te willen maken door op "create" te drukken, kom je op de "created" pagina. Deze pagina maakt op de achtergrond je instantie aan met de ingevulde keuzes van het hosting platform, features die aan/uit staan. Dit proces kan 1 of meerdere minuten duren.
![[Pasted image 20240117162546.png]]
Uiteindelijk krijg je een scherm te zien met informatie als het IP-adres waarnaar gewezen moet worden door het domein. Na deze stap kan weer genavigeerd worden naar de home pagina.

## API
Zowel de frontend als de backend is in het NextJS framework geschreven. De developers bij Boldly hebben al veelal gebruik gemaakt van React en NextJS, dus was de keuze vanzelfsprekend. De API bestaat heeft verschillende routes, elke route met hun eigen doel. Tevens beschikt de API ook over documentatie te vinden op de `/docs` endpoint.

#### /Kubernetes
De route met de meeste mogelijkheden is het pad dat begint met `api/kubernetes`. Kubernetes wordt sterk gebruikt in samenwerking met een client die op basis van geselecteerde gegevens, die in het volgende hoofdstuk uitgelegd worden, acties uit kan voeren die je normaliter met de Kubernetes CLI zou uitvoeren. Alleen het Kubernetes pad zelf voert niet veel uit en heeft niet veel toegevoegde waarde!!!!!

##### kubernetes/clusters
Deze endpoint stelt beschikbaar om data over een cluster op te vragen en deze ook aan te maken. Bewerken en verwijderen zitten niet verwerkt in de endpoint, omdat deze actie veel complexe opties kan bevatten en het niet vaak gebeurt. Daarom is de afweging gemaakt om dat te doen. Wat deze endpoint dus wel toelaat is het ophalen van de clusters die gerelateerd zijn aan het meegegeven `service_account`

###### kubernetes/clusters/\[\name\]
Deze endpoint verzorgt het beschikbaar stellen van de detail data van clusters. Als er over de details van een cluster worden gesproken worden in deze context, slechts beperkte data bedoelt, omdat er anders data beschikbaar gesteld wordt voor geen enkele reden. Een cluster bestaat in principe, zover de applicatie betreft, uit een naam, IP of een DNS-adres en een status. Want een Cluster object van bijvoorbeeld Google geeft allerlei data terug die niet interessant is voor de applicatie. Dit is een voorbeeld van een response met data die je terug krijgt van Google.

``` json
"config": {
            "url": "https://container.googleapis.com/v1/projects/hubs-installer-stan/zones/europe-west4-a/clusters",
            "method": "POST",
            "userAgentDirectives": [
                {
                    "product": "google-api-nodejs-client",
                    "version": "7.0.1",
                    "comment": "gzip"
                }
            ],
            "data": {
                "cluster": {
                    "name": "your-cluster-name",
                    "description": "Your GKE Cluster Description",
                    "initialClusterVersion": "1.27.3-gke.100",
                    "nodePools": [
                        {
                            "name": "default-pool",
                            "config": {
                                "machineType": "n1-standard-2",
                                "diskSizeGb": 100,
                                "diskType": "pd-standard",
                                "oauthScopes": [
                                    "https://www.googleapis.com/auth/logging.write"
                                ],
                                "serviceAccount": "default"
                            },
                            "initialNodeCount": 3
                        }
                    ]
                }
            },
            "headers": {
                "x-goog-api-client": "gdcl/7.0.1 gl-node/18.17.1",
                "Accept-Encoding": "gzip",
                "User-Agent": "google-api-nodejs-client/7.0.1 (gzip)",
                "Authorization": "Bearer ya29.c.c0AY_VpZj4qdynHOYtbb9NYd6Vu0R_UfIaNxQYdlZWOgaJGps4jnXOEVE_3ZRZrFNUwKd3Zg_UvEyuz9UZjWTMCAjhiXpl0rvlvX4tHGvXDP15WtbbeDeIbBXkOi1LrWVhFtMa3GuGapo-w44cpIj9Y27VL-aLjwljX8_uN9NGWs267-nTjupXMKKI6ogVr1ynRD97drHvVOkllPctH7EtTHZFzA2Nxnxr2nof7AQ82QX91OypJiA25nu6_t3mAXtfH_IyOLJXj55o9qBREBUdSgAJ6IXNjs4w_dkB8xlUUxDvpCybpNNZpAnfSe1mTuv8pK9fJF6s8FyNZ5oVkWLPqYBYU9H5jAjrjfTCFTIRLIyUzHvm498yvzVHaQE387Cu90lUQS39oMVwfupy6kyaFon7yuQQdl_leOp496qBkcJ-w3asIQtUMVmdiWn6BrJhMgYMnYxxZYc9Us_-U0Y5uJ52dUie1eQrzudu1Sfq78S_u67tBv9rqn2R-vhmisYUdetsu6uqkfwtrRQ4qlmxOhtkUYZg2V42m2OI_VFti7VqUs4Rk4moZ-F0wjuZvg1fv_0VzqZJ3g-eBI4xB-BreqXqkp4_YbFx4d_zY2aoBWOX4d_kBwOl4uYMUu5__mkuh2bkFJ0OWieW97p-8Vvh5nFsnvyFSBzleJ20RBs6QFo9yr2_zwFs-r-0ue_waomi_qFR-XRa7ug9Fc-uf4u87jph3hJBbvqzydZn-49q8-cqX_I-_l26g5hMuVz9kjdUJM4t8l7piO4-_gXRFYWZcrkFrMzkZ4gewn9Oc7JOVWycgabp3VWjWIMvF4rOBB95Fzpna6Sgq_-Y68aRj9j9Msl_Q6UaFQuIFbuF0S1nr7SdFRnqfvQ3ypta9g1Uqd6-Xt9_FkUI5Y-YiQqgheWcIOqxe_FXhwUWJ9bM12qcdp68UUicfMp0hSbJdkBBbzsuYUUnn243ByWWo03jS2Rekq25_dnis_q9vk6ttQOpz4San7vqseqX7if",
                "Content-Type": "application/json"
            },
            "params": {},
            "retry": true,
            "body": "{\"cluster\":{\"name\":\"your-cluster-name\",\"description\":\"Your GKE Cluster Description\",\"initialClusterVersion\":\"1.27.3-gke.100\",\"nodePools\":[{\"name\":\"default-pool\",\"config\":{\"machineType\":\"n1-standard-2\",\"diskSizeGb\":100,\"diskType\":\"pd-standard\",\"oauthScopes\":[\"https://www.googleapis.com/auth/logging.write\"],\"serviceAccount\":\"default\"},\"initialNodeCount\":3}]}}",
            "responseType": "unknown"
        },
```

Vervolgens haal je met de endpoint namespaces op die in het cluster aanwezig zijn. Ook is het mogelijk om een namespace aan te maken binnen het cluster waarop de http request wordt aangeroepen.


##### kubernetes/deployment
Deze endpoint verzorgt de data omtrent het aanpassen van een draaiende hubs oplossing. Je geeft in je body mee welke onderdelen van de Hubs Client je aan of uit wil hebben en deze worden toegepast op de de deployment en de huidige draaiende pods relaterend aan die pod worden verwijderd, waarna een nieuwe automatisch door Kubernetes opgestart wordt. Het opmerkelijke aan deze endpoint is dat het rond de minuut kan duren voordat je een response krijgt.

##### kubernetes/namespaces
Deze endpoint haalt alle 