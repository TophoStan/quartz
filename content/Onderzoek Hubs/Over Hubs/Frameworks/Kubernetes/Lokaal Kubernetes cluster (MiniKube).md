Het is mogelijk om lokaal een Kubernetes cluster te draaien met behulp van Minikube.
Voordat je begint, moet je het volgende weten:
- Docker moet al geïnstalleerd zijn, want daar draait minikube op
- De chocolaty installer moet aanwezig om het op een windows apparaat te kunnen installeren
- De Kubernetes CLI moet geïnstalleerd zijn (`kubectl`)
- Gebruik op windows het Git Bash terminal, want WSL kan niet bij het `.kube`-bestand


1. Download Minikube `choco install minikube`
2. `Minikube start`
3. `Minikube tunnel` LET OP, deze terminal mag niet afgesloten worden
4. `kubectl config get-contexts` 
	Resulteert in dit scherm
	   ![[Pasted image 20231120153031.png]]
	   
	Staat er geen `*` voor minikube `kubectl config set-context minikube`
5. kubectl get pods om te kijken of het werkt

Je hebt nu een draaiend lokaal cluster!

