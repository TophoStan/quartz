Voor het draaien van Mozilla Hubs instanties zijn er diverse mogelijkheden. Dit komt doordat het draait op Kubernetes. Het moment dat een platform Kubernetes clusters ondersteund, wordt het velen malen makkelijker om iets te deployen. Omdat het gezien kan worden als een laag die alles voor je managed. Daarom is er minder kennis nodig van platform zelf. 

**Welke partijen ondersteunen Kubernetes**
Kubernetes wordt ondersteund door vele grote spelers als Microsoft op Azure, Amazon met AWS en Google met de Google Kubernetes Engine. Naast deze 3 giganten zijn er nog veel meer platformen, hier een paar [(bron)](https://technologymagazine.com/top10/top-10-managed-kubernetes-platforms):
- Platform9
- Linode Kubernetes Engine
- Red Hat OpenShift
- Alibaba Cloud Container Service for Kubernetes
- IBM Cloud Kubernetes Service
- VMWare Tanzu Kubernetes Grid
- DigitalOcean Kubernetes

**Vereisten voor het draaien van een volle stack van Mozilla Hubs**
Voor het draaien van een hubs-stack zijn er onderdelen nodig vanuit het hostingplatform. Hier kan je denken aan IPv4 addressen bijvoorbeeld. De meeste platformen zetten deze onderdelen klaar op het moment dat je jouw hubs-stack voor de eerste keer opzet. Vandaar dat het langer kan duren voordat er een daadwerkelijke interface beschikbaar komt zoals de Hubs Client. Er ligt geen standaard vast qua hardware, vanwege de complexe overwegingen die gemaakt zouden moeten worden en de vele data die nodig is. Vandaar dat het handig is om onder een iets zwaarder dan gemiddelde load te testen, zodat je altijd genoeg overhead hebt. En daarnaast kan je aangeven in je configuratie dat er automatisch bijgeschaald moet worden, indien data als netwerkverkeer dat zegt.



**Hoe werkt het deployen naar een provider met de HubsInstaller**

Na het opstarten van de applicatie kan je daar naartoe gaan, als deze lokaal staat kan je naar http://localhost:3000. Waar je vervolgens naar de home pagina wordt gestuurt op `/app/`. 

![[Pasted image 20231122150150.png]]

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
