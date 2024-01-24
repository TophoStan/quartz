## Google Cloud platform
Ga naar console.cloud.google.com

IAM & ADMIN -> Service Accounts
![[Pasted image 20240124110532.png]]


Create service account
Vul de gegevens in
![[Pasted image 20240124110608.png]]

Geef het de rol `Kubernetes Engine Service Agent` of `Compute engine Service Agent`
![[Pasted image 20240124110700.png]]

Done

Vervolgens naar de keys van het net aangemaakte service acount

![[Pasted image 20240124111208.png]]

Add key

![[Pasted image 20240124111230.png]]

Bewaar het gedownloade JSON bestand goed!

## Microsoft Azure

`az login`
Sla de `id` waarde ergens op 
![[Pasted image 20240124100705.png]]


`az ad sp create-for-rbac --name <app-name> --role "Application Administrator"`
Bewaar de output
![[Pasted image 20240124100742.png]]



De laatste stap
`az role assignment create --assignee "<APPID>" --role Contributor --scope /subscriptions/<SUBSCRIPTIONID>/resourceGroups/<RESOURCEGROUPNAAM>`

Maak uiteindelijk een JSON-bestand met daarin
`appId`: het id van je app
`password`:  het wachtwoord van je service account
`tenant`: het id van je organisatie
`subscriptionId`: het Id van je abbonement

```JSON
{                                                        
  "appId": "628934ui23ghf789wsdyfdsaf",       
  "password": "N0~8Q~asldjfhiasdluhflasdhfliasdf",
  "tenant": "ea538eea-68fd-ausdhfyu8asdfg234287tfsda-",
  "subscriptionId": "asuiy9v8sd-bd88-sdfygihsdabgiusd-a04f-jjjjjj"       
}                                                        
```


