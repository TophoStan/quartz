Module heeft 3 endpoints
- `/clone`: Complex proces dat verder wordt uitgewerkt
- `/validate` :Valideert of de opgegeven modules valide zijn
- `/`: Haalt de lijst van modules op


### `/Clone`
Deze endpoint is een `POST` endpoint met de volgende functies
- `getoAuth2Client`: Client die gebruikt moet worden voor het authentiseren van Google Drive
- `DriveProcess`: Deze functie roept de volgende methodes aan: `deleteDownloadIfPresent`, `deleteHubsIfPresent`, `cloneRepo`, `renameHubsDockerFile` en een loop van 1^n modules die `initiateProcess` aanroept.
- `deleteDownloadIfPresent`: verwijdert de downloadfolder als die aanwezig is
- `deleteHubsIfPresent`: verwijdert de hubsfolder als die aanwezig is
- `cloneRepo`: clonet de Mozilla Hubs repository van `TophoStan/hubs`
- `renameHubsDockerFile`: hernoemt `RetPageOriginDockerFile` naar `Dockerfile`
- `initiateProcess`: roept sequentieel de volgende functies aan: `checkForModule`, `checkForVersions`, `checkForFiles`, `downloadFiles` en `applyModulesInfo`.
- `checkForModule`: Kijkt of de meegegeven module bestaat
- `checkForVersions`: Kijkt welke versies er bestaan en of de opgegeven versie bestaat
- `checkForFiles`: Kijkt welke bestanden bij de module horen
- `downloadFiles`: Download recursief de daadwerkelijke bestanden die in de folders van de module staan
- `applyModulesInfo`: leest `moduleInfo.json` uit en past deze toe, meer info [[Module inhoud]]
- `buildDockerImage`: bouwt Mozilla Hubs naar een Docker Image, meer info [[dockerBuild]]

### `validate`
Deze endpoint is een `POST` endpoint met de volgende functies
- `getoAuth2Client`: Client die gebruikt moet worden voor het authentiseren van Google Drive
- `checkForModule`: Kijkt of de meegegeven module bestaat
- `checkForVersions`: Kijkt welke versies er bestaan en of de opgegeven versie bestaat
- `checkForFiles`: Kijkt welke bestanden bij de module horen

### `/`
Deze endpoint is een `POST` endpoint met de volgende functies
- `getoAuth2Client`: Client die gebruikt moet worden voor het authentiseren van Google Drive
In de `POST` functie zelf worden ook bestanden opgehaald en teruggegeven als lijst
