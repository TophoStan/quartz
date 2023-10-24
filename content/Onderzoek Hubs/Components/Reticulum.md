#hubs
# **Reticulum Configuration in Kubernetes**

This Kubernetes YAML configuration defines the deployment of the Reticulum application in the "hcce" namespace. Reticulum is a service that serves as an integral part of your infrastructure, and this configuration file provides various settings for it.

### ConfigMap (ret-config)

The "ret-config" ConfigMap contains a wide range of configuration parameters for the Reticulum application, including:

- Peerage and DNS settings.
- Discord integration credentials.
- SSL settings for HTTPS.
- Database connection details and pool settings.
- Integration with third-party services.
- Email settings and CORS configurations.
- Environment variables for various configurations.

### Deployment (reticulum)

This section defines the deployment of the Reticulum application using the ConfigMap created earlier. Key deployment details include:

- The use of hostPath for storing data and configuration files.
- Environment variables populated with secrets from the "configs" ConfigMap.
- Probes for liveness and readiness to ensure application health.
- A PostgREST container, which serves as a RESTful API for the PostgreSQL database.
- Integration with PostgreSQL and JWT settings.

#### Environment Variables

The Reticulum Deployment relies on a variety of environment variables for its configuration. These include:

- Database connection details such as `turkeyCfg_DB_USER`, `turkeyCfg_DB_PASS`, `turkeyCfg_DB_NAME`, and more.
- Security settings like `turkeyCfg_GUARDIAN_KEY` and `turkeyCfg_PERMS_KEY`.
- Integration with third-party services through variables like `turkeyCfg_SKETCHFAB_API_KEY` and `turkeyCfg_TENOR_API_KEY`.
- Email settings and other application-specific configurations.

### Service (ret)

This Service resource exposes the Reticulum application within the Kubernetes cluster. It defines ports for both HTTP and HTTPS access to the Reticulum application, and it selects pods labeled with "app: reticulum."

Make sure to populate the placeholders in the ConfigMap with the appropriate values and secrets.

For more information on Reticulum, refer to the [Reticulum documentation](https://example.com/reticulum-docs).

Feel free to reach out for further details or assistance.

---

``` YAML
##############################################################################################
############################################ reticulum #######################################
##############################################################################################
apiVersion: v1
kind: ConfigMap
metadata:
  name: ret-config
  namespace: hcce
data:
  config.toml.template: |
    [peerage]
    dns_name = "ret.<POD_NS>.svc.cluster.local"
    app_name = "ret"

    [ret."Elixir.Ret"]
    pool = "hubs"

    [ret."Elixir.RetWeb.Plugs.DashboardHeaderAuthorization"]
    dashboard_access_key = "<DASHBOARD_ACCESS_KEY>"

    [ret."Elixir.Ret.DiscordClient"]
    client_id = ""
    client_secret = ""
    bot_token = ""

    [ret."Elixir.RetWeb.Endpoint".https]
    port = 4000
    certfile = "/ret/cert.pem"
    cacertfile = "/ret/cacert.pem"
    keyfile = "/ret/key.pem"

    [ret."Elixir.RetWeb.Endpoint"]
    allowed_origins = "*"
    secret_key_base = "<PHX_KEY>"
    allow_crawlers = true

    [ret."Elixir.RetWeb.Endpoint".secondary_url]

    [ret."Elixir.RetWeb.Endpoint".cors_proxy_url]
    # host = "cors.<DOMAIN>"
    host = "hubs-proxy.com"  
    port = 443

    [ret."Elixir.RetWeb.Endpoint".imgproxy_url]
    host = "<IMG_PROXY>"
    port = 5000

    [ret."Elixir.RetWeb.Endpoint".assets_url]
    host = "assets.<DOMAIN>"
    port = 443

    [ret."Elixir.RetWeb.Endpoint".link_url]
    host = "hubs-link.local"

    [ret."Elixir.RetWeb.Endpoint".url]
    host = "<HUB_DOMAIN>"
    port = 443

    [ret."Elixir.RetWeb.Endpoint".static_url]
    host = "<HUB_DOMAIN>"

    [ret."Elixir.Ret.Repo"]
    username = "<DB_USER>"
    password = "<DB_PASS>"
    database = "<DB_NAME>"
    hostname = "<DB_HOST_T>"
    template = "template0"
    pool_size = 10
    port = 5432

    [ret."Elixir.Ret.SessionLockRepo"]
    username = "<DB_USER>"
    password = "<DB_PASS>"
    database = "<DB_NAME>"
    hostname = "<DB_HOST>"
    template = "template0"

    port = 5432

    [ret."Elixir.Ret.Locking".session_lock_db]
    username = "<DB_USER>"
    password = "<DB_PASS>"
    database = "<DB_NAME>"
    hostname = "<DB_HOST>"
    port = 5432

    [ret."Elixir.Ret.Habitat"]
    ip = "127.0.0.1"
    http_port = 9631

    [ret."Elixir.Ret.JanusLoadStatus"]
    default_janus_host = "stream.<DOMAIN>"
    janus_service_name = ""
    janus_admin_secret = ""
    janus_admin_port = 7000
    janus_port = 4443

    [ret."Elixir.Ret.Guardian"]
    secret_key = "<GUARDIAN_KEY>"
    issuer = "<HUB_DOMAIN>"

    [ret."Elixir.Ret.PermsToken"]
    perms_key = "<PERMS_KEY>"

    [ret."Elixir.Ret.OAuthToken"]
    oauth_token_key = ""

    [ret]
    bot_access_key = ""
    # pgrest_host = ""
    # ita_host = ""

    [ret."Elixir.Ret.MediaResolver"]
    ytdl_host = "<YTDL_HOST>"
    photomnemonic_endpoint = "<PHOTOMNEMONIC>"
    sketchfab_api_key = "<SKETCHFAB_API_KEY>"
    tenor_api_key = "<TENOR_API_KEY>"

    [ret."Elixir.Ret.Speelycaptor"]
    speelycaptor_endpoint = "<SPEELYCAPTOR>"

    [ret."Elixir.Ret.PageOriginWarmer"]
    hubs_page_origin = "https://hubs.<POD_NS>:8080/hubs/pages"
    spoke_page_origin = "https://spoke.<POD_NS>:8080/spoke/pages"
    admin_page_origin = "https://hubs.<POD_NS>:8080/hubs/pages"
    insecure_ssl = true

    [ret."Elixir.Ret.HttpUtils"]
    insecure_ssl = true

    [ret."Elixir.Ret.Storage"]
    storage_path = "/storage"
    ttl = 172800
    host = "https://<HUB_DOMAIN>"
    quota_gb = "<STORAGE_QUOTA_GB>" # example: "12"
    # ^^^ has to be string or elixir throws (ArgumentError) argument error:erlang.byte_size(#), but why

    [ret."Elixir.RetWeb.Email"]
    from = "noreply@<HUB_DOMAIN>"

    [ret."Elixir.Ret.Mailer"]
    server = "<SMTP_SERVER>"
    port = "<SMTP_PORT>"
    username = "<SMTP_USER>"
    password = "<SMTP_PASS>"

    [ret."Elixir.Ret.Support"]
    slack_webhook_url = "<SLACK_WEBHOOK>"

    [ret."Elixir.RetWeb.Plugs.AddCSP"]
    child_src = ""
    connect_src = "wss://*.stream.<DOMAIN>:4443"
    font_src = ""
    form_action = ""
    frame_src = ""
    img_src = "nearspark.reticulum.io"
    manifest_src = ""
    media_src = ""
    script_src = ""
    style_src = ""
    worker_src = ""

    [ret."Ret.Repo.Migrations.AdminSchemaInit"]
    postgrest_password = ""

    [ret."Elixir.Ret.StatsJob"]

    [ret."Elixir.RetWeb.HealthController"]

    [ret."Elixir.RetWeb.PageController"]
    skip_cache = false
    extra_avatar_headers = ""
    extra_index_headers = ""
    extra_room_headers = ""
    extra_scene_headers = ""

    extra_avatar_html = ""
    extra_index_html = ""
    extra_room_html = ""
    extra_scene_html = ""

    extra_avatar_script = ""
    extra_index_script = ""
    extra_room_script = ""
    extra_scene_script = ""

    [ret."Elixir.Ret.Account"]
    admin_email = "<ADM_EMAIL>"

    [ret."Elixir.Ret.Coturn"]
    realm = "turkey"
    public_tls_ports = "5349"

    [web_push_encryption.vapid_details]
    subject = ""
    public_key = ""
    private_key = ""

    [sentry]
    dsn = "<SENTRY_DSN>"

    [run]
    hostname_dns_suffix = "turkey"

    [hackney]
    max_connections = 250

    [ret."Elixir.Ret.Meta"]
    phx_host = "<HUB_DOMAIN>"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reticulum
  namespace: hcce
  annotations:
    cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
spec:
  replicas: 1
  selector:
    matchLabels:
      app: reticulum
  minReadySeconds: 15
  strategy:
    type: RollingUpdate
    rollingUpdate: 
      maxUnavailable: 0
      maxSurge: 1
  revisionHistoryLimit: 1
  template:
    metadata:
      labels:
        app: reticulum
    spec:
      volumes:
      - name: storage
        hostPath:
          path: /tmp/ret_storage_data
          type: DirectoryOrCreate
      - name: config
        configMap:
          name: ret-config           
      containers:
      - name: reticulum
        volumeMounts:
        - name: storage
          mountPath: /storage
          mountPropagation: Bidirectional
        - name: config
          mountPath: /home/ret
        securityContext:
          privileged: true       
        image: mozillareality/ret:stable-latest
        ports:
        - containerPort: 9100
        imagePullPolicy: IfNotPresent
        env:
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: turkeyCfg_POD_NS
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: turkeyCfg_NODE_COOKIE
          valueFrom:
            secretKeyRef:
              name: configs
              key: NODE_COOKIE
        - name: turkeyCfg_HUB_DOMAIN
          valueFrom:
            secretKeyRef:
              name: configs
              key: HUB_DOMAIN
        - name: turkeyCfg_DOMAIN
          valueFrom:
            secretKeyRef:
              name: configs
              key: HUB_DOMAIN
        - name: turkeyCfg_DB_USER
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_USER
        - name: turkeyCfg_DB_PASS
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_PASS
        - name: turkeyCfg_DB_NAME
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_NAME
        - name: turkeyCfg_DB_HOST
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_HOST
        - name: turkeyCfg_DB_HOST_T
          valueFrom:
            secretKeyRef:
              name: configs
              key: DB_HOST_T
        - name: turkeyCfg_GUARDIAN_KEY
          valueFrom:
            secretKeyRef:
              name: configs
              key: GUARDIAN_KEY
        - name: turkeyCfg_PERMS_KEY
          valueFrom:
            secretKeyRef:
              name: configs
              key: PERMS_KEY
        - name: turkeyCfg_PHX_KEY
          valueFrom:
            secretKeyRef:
              name: configs
              key: PHX_KEY
        - name: turkeyCfg_SMTP_SERVER
          valueFrom:
            secretKeyRef:
              name: configs
              key: SMTP_SERVER
        - name: turkeyCfg_SMTP_PORT
          valueFrom:
            secretKeyRef:
              name: configs
              key: SMTP_PORT
        - name: turkeyCfg_SMTP_USER
          valueFrom:
            secretKeyRef:
              name: configs
              key: SMTP_USER
        - name: turkeyCfg_SMTP_PASS
          valueFrom:
            secretKeyRef:
              name: configs
              key: SMTP_PASS
        - name: turkeyCfg_ADM_EMAIL
          valueFrom:
            secretKeyRef:
              name: configs
              key: ADM_EMAIL
        - name: turkeyCfg_SKETCHFAB_API_KEY
          valueFrom:
            secretKeyRef:
              name: configs
              key: SKETCHFAB_API_KEY
        - name: turkeyCfg_IMG_PROXY
          value: nearspark.hcce
        - name: turkeyCfg_TENOR_API_KEY
          valueFrom:
            secretKeyRef:
              name: configs
              key: TENOR_API_KEY
        - name: turkeyCfg_YTDL_HOST
          value: "https://hubs-ytdl-fsu7tyt32a-uc.a.run.app"
        - name: turkeyCfg_PHOTOMNEMONIC
          value: "https://photomnemonic-fsu7tyt32a-uc.a.run.app"  
        - name: turkeyCfg_SPEELYCAPTOR
          value: "http://speelycaptor:5000"
        - name: turkeyCfg_STORAGE_QUOTA_GB
          value: "1000"
        livenessProbe:
          httpGet:
            path: /health
            port: 4001
            scheme: HTTP
          initialDelaySeconds: 30
          timeoutSeconds: 3
          periodSeconds: 30
        readinessProbe:
            initialDelaySeconds: 20
            httpGet:
              path: /?skipadmin
              port: 4001
              scheme: HTTP
            timeoutSeconds: 5
            periodSeconds: 5
            successThreshold: 5
            failureThreshold: 100
      - name: postgrest
        image: mozillareality/postgrest:stable-latest
        ports:
        - containerPort: 3000
        imagePullPolicy: IfNotPresent
        env:
        - name: PGRST_LOG_LEVEL
          value: info        
        - name: PGRST_DB_SCHEMA
          value: ret0_admin
        - name: PGRST_DB_ANON_ROLE
          value: postgres          
        - name: PGRST_DB_URI
          valueFrom:
            secretKeyRef:
              name: configs
              key: PGRST_DB_URI
        - name: PGRST_JWT_SECRET
          valueFrom:
            secretKeyRef:
              name: configs
              key: PGRST_JWT_SECRET
---
apiVersion: v1
kind: Service
metadata:
  name: ret
  namespace: hcce
spec:
  clusterIP: None
  ports:
  - name: http-reticulum
    port: 4001
    targetPort: 4001
  - name: https-reticulum
    port: 4000
    targetPort: 4000
  selector:
    app: reticulum
---
```