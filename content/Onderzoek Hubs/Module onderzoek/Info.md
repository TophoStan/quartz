#hubs 
# Wat is een module?
Een module is een stuk code dat op een `npm`-achtige wijze geïnstalleerd kan worden in je applicatie. 

### Waar bestaat een Module uit?
Een module heeft:
- een Naam
- een readMe
- een versie
- code

### Huidige problemen
Het probleem wat zich nu voordoet op de werkvloer bij de ontwikkelaars van Boldly-XR, is dat eerder ontwikkelde features niet makkelijk overgebracht kunnen worden naar huidige of toekomstige projecten. 

### Doel
Het doel is dus om een oplossing te ontwikkelen waarmee een feature als Module geïnstalleerd kan worden 


### Tools onderzoek
#### NPM
NPM staat voor Node Package Manager en is dus de package manager voor NodeJS Packages. Een NPM kan privé online gehost worden op het NPM register. Dit betekent dat je code naast een Git repo, ook op een andere plaats. Hiermee is er geen Single Point of Failure meer aanwezig qua punten waar je data op staat. Uiteraard hebben Github en NodeJS hoogstwaarschijnlijk niet alles op 1 centrale database staan die verwijderd kan worden. Daarnaast kan door het gebruiken van NPM, gebruik gemaakt worden van een bestaand applicatie die elke ontwikkelaar kent. Versiebeheer is tevens net zo makkelijk op NPM ([source](https://docs.npmjs.com/creating-a-package-json-file)). 

Het Mozilla Hubs platform bestaat uit diverse applicaties en het gros daarvan is ontwikkeld in de taal JavaScript. Voor het draaien van deze JavaScript applicaties op een server wordt gebruik gemaakt van Node.JS. Hierdoor scoort NPM al gelijk wat puntjes.





### Hoe zien deze eigen ontwikkelde onderdelen er uit?
Het development team heeft meerdere projecten waarvoor op maat gemaakte onderdelen zijn ontwikkeld. De meest recente projecten zijn voor het Spookslot van de Efteling en een klant genaamd Westherts. Westherts college is een school in Watford, Verenigd Koninkrijk. 

Tijdens het bekijken van de ontwikkelde onderdelen zijn een paar dingen eruit gesprongen. De broncode van deze onderdelen staat altijd in een mapje `BoldlyXR` in de `/src` map.









### Mogelijke oplossingen
Een mogelijke oplossing kan zijn, dat je een bestand pusht naar een git repository. Met een Git repository is het mogelijk om versiebeheer toe te passen door middel van het gebruik van Tags en Branches. Een implementatie van Git die gebruikt kan worden hiervoor, is Github. De meest gebruikte Git provider en tevens ondersteund het ook Github Actions. Dit betekent dat er een automatische vorm van deployment met een CI/CD straat. Hier komt vervolgens een npm package uit, deze package kan je dan installeren en toe passen op je code.


### Uitdagingen
Al is het gelukt om oude broncode in een package te zetten. Hoe zal je het voor elkaar krijgen om oude projecten te migreren. 



### Conclusie



### Wat is het proces achter een Module?
In de woorden van Albert: *'Je pusht iets naar een repository en op een of andere manier als je ergens op drukt poept die een package uit'* 
