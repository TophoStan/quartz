---
tag: XR, VR
---

Hubs is een gereedschap waarmee virtuele ruimtes mee gecreëerd kunnen worden in je browser die [[VR (Virtual Reality)]] en [[XR (Extended Reality)]] ondersteund. Er is geen app-store of installatie vereist om hiervan gebruik te kunnen maken. Gebaseerd op WebXR standaarden

In Hubs kan een Room aangemaakt worden
Een Room is een eigen virtuele ruimte.

Voor het betreden van een Room, kom je in de Lobby van de Room
Vanaf de Lobby kan je de Room betreden

In een Room kan je bewegen door middel van de standaard movement keys WASD


**Links**
[https://hubs.mozilla.com/](https://hubs.mozilla.com/ "https://hubs.mozilla.com/")
[https://hubs.mozilla.com/demo](https://hubs.mozilla.com/demo "https://hubs.mozilla.com/demo")
[https://hubs.mozilla.com/cloud](https://hubs.mozilla.com/cloud "https://hubs.mozilla.com/cloud")
[https://hubs.mozilla.com/spoke](https://hubs.mozilla.com/spoke "https://hubs.mozilla.com/spoke") 
[https://github.com/mozilla/hubs](https://github.com/mozilla/hubs "https://github.com/mozilla/hubs") 
[https://hubs.mozilla.com/labs/](https://hubs.mozilla.com/labs/ "https://hubs.mozilla.com/labs/") 
[https://www.youtube.com/@MozillaHubs](https://www.youtube.com/@MozillaHubs "https://www.youtube.com/@MozillaHubs") 

**Tutorial Nigel**
[https://www.youtube.com/playlist?list=PLk58K5q-RaTGRqe3pfzjEkujSl_OUh-iX](https://www.youtube.com/playlist?list=PLk58K5q-RaTGRqe3pfzjEkujSl_OUh-iX "https://www.youtube.com/playlist?list=PLk58K5q-RaTGRqe3pfzjEkujSl_OUh-iX") 
## **Technische aspecten**
#### **Client** 
De Frontend van Mozilla Hubs is gebouwd op JS libraries React voor 2D componenten en Three.js and A-Frame voor 3D objecten. Logica wordt gedaan op de client door [[Ammo.JSwasm]] . Pagina's worden verstuurd vanuit de backend met [[Reticulum]]. Daarnaast is de UI in de Rooms bijvoorbeeld met [[React]] gemaakt.

#### [[Habitat]] 
Mozilla Hubs maakt gebruik van Chef Habitat voor deployments. Habitat draait namelijk op AWS EC2 instanties. En Habitat draait op zijn beurt dus packages zoals Hubs, Reticulum of Dialog.

#### [Dialog](Dialog/Dialog.md)
Dialog is een WebRTC SFU voor Hubs

#### [[Postgres DB]]
PostgresDB wordt gebruikt om de data te persisteren. Het is tevens de database van Reticulum en een file store.

![[Pasted image 20230830103723.png]] 