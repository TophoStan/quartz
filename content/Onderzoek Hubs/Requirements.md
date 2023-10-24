#hubs 
De flow van de applicatie bestaat uit 5 stappen
- Home pagina
- Deployment hosting
- Hubs features toggle 
- Module installatie (custom features)
- Overzicht pagina

**Home pagina**
Niks bijzonders voor nodig in het specifiek

**Deployment hosting**
Hier moet er gekozen kunnen worden waar je wilt hosten, zoals Azure, AWS of localhost. Aangezien er met Docker containers wordt gewerkt is het, het meest logisch om een programma als Kubernetes te gebruiken om deze containers te overzien en te managen. Elke grote speler in de markt als Amazon, Microsoft en Google ondersteunen dit. Met een Kubernetes configuratie bestand wordt het dus mogelijk om minder tightly te ontwikkelen.  

*Nieuwe technologie*
- Kubernetes

**Hubs features toggle**
Er kan met behulp van het door passen van environment variabelen keuze worden gemaakt welke modules aan of uitgezet worden. Dit is in detail uitgewerkt in [[Slopen features hubs]]. De pagina zelf is uitgerust met een paar check- of toggleboxen waarmee features, slechts tijdens installatie aan of uit gezet kunnen worden. 

**Module installatie (custom features)**
Hier heb ik nog niet eerder mee gewerkt of überhaupt ooit over nagedacht. Maar Albert kwam wel met een suggestie. Je kan het zien als een git repository, bijvoorbeeld Github, waarop je branches maakt van versiebeheer. Vervolgens kan je ergens op klikken en in woorden van Albert, wordt er een package "uitgepoept" die gebruikt kan worden in de Hubs applicatie.

**Overzicht pagina**
Hierop bevind zich net als de home pagina niks speciaals, maar wat er wel op zit is een knop waarmee het process daadwerkelijk geïnstantieerd wordt. Dit kan door het sturen van een API call naar de gekozen hosting partij.


