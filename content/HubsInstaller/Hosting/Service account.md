## Google Cloud platform


## Microsoft Azure

`az login`
Sla de `id` waarde ergens op 
![[Pasted image 20240124100705.png]]


`az ad sp create-for-rbac --name <app-name> --role "Application Administrator"`
Bewaar de output
![[Pasted image 20240124100742.png]]

Maak uiteindelijk een JSON-bestand met daarin
`appId`: het id van je app
`password`:  het wachtwoord van je service account
`tenant`: het id van je organisatie
`subscriptionId`: het Id van je abbonement

De laatste stap
`az role assignment create --assignee "<APPID>" --role Contributor --scope /subscriptions/<SUBSCRIPTIONID>/resourceGroups/<RESOURCEGROUPNAAM>`



