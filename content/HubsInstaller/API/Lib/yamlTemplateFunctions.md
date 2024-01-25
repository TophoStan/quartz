Dit bestand bevat functies voor het inlezen van het `deployment.template.YAM`. Na het inlezen worden default waardes meegegeven aan de variabelen wanneer deze leeg waren. Als er custom images gebruikt worden voor de deployment, worden deze apart vervangen in de `replaceImages` methode. Na het vervangen van de images, worden variabelen die in het bestand staan veranderd. De syntax van de variabelen wordt daarmee aangepast namelijk van `$NAMESPACE` naar `{{NAMESPACE}}`. Dit is nodig voor de library `handlebars`.

```ts
yamlWithCorrectImages.replace(/\$([a-zA-Z_]\w*)/g, '{{$1}}')
```



``` ts
    let data = {
        HUB_DOMAIN: variables.HUB_DOMAIN,
        Namespace: variables.Namespace,
        ADM_EMAIL: variables.ADM_EMAIL,
        DB_USER: variables.DB_USER,
        DB_PASS: variables.DB_PASS,
        DB_NAME: variables.DB_NAME,
        DB_HOST: variables.DB_HOST,
        DB_HOST_T: variables.DB_HOST_T,
        PGRST_DB_URI: variables.PGRST_DB_URI,
        PSQL: variables.PSQL,
        SMTP_SERVER: variables.SMTP_SERVER,
        SMTP_PORT: variables.SMTP_PORT,
        SMTP_USER: variables.SMTP_USER,
        SMTP_PASS: variables.SMTP_PASS,
        initCert: "3h2RWIzZnJ1RmwwCjRkNngxanZRWklIR0s2NwUmVCeHlWZXBhZTN6UnE5NFg1VHA4UwpUcE9QMzVCeTRvTzdmZmFGanVJSGpCbXZkRm1oUkhKTE80NmkzVzNKbmczVU85amdUZXlrdXkyOFQwM09qUkMvClRoUT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=",
        initKey: "zZhOEZsUHZZUXFURzYyUlZ0UUNOVXg1abVk2Q1lrbkMvRGhRNmtjUTFhcnd6eHNFOGE3L3JYSUdNOEN6dTdmNWYyS1hmcjdwQXArSmlCaWhydmVJVnMKNk0xZTd6enZZRDFIcEFzUzlvTDNRa2h4bmc9PQotLS0tLUVORCBQUklWQVRFIEtFWS0tLS0tCg==",
        PGRST_JWT_SECRET: '{"kty":"RSA","n":"uFXP_Vd35BAEs11XTlkxBIY84FFPCpY8rz-zcSW9GmsGCLsJdZupWsKyXLyW3oHdmXDIoOan5LWrI455ZUnrIQ2EjTJ1owcvSyeYBZ2Fc0tyf3JxE8kHf9dD4w8abczojDn0gSfXuyloxxDnTt50kKx2QVIg3Le4Jzbsxmg1G6RwKKN-Mg3PwrLvAvV7MOfwvSUelKoEEh7WSaqHnTfMtD502bTQ93Kof7n-fr42PUIsWrXsJ_WqPE2bZXdKOM8T3tUe7c-voITZAB7IbCZFwXBa1AVBo8QaUy6ci9C8R6ZTVFo7Xqbv8p8OfZFHk_yTqlSwiBKvDPymBlZPUZMRZw","e":"AQAB"}',

        reticulumImage: variables.reticulumImage,
        postgrestImage: variables.postgrestImage,
        pgsqlImage: variables.pgsqlImage,
        pgbouncerImage: variables.pgbouncerImage,
        pgbouncerTImage: variables.pgbouncerTImage,
        hubsImage: variables.hubsImage,
        spokeImage: variables.spokeImage,
        nearSparkImage: variables.nearSparkImage,
        speelycaptorImage: variables.speelycaptorImage,
        photomnemonicImage: variables.photomnemonicImage,
        dialogImage: variables.dialogImage,
        coturnImage: variables.coturnImage,
    }

    data.reticulumImage = data.reticulumImage || "mozillareality/ret:stable-latest";
    data.postgrestImage = data.postgrestImage || "mozillareality/postgrest:stable-latest";
    data.pgsqlImage = data.pgsqlImage || "postgres:12";
    data.pgbouncerImage = data.pgbouncerImage || "mozillareality/pgbouncer:stable-latest";
    data.pgbouncerTImage = data.pgbouncerTImage || "mozillareality/pgbouncer:stable-latest";
    data.hubsImage = data.hubsImage || "mozillareality/hubs:stable-latest";
    data.spokeImage = data.spokeImage || "mozillareality/spoke:stable-latest";
    data.nearSparkImage = data.nearSparkImage || "mozillareality/nearspark:stable-latest";
    data.speelycaptorImage = data.speelycaptorImage || "mozillareality/speelycaptor:stable-latest";
    data.photomnemonicImage = data.photomnemonicImage || "mozillareality/photomnemonic:stable-latest";
    data.dialogImage = data.dialogImage || "mozillareality/dialog:stable-latest";
    data.coturnImage = data.coturnImage || "mozillareality/coturn:stable-latest";
```


Na het vervangen van alle variabelen in het bestand, wordt het ingelezen bestand dat omgezet was naar een `string` omgezet naar een `unknown[]` waarin de Kubernetes objecten in zitten.