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
	   
	(Staat er **geen** `*` voor minikube `kubectl config set-context minikube`)
5. `kubectl get pods` om te kijken of je een valide configuratie hebt gemaakt!

Je hebt nu een draaiend lokaal cluster!

### Lokaal Community Edition draaien
Nu heb je een lokaal cluster, maar niks er op draaien. Mozilla Hubs Community Edition kan ook draaien op je lokale cluster. Hieronder volgt het stappenplan om het op te zetten.

1. Download de bestanden van de [Github Repository](https://github.com/mozilla/hubs-cloud/tree/master) van Hubs
2. Open het bestand in een code editor, VS (code) wordt gebruikt in dit voorbeeld
Vervolgens heb je de volgende bestanden

![[Pasted image 20231201093702.png]]

In `/community-edition` zitten diverse bestanden.
De belangrijkste twee zijn: `render_hcce.sh` en `hcce.yam`. 

`render_hcce.sh` bevat een script als het ware, waarin je variabelen mee kan geven. Het standaard bestand ziet er zo uit
``` sh

bins=("bash" "openssl" "npm")
for cmd in "${bins[@]}"; do
    if ! command -v $cmd &> /dev/null; then
        echo "missing required binary: $cmd"
        return 1
    fi
done

if ! npm list -g pem-jwk | grep -q pem-jwk; then
    echo "missing required npm pkg: pem-jwk, try (sudo) npm install pem-jwk -g to install it"
    return 1
fi

### required
export HUB_DOMAIN="example.net"
export ADM_EMAIL="admin@example.net"

export Namespace="hcce"

export DB_USER="postgres"
export DB_PASS="123456"
export DB_NAME="retdb"
export DB_HOST="pgbouncer"
export DB_HOST_T="pgbouncer-t"
export PGRST_DB_URI="postgres://$DB_USER:$DB_PASS@pgbouncer/$DB_NAME"
export PSQL="postgres://$DB_USER:$DB_PASS@pgbouncer/$DB_NAME"

export SMTP_SERVER="changeMe"
export SMTP_PORT="changeMe"
export SMTP_USER="changeMe"
export SMTP_PASS="changeMe"

export NODE_COOKIE="changeMe"
export GUARDIAN_KEY="changeMe"
export PHX_KEY="changeMe"

export SKETCHFAB_API_KEY="?"
export TENOR_API_KEY="?"

openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
export PERMS_KEY="$(echo -n "$(awk '{printf "%s\\\\n", $0}' private_key.pem)")"
openssl ec -pubout -in private_key.pem -out public_key.pem
#sudo apt-get install npm && sudo npm install -g pem-jwk
export PGRST_JWT_SECRET=$(pem-jwk public_key.pem)

### initial cert
openssl req -x509 -newkey rsa:2048 -sha256 -days 36500 -nodes -keyout key.pem -out cert.pem -subj '/CN='$HUB_DOMAIN
export initCert=$(base64 -i cert.pem | tr -d '\n')
export initKey=$(base64 -i key.pem | tr -d '\n')

envsubst < "hcce.yam" > "hcce.yaml"
```

Alleen om op Localhost te draaien heb je een net iets andere configuratie nodig. Vandaar dat ik mijn eigen draai eraan heb gegeven door een conditie toe te voegen wanneer `HUB_DOMAIN` gelijk is `localhost` . 

``` sh
bins=("bash" "openssl" "npm")
for cmd in "${bins[@]}"; do
    if ! command -v $cmd &> /dev/null; then
        echo "missing required binary: $cmd"
        return 1
    fi
done

if ! npm list -g pem-jwk | grep -q pem-jwk; then
    echo "missing required npm pkg: pem-jwk, try (sudo) npm install pem-jwk -g to install it"
    return 1
fi


### required
export HUB_DOMAIN="localhost"
export ADM_EMAIL="stantophoven2004@outlook.com"


### MAY NEVER BE "default"
export Namespace="nexus"

export DB_USER="postgres"
export DB_PASS="123456"
export DB_NAME="retdb"
export DB_HOST="pgbouncer"
export DB_HOST_T="pgbouncer-t"
export PGRST_DB_URI="postgres://$DB_USER:$DB_PASS@pgbouncer/$DB_NAME"
export PSQL="postgres://$DB_USER:$DB_PASS@pgbouncer/$DB_NAME"

export SMTP_SERVER="smtp-relay.brevo.com"
export SMTP_PORT="587"
export SMTP_USER="stophoven@boldly-xr.com"
export SMTP_PASS="UY7TXQF5wOWLESr3"

export NODE_COOKIE="changeMe"
export GUARDIAN_KEY="changeMe"
export PHX_KEY="changeMe"

export SKETCHFAB_API_KEY="?"
export TENOR_API_KEY="?"

openssl genpkey -algorithm RSA -out ssl/private_key.pem -pkeyopt rsa_keygen_bits:2048
export PERMS_KEY="$(echo -n "$(awk '{printf "%s\\\\n", $0}' ssl/private_key.pem)")"
openssl ec -pubout -in ssl/private_key.pem -out ssl/public_key.pem
#sudo apt-get install npm && sudo npm install -g pem-jwk
export PGRST_JWT_SECRET=$(pem-jwk ssl/public_key.pem)

### initial cert

cd $(pwd)/ssl

mkcert $HUB_DOMAIN
export initCert=$(base64 -i $HUB_DOMAIN.pem | tr -d '\n')
export initKey=$(base64 -i $HUB_DOMAIN-key.pem | tr -d '\n')
#If localhost is used, generate localhost certs and use hcce_localhost.yam instead of hcce.yam

if [ "$HUB_DOMAIN" = "localhost" ]; then {
        mkcert assets.localhost
        mkcert stream.localhost
        mkcert cors.localhost
        
        
        export localhostAssetsCert=$(base64 -i assets.localhost.pem | tr -d '\n')
        export localhostAssetsKey=$(base64 -i assets.localhost-key.pem | tr -d '\n')
        export localhostStreamCert=$(base64 -i stream.localhost.pem | tr -d '\n')
        export localhostStreamKey=$(base64 -i stream.localhost-key.pem | tr -d '\n')
        export localhostCorsCert=$(base64 -i cors.localhost.pem | tr -d '\n')
        export localhostCorsKey=$(base64 -i cors.localhost-key.pem | tr -d '\n')
        cd ..
        envsubst < "localhost_hcce.yam" > "localhost_hcce.yaml"
        
        } else {
        
        envsubst < "hcce.yam" > "hcce.yaml"           
        
    }
    
fi

```


3. Vervang het standaard bestand met het `.sh`-bestand dat hierboven staat.
4. Download het het bestand `localhost_hcce.yam` van deze [Github repo]() af

#### BELANGRIJK!
Installeer `pem-jwk` met `npm install -g pem-jwk`
Installeer `mkcert` met `choco install mkcert`

