# Eigen vragen

- In de casus staat: “Er kan maximaal één developer aan een backlog item gekoppeld worden. Indien er meer developers aan gaan werken, dienen activiteiten onder het backlog item bijgemaakt te worden”. Kunnen er dan toch meer developers aan een taak worden gekoppeld? Of kunnen developers aan activiteiten worden gekoppeld en kunnen dit er dan meer zijn?
    - Max 1 als er geen activiteiten zijn, meerdere als er wel activiteiten zijn

- In de casus staan verschillende rollen beschreven (product owner, Scrum master, developer, tester). Moet er ook iets van autorisatie komen om te controleren of de persoon de juiste rol heeft om een actie te doen?
    - Nee. Maar wel role based acties dat een product owner dus meer kan dan een developer.

- Is tester überhaupt een rol? Of zijn dit ook gewoon developers?
    - Nee

- Er zijn 2 type sprints, een deelproduct en release van software.  In de casus, bij release van software, wordt er gezegd dat er ook een review wordt gedaan voordat de release in gang wordt gezet. Is dit ook een sprint review? Of is de fase sprint review alleen voor een deelproduct?
    - Nee.

- "Mocht de release wel in gang worden gezet, dan wordt de voor die sprint ingestelde development pipeline gestart (installeren packages, build, test etc. tot en met deployment).”Ik neem aan dat dit niet helemaal uitgewerkt hoeft te worden? Meer aangeven met string?
    - Ja, aangeven met strings.

- In de casus staat dat er een mogelijkheid is dat developers met elkaar kunnen discussiëren en er verschillende discussiethreads kunnen ontstaan. Wij hebben dit geïnterpreteerd als dat er onder elke backlog item een discussiethread kan ontstaan. Maar nu hebben we vernomen van andere studenten dat deze forum een heel apart ding is op de applicatie. Wat is juist? Of is eigen implementatie/aanname met een goeie onderbouwing leidend?

![Leeg diagram.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b3f1d171-92c6-4aba-a504-51a9049bed69/2f70e973-678d-409c-993c-b5892043ac2e/Leeg_diagram.png)

Staat niet onder elkaar. Naast elkaar

- De overige patronen die we willen gebruiken per functionaliteit is ons best duidelijk. Zo niet komen we erop terug. Maar voor nu zitten wij wel met de composite pattern. Hierboven zie je de boom structuur. Eerst wilden we de composite toepassen op deze gehele boom, maar dat lijkt ons niet correct. Alsof we de composite dan niet goed benutten. Is het bijvoorbeeld wel goed om van de thread een composite te maken met reacties als leafs?
    - Backlog item als composite omdat je dan kunt checken wanneer iets af is. Dus je kunt kijken of taken af zijn die onder een backlog item vallen.
- Bij het genereren van een rapportage is er de mogelijkheid om headers en footers toe te passen. Hoe moeten we dit voor ons zien? Zijn deze headers en footers twee aparte classes die dan aan het bestand mee gegeven kunnen worden? En moet een bestand ook echt gegeneerd kunnen worden of is alleen met string aangeven in welk formaat deze komt te staan voldoende?
    - Template pattern
    - Aangeven met een string

Misschien dat we deze vraag ook gewoon nog een keer aan Marc kunnen stellen

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b3f1d171-92c6-4aba-a504-51a9049bed69/4f834a1f-ec0c-496f-81fe-085460ea9960/Untitled.png)

- Antwoord: adapter pattern? Als er in ons systeem een push wordt gedaan wordt dit natuurlijk getriggered in het daadwerkelijke Git systeem. Wij maken dit externe systeem enkel en alleen met de benodigde functies en als ie wordt uitgevoerd, loggen we dit wat er gebeurt. EZ.

- Moeten de verschillende acties (package, build, test etc.) van de development pipeline automatisch worden uitgevoerd? Of kan dit handmatig worden gedaan?
    - Automatisch achter elkaar uitvoeren.