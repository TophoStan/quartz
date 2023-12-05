

![[Pasted image 20231127103250.png]]


![[Pasted image 20231127103304.png]]
In het mapje BoldlyXR zitten, zoals in de bovenstaande afbeelding te zien is, diverse `.js` bestanden met eventueel een `.scss`-bestand. Op het moment dat er een  `.scss`-bestand bij staat, is het aan frontend component. Het opmerkelijke hier aan is dat het `interactions.js` al bestaat, maar dan in `src/systems/`. Maar op de plaatsen waar vervolgens `interactions.js` geimporteerd wordt, wordt deze overschreven door regels later `interactions.js` uit  `src/ BoldlyXR` te importeren. Dit is te zien in de onderstaande afbeelding.

![[Pasted image 20231127103732.png]]

Een voorbeeld van een toevoeging op plaatsen. `hub.js` regel 466,471
``` js
 // We've already entered, so move to new spawn point once new environment is loaded
            if (sceneEl.is("entered")) {
              waypointSystem.moveToSpawnPoint();
              // oli414
              window.BoldInteractions.hasJoined = true;
            }
```


Het meest recente project en een project dat tot dit moment nog onderhouden wordt is Metacollege. Voor dit project zijn diverse custom features ontwikkeld. De ontwikkelaars van Boldly zijn op weg om minder tightly coupled te werk te gaan, maar er zitten nog steeds wel hardgecodeerde onderdelen in verwerkt. Dit zorgt ervoor dat het niet direct in bijvoorbeeld een NPM package gezet kan worden, maar er is natuurlijk ruimte om hiermee te spelen.
![[Pasted image 20231128113456.png]]