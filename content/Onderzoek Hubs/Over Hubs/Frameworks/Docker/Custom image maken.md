
Kubernetes maakt gebruik van Docker images om containers aan te maken. Een image wordt vervolgens in een pod gestopt om en deze pod is dan te bereiken via een ip-adres indien een service aanwezig is die naar de pod kan wijzen. Voor het maken van een custom hubs client is het dus nodig om een eigen image te maken. Een image is onderdeel van een deployment. Een deployment kan er als volgt uitzien

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pgbouncer
  namespace: hcce
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pgbouncer
  minReadySeconds: 2
  template:
    metadata:
      labels:
        app: pgbouncer
    spec:
      containers:
      - image: mozillareality/pgbouncer:stable-latest
        imagePullPolicy: IfNotPresent
        name: pgbouncer
        env:
        - name: MAX_CLIENT_CONN
          value: "10000"
        - name: DB_USER
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_USER
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_PASS
        - name: DB_HOST
          value: pgsql
```
in spec->template->spec->containers->image staan de volgende twee regels:
```
  - image: mozillareality/pgbouncer:stable-latest
	imagePullPolicy: IfNotPresent
```
Als image staat hier een door Mozzilla onderhouden Docker repository met daarin `pgbouncer:stable-latest`. Je kan hier dus kiezen om je eigen repository toe te passen door deze regel aan te passen. Hiervoor moet je Docker container online beschikbaar staan op een Container Registry, dit kan op Docker zelf maar ook op Azure of andere platform. Een voorbeeld van een eigen image kan dus zijn
`stantop/hubs-client:v1` of `boldlyxr/hubs-client-efteling:v1`. Na de `:` komt de tag. Deze kan je gebruiken voor onder andere versiebeheer, lokale mogelijkheden of ontwikkeldoeleinden. 

### Hoe maak je een custom image
Een custom image is gewoon een image. Dus wat dat betekent is dat je `docker build -t boldlyxr/hubs-client-efteling:dev -f <LOCATIE VAN DOCKERFILE> .` Vergeet de `.` niet. Deze geeft de context aan waar het Dockerfile op toegepast moet worden.  
#### Hoe ziet een Dockerfile er uit?
Een Dockerfile kan er als volgt uit zien:
```Dockerfile
###
# this dockerfile produces image/container that serves customly packaged hubs and admin static files
# the result container should serve reticulum as "hubs_page_origin" and "admin_page_origin" on (path) "/hubs/pages"
###
from node:16.16 as builder
run mkdir -p /hubs/admin/ && cd /hubs
copy package.json ./
copy package-lock.json ./
run npm ci
copy admin/package.json admin/
copy admin/package-lock.json admin/
run cd admin && npm ci --legacy-peer-deps && cd ..
copy . .
env BASE_ASSETS_PATH="{{rawhubs-base-assets-path}}"
run npm run build 1> /dev/null
run cd admin && npm run build 1> /dev/null && cp -R dist/* ../dist && cd ..
run mkdir -p dist/pages && mv dist/*.html dist/pages && mv dist/hub.service.js dist/pages && mv dist/schema.toml dist/pages          
run mkdir /hubs/rawhubs && mv dist/pages /hubs/rawhubs && mv dist/assets /hubs/rawhubs && mv dist/favicon.ico /hubs/rawhubs/pages

from alpine/openssl as ssl
run mkdir /ssl && openssl req -x509 -newkey rsa:2048 -sha256 -days 36500 -nodes -keyout /ssl/key -out /ssl/cert -subj '/CN=hubs'

from nginx:alpine
run apk add bash
run mkdir /ssl && mkdir -p /www/hubs && mkdir -p /www/hubs/pages && mkdir -p /www/hubs/assets
copy --from=ssl /ssl /ssl
copy --from=builder /hubs/rawhubs/pages /www/hubs/pages
copy --from=builder /hubs/rawhubs/assets /www/hubs/assets
copy scripts/docker/nginx.config /etc/nginx/conf.d/default.conf
copy scripts/docker/run.sh /run.sh
run chmod +x /run.sh && cat /run.sh
cmd bash /run.sh
```

Een Dockerfile bevat instructies over hoe een Docker applicatie gebuild en opgestart kan worden. Het bovenstaande voorbeeld is een Dockerfile dat door Mozilla Hubs wordt geleverd.  

Wanneer je deze uitvoert met het volgende commando:
`docker build -t boldlyxr/hubs-client-efteling:dev -f <LOCATIE VAN DOCKERFILE> .` Krijg je dus een image op boldlyxr/hubs-client. Nu staat deze lokaal, maar je wilt hem natuurlijk online hebben staan. Dit betekent dat je bij een container register zoals DockerHub een account moet maken genaamd `boldlyxr` als je nog geen account had. `boldlyxr` kan door elke willekeurige naam veranderd worden zolang je over dat account beschikt. 

#### Hoe zet ik mijn docker image online
Het commando `docker push boldlyxr/hubs-client-efteling:dev` pusht je image naar het huidig geselecteerde container register (standaard is dit DockerHub). 
Vervolgens kan je dit checken door op je containerregister te kijken zoals
https://hub.docker.com/
![[Pasted image 20231120150738.png]]

