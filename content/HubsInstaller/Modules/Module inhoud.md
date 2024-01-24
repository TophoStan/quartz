Voor het ontwikkelen van een oplossing voor het makkelijk installeerbaar maken van eerder ontwikkelde features voor Mozilla Hubs, is het volgende bedacht.

**Module**
- Naam
- Een of meerdere bestanden
- Versie
- ModuleInfo.json

**Waar staan modules opgeslagen?**
Modules staan opgeslagen in Google Drive. Het vereiste is dat je een map hebt die 'hubsInstaller' heet. In dit mapje kan je dan de modules neerzetten. De folder structuur ziet er als volgt uit. De structuur van `HubsInstaller/MODULENAAM/VERSIE/....` moet aangehouden worden. De app zoekt namelijk binnen de `hubinstaller`-folder naar de modules. Tevens moet er ook altijd een ModuleInfo.json aanwezig zijn.
![[Pasted image 20240116092024.png]]
**ModuleInfo.json**
Een moduleInfo bestand heeft altijd  `name: string`, `Version: string`, `codeLines: [codeLine]` als waardes. Een `codeLine` bevat `fileName: string`, `lineToBeAdded: string`, `beneath: string` en optioneel `replaceLine: "above" of "beneath"`. 
Een voorbeeld van een ModuleInfo.JSON-bestand staat hieronder. Deze komt uit de `avatarSystem`-module.

``` JSON
{
    "name": "AvatarSystem",
    "Version": "V1",
    "codeLines": [
        {
            "fileName": "src/react-components/home/HomePage.js",
            "lineToBeAdded" : "import { CreateHomepageDropdown } from '../../BoldlyXR/react-components/home/CreateRoomDropdown';",
            "beneath": "import maskEmail from '../../utils/mask-email';"            
        },
        {
            "fileName": "src/react-components/home/HomePage.js",
            "lineToBeAdded" : "{canCreateRooms && <CreateHomepageDropdown/>}",
            "beneath": "{canCreateRooms && <CreateRoomButton />}",
            "replaceLine": "above"
        }
    ]
}
```


**Programmatische oplossing**
Voor het toepassen van de theorie die hierboven vermeld staat, wordt er gebruik gemaakt van de `googleapis` NPM-package. 


**Overige vereisten**
Het volgen van dit korte opstart voorbeeld is vereist
[Google example](https://developers.google.com/drive/api/quickstart/nodejs)

**Bronnen**
[Overview mogelijkheden van DriveAPI](https://developers.google.com/drive/api/guides/about-sdk) 
