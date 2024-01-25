Een heel simpel bestand waarmee met de `dockerode` npm-library een Docker Image gebouwd wordt. 

De naam van de Docker Image wordt in `IMAGE_NAME` gezet en krijgt een unieke naam. Namelijk `hubs_DAG_MAAND_JAAR_UUR_SECONDEN_MILLISECONDEN(afgerond naar 2 cijfers)`.


```ts
 const docker = new Dockerode();
    const stream = await docker.buildImage({
        context: `${process.cwd()}/hubs`,
        src: ["Dockerfile", "package.json", "package-lock.json", "tsconfig.json", "src", "types", "admin", "scripts/docker/nginx.config", "scripts/docker/run.sh", "webpack.config.js", "babel.config.js"]
    }, {
        t: `${IMAGE_NAME}:dev`
    });

    await new Promise((resolve, reject) => {
        docker.modem.followProgress(stream, (err, res) => {
            if (err) {
                reject(err);
            } else {
                resolve(res);
            }
        }, (event) => {
            console.log(event);
        });
    });
```

in de `src` van de `docker.buildImage` staan de bestanden die nodig zijn voor het builden van Mozilla Hubs. Het builden wordt gedaan met `webpack`, zoals te zien is in de `package.json`.