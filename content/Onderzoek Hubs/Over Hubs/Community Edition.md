---
tags:
  - ce
  - hubs
---
16 oktober is er een nieuwe editie van Hubs gereleased. Deze editite moet het Hubs Cloud plan vervangen door meer toegang te geven door alle onderdelen van de applicatie zoals Reticulum. Deze was voorheen niet toegankelijk en puur de Hubs Client was aanpasbaar. 

*"Mozilla Hubs pusht een drastische verandering"* is wat ik in mijn risicoanalyse heb geschreven. En dit is waar geworden. Mijn opdracht was om met het hubs Cloud platform te werken, maar dit is sinds de update van de community edition gewijzigd om daar dus mee te werken. Dit betekent dat er een risico waar ik eigenlijk geen rekening mee had gehouden waar is geworden. Mijn opgedane kennis kan ik wel weer toe passen op de nieuwe editie, maar de tijd die ik in r&d gestopt heb, om het zo maar even te benoemen, is niet zo effectief geweest dus. 

**Guides**
- [Opstart process](https://hubs.mozilla.com/labs/community-edition-case-study-quick-start-on-gcp-w-aws-services/ ) Google Cloud
- 


## **Release**
[Github repo](https://github.com/mozilla/hubs-cloud/tree/master) van Mozilla hubs. Een mapje genaamd `community-edition` heeft de belangrijke bestanden voor het opzetten van een hubs-stack in zich bevinden. Er zijn de volgende bestanden aanwezig:
- `cbb.sh` 
- `cbb.yam`
- `hcce.yam`
- `readme.md`
- `render_hcce.sh` 

Het process wordt gestart op het moment van uitvoeren van de volgende code in je terminal
`bash render_hcce.sh && kubectl apply -f hcce.yaml` 


**LET OP!**
*Door `kubectl` te gebruiken, wordt de hubs instantie op het cluster van de huidige context toegepast. Let dus op of je op het goede cluster zit.* 


Vervolgens moet je het IP zoeken van de Loadbalancer, dit kan je doen door `kubectl -n <hcce_namespace> get svc lb`. Het gevonden IP zet je in de DNS service van je domein en op de volgende subdomeinen:
- assets.DOMEIN 
- stream.DOMEIN 
- cors.DOMEIN 

Daarna moeten er certificaten worden toegevoegd, dit kan handmatig. MAAR het kan ook automatisch gedaan worden door CertBot. 

Dit kan met behulp van het `cbb.sh` bestand. Dit bestand zorgt ervoor dat Certbot gaat draaien en je certificaten krijgt voor je domein. 
**Zorg er wel voor dat je variabelen in het bestand goed staan**  
`bash cbb.sh` om het bestand uit te voeren
 
## **Componenten nodig voor het draaien van Hubs Community Edition**
- Reticulum
- Spoke
- Hubs
- Dialog
- NearSpark
- Certbot-http
- Photomnemonic
- HaProxy
- pgBouncer
- Pgsql
- Coturn

### **Reticulum**
Reticulum is de backend server die alles orkestreert, vaak is hier ook maar 1 instantie van door de limitaties van replicatie.
[Reticulum](https://github.com/mozilla/reticulum)


### Spoke
Spoke is de scene editor van het Mozilla Hubs platform waarmee scenes aangemaakt, bewerkt en gepubliceerd kunnen worden.
[Spoke](https://github.com/mozilla/spoke)
### Hubs
Hubs is front-end applicatie die de eindgebruikers te zien krijgen. Hubs wordt vaak ook als Hubs Client benoemd, omdat het de Client is waarvan de users gebruik maken. 
[Hubs](https://github.com/mozilla/hubs)
### Dialog
Dialog, zoals de naam in het Engels impliceert, is verantwoordelijk voor Audio en Text in Hubs Client. Dit wordt met WebSockets geregeld, Dialog luistert slechts. Hubs en Reticulum initiëren dus het proces van communicatie. 
[Dialog](https://github.com/mozilla/Dialog)
### Nearspark
Nearspark is verantwoordelijk voor het genereren van thumbnails voor Avatars, Scenes en Rooms en ook afbeeldingen in het algemeen.
[Bron](https://github.com/MozillaReality/nearspark)
### Certbot-http
Certbot is een nieuw onderdeel in de stack die in Hubs Cloud nog niet aanwezig was en in Community Edition wel. Certbot is verantwoordelijk voor het regelen van Certificaten waarmee TLS/HTTPS gewaarborgd kan worden op je website. Certbot werkt vaak samen met Let's Encrypt. Let's Encrypt wil dat je een bestand op je webserver upload, om te bewijzen dat je de eigenaar bent van de website door de digitale handtekening te verifieeren op die bestandslocatie van de website. Als alles goed is gegaan, verkrijgt je domein Certificaten van Let's Encrypt.
[Bron](https://www.youtube.com/watch?v=jrR_WfgmWEw)
### Photomnemonic
Photomnemonic is verantwoordelijk voor het toe in staat stellen tot het maken van screenshots
[Bron](https://github.com/MozillaReality/photomnemonic)
### HaProxy
HaProxy is een van de meest gebruikte proxies voor webapplicaties. Ook is het een LoadBalancer die vaak als standaard gebruikt wordt door de grote cloud marktspelers. 
[Bron](https://www.haproxy.org/#desc)

### PgBouncer
PgBouncer is een LoadBalancer voor Postgres databases. Het maakt gebruik van connection pool's, hierdoor hoeft er niet steeds een nieuwe connectie vastgesteld te worden met de database. Maar worden er juist al bestaande connecties hergebruikt.
[Bron](https://www.pgbouncer.org/faq.html#how-to-load-balance-queries-between-several-servers)
### Pgsql
PgSQL is een toevoeging op SQL, waarmee meer computationele stukken gebouwd kunnen worden IN de database, wat onder andere performance optimalisaties levert.
[Bron](https://www.postgresql.org/docs/current/plpgsql-overview.html#PLPGSQL-ADVANTAGES)
### Coturn
Coturn zorgt ervoor dat er op de meest efficiënte en effectieve manier een P2P verbinding vastgesteld kan worden. 
[Bron](https://www.youtube.com/watch?v=_4FkRf9utSc) 
## **Huidige issue**
De CORS server werkt op dit moment niet, dit betekent dat er geen nieuwe content geïmporteerd kan worden. 

