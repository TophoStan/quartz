

### Community Edition
Wanneer Community edition een update krijgt, kijk goed naar wat er geupdate wordt. Is `hcce.yam` veranderd, vervang `deployment.template.yam` daarmee. De code is zo geschreven dat het `deployment.template.yam` verwacht in `src/app/api/lib/`. Na het hernoemen en het zetten op de goede bestandslocatie van het bestand hoeft er niks veranderd te worden aan de inhoud van het bestand. De werking hiervan kan in [[yamlTemplateFunctions]] bestudeerd worden. 

### Hubs client
Houd de Hubs client bij, net als Community Edition wordt deze nog steeds bijgewerkt en is mijn implementatie van het aan- en uitzetten van Hubs features niet gebaseerd op bitECS. 