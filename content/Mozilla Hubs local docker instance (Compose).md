
**Pre-installatie vereisten**
	- Docker desktop


Met behulp van docker compose is het mogelijk om simpel een cluster van verschillende applicaties binnen seconden te hebben opgestart.

Compose.yml
```yml
version: '1'
services:

  db:
    image: postgres:14-alpine
    environment:
      POSTGRES_DB: ret_dev
      POSTGRES_PASSWORD: postgres
      POSTGRES_USER: postgres
    restart: always
    user: postgres
    networks:
      - localhubs
    healthcheck:
      test: [ "CMD-SHELL", "pg_isready" ]
      interval: 10s
      timeout: 5s
      retries: 5

  hubs:
    image: stantop/local-hubs:latest
    environment:
      RETICULUM_SERVER: localhost:4000
      HOST_IP: localhost
      INTERNAL_HOSTNAME: hubs
    ports:
      - "8080:8080"
    depends_on:
      - app
    networks:
      - localhubs
    command: "npm run local"
    volumes:
      - .:/app/storage/dev

  dialog:
    image: stantop/local-dialog
    ports:
      - "4443:4443"
    depends_on:
      db:
        condition: service_healthy
    networks:
      - localhubs
    command: "node index.js"
    volumes:
      - .:/dialog/storage/dev

  spoke:
    image: stantop/local-spoke
    environment:
      CORS_PROXY_SERVER: "localhost:4000"
      INTERNAL_HOSTNAME: spoke
    ports:
      - "9090:9090"
    networks:
      - localhubs
    command: "yarn run-local-reticulum"
    volumes:
      - .:/app/storage/dev

  app:
    image: stantop/local-reticulum
    ports:
      - "4000:4000"
    depends_on:
      db:
        condition: service_healthy
    environment:
      HUBS_ADMIN_INTERNAL_HOSTNAME: admin
      HUBS_CLIENT_INTERNAL_HOSTNAME: hubs
      POSTGREST_INTERNAL_HOSTNAME: postgrest
      SPOKE_INTERNAL_HOSTNAME: spoke
    networks:
      - localhubs
    command: "mix phx.server"
    volumes:
      - .:/app/storage/dev

  admin:
    image: stantop/local-hubs:latest
    environment:
      INTERNAL_HOSTNAME: admin
      POSTGREST_SERVER: postgrest
    ports:
      - "8989:8989"
    networks:
      - localhubs
    command: sh -c "cd admin && npm run local"

  postgrest:
    image: stantop/local-postgrest:latest
    ports:
      - "3000:3000"
    networks:
      - localhubs

networks:
  localhubs:
```

Navigeer naar de directory van ``Compose.yml`` 
```shell
Docker compose up -d
```
Tijdens het opstarten van de containers kan je alvast het certificaat installeren van localhost voor SSL/HTTPS ondersteuning. Het certificaat is te vinden in elk project, maar je kan hem via deze [link](https://github.com/TophoStan/hubs/blob/master/ssl/localhost.pem)ook downloaden. Installeer deze in de root trust.


Navigeer naar  ``https://localhost:4000``
Sign vervolgens in met een email account naar keuze
In de logs van ``Reticulum-app-1`` moet je op de authenticatie link klikken
Nu is je account geregistreerd!

In de terminal van ``Reticulum-app-1``  
```shell
iex -S mix  # Op deze wijze verbind je met de huidige draaiende instantie Reticulum
```
Maak hiermee het als eerst geregistreerde gebruikers account een admin (jouw account dus)
``` Elixir
Ret.Account |> Ret.Repo.all() |> Enum.at(0) |> Ecto.Changeset.change(is_admin: true) |> Ret.Repo.update!()
```

Het volgende commando zorgt dat de authenticatie tussen Reticulum en Postgrest werkt met behulp van een Jason Web Key

``` Elixir
jwk = Application.get_env(:ret, Ret.PermsToken)[:perms_key] |> JOSE.JWK.from_pem(); JOSE.JWK.to_file("reticulum-jwk.json", jwk)
```

**Limitaties lokale instantie van Mozilla Hubs**
Het importeren van scenes die je lokaal hebt gemaakt is niet mogelijk
Het importeren van iets kan alleen met een http**s** verbinding