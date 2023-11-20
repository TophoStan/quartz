---
tags:
  - hubs
  - ce
---


![[HubsToolbar.png]]
De afbeelding weergeeft welke features er in welk deel van de toolbar zitten
In ToolbarLeft zit
- Invite button
In ToolbarCenter zit (enter scherm)
- Join Room knop
In ToolbarCenter zit (in room)
- Audio 

![[Pasted image 20231002105244.png]]
- Share

![[Pasted image 20231002105314.png]]
- Place

![[Pasted image 20231002105324.png]]
- React

![[Pasted image 20231002105337.png]]
- Chat

![[Pasted image 20231002105351.png]]

De inhoud van de toolbar staat gedefinieerd in de UI-root.js
```js
toolbarCenter={

                  <>

                    {watching && (

                      <>

                        <ToolbarButton

                          icon={<EnterIcon />}

                          label={<FormattedMessage id="toolbar.join-room-button" defaultMessage="Join Room" />}

                          preset="accept"

                          onClick={() => this.setState({ watching: false })}

                        />

                        {enableSpectateVRButton && (

                          <ToolbarButton

                            icon={<VRIcon />}

                            preset="accent5"

                            label={

                              <FormattedMessage id="toolbar.spectate-in-vr-button" defaultMessage="Spectate in VR" />

                            }

                            onClick={() => this.props.scene.enterVR()}

                          />

                        )}

                      </>

                    )}

                    {entered && (

                      <>

                        {!isLockedDownDemo && (

                          <>

                            <AudioPopoverButtonContainer scene={this.props.scene} />

                            <SharePopoverContainer scene={this.props.scene} hubChannel={this.props.hubChannel} />

                            <PlacePopoverContainer

                              scene={this.props.scene}

                              hubChannel={this.props.hubChannel}

                              mediaSearchStore={this.props.mediaSearchStore}

                              showNonHistoriedDialog={this.showNonHistoriedDialog}

                            />

                          </>

                        )}

                        {this.props.hubChannel.can("spawn_emoji") && (

                          <ReactionPopoverContainer

                            scene={this.props.scene}

                            initialPresence={getPresenceProfileForSession(this.props.presences, this.props.sessionId)}

                          />

                        )}

                      </>

                    )}

                    {/* {!isLockedDownDemo && (

                      <ChatToolbarButton

                        onClick={() => this.toggleSidebar("chat", { chatPrefix: "", chatAutofocus: false })}

                        selected={this.state.sidebarId === "chat"}

                      />

                    )} */}

                    {entered && isMobileVR && (

                      <ToolbarButton

                        className={styleUtils.hideLg}

                        icon={<VRIcon />}

                        preset="accept"

                        label={<FormattedMessage id="toolbar.enter-vr-button" defaultMessage="Enter VR" />}

                        onClick={() => exit2DInterstitialAndEnterVR(true)}

                      />

                    )}

                  </>

}
```

Maar als je bijvoorbeeld de chat knop weghaald is het nog niet afgelopen, want er zijn nog keyboard shortcuts. Dit betekent dat de text chat nog steeds geopend kan worden. Om dit te voorkomen zullen die ook uitgezet moeten worden.

Dit wordt gedaan in `keyboard-mouse-user.js` . In het code fragment is te zien hoe de shortcuts worden gedefinieerd. En dus ook verwijderd of met comments uitgezet worden. Of misschien met een boolean aan en uitgezet kan worden.

```js
{

      src: {

        value: paths.device.keyboard.key("t")

      },

      dest: {

        value: paths.actions.focusChat

      },

      xform: xforms.rising

    },

    {

      src: {

        value: paths.device.keyboard.key("/")

      },

      dest: {

        value: paths.actions.focusChatCommand

      },

      xform: xforms.rising

    },
```

LET OP: Ook in keyboard-debugging.js staan een paar shortcuts voor emojis/reacties

### Room settings
![[Pasted image 20231031092709.png]]
In de room settings zitten ook onderdelen verwerkt. Dit zijn de [[Slopen features hubs#text_chat]] & text chat. 

### **Potentiële mogelijkheden**
Er zijn verschillende aanpakken die gebruikt kunnen worden.
- Programmatisch comments toevoegen
- Environment variables boolean meegeven
- Elke mogelijkheid een kopie maken (*Niet aan te raden*)
- Browser extension


##### **Boolean meegeven**
je kan met een `ternary-operator` conditioneel een toetsenbord combinatie toevoegen. 
``` TS
shouldBeEnabled ? {

      src: {

        value: paths.device.keyboard.key("t")

      },

      dest: {

        value: paths.actions.focusChat

      },

      xform: xforms.rising

    } : {

      src: {

        value: paths.device.keyboard.key("")

      },

      dest: {

        value: paths.actions.focusChat

      },

      xform: xforms.rising

    }
```

De waarde van deze boolean komt tot stand door de volgende env variable mee te geven, `turkeyCfg_features_to_enable`. Deze variablen kan waarden op de volgende manier opslaan. Bijvoorbeeld `turkeyCfg_features_to_enable="text_chat,voice_chat"`. Dit houdt in dat text en voicechat aan worden gezet tijdens het opstarten van de hubs installatie. Deze manier van het aan en uitzetten zorgt ervoor dat je geen code hoeft aan te passen en slechts de eerder benoemde `ternary-operator` hoeft toe te passen op de juiste plaats. De reden voor de `turkeyCfg` prefix komt uit het bestand `run.sh`. 
``` powershell
#cheatsheet
#if [ -z ${var1+x} ]; then echo "is unset"; else echo " is set to '$var1'"; fi
if [ -z ${turkeyCfg_thumbnail_server+x} ]; then export turkeyCfg_thumbnail_server="nearspark.reticulum.io"; fi
if [ -z ${turkeyCfg_base_assets_path+x} ]; then export turkeyCfg_base_assets_path="https://$SUB_DOMAIN-assets.$DOMAIN/hubs/"; fi
if [ -z ${turkeyCfg_non_cors_proxy_domains+x} ]; then export turkeyCfg_non_cors_proxy_domains="$SUB_DOMAIN.$DOMAIN,$SUB_DOMAIN-assets.$DOMAIN"; fi
if [ -z ${turkeyCfg_reticulum_server+x} ]; then export turkeyCfg_reticulum_server="$SUB_DOMAIN.$DOMAIN"; fi
if [ -z ${turkeyCfg_cors_proxy_server+x} ]; then export turkeyCfg_cors_proxy_server="$SUB_DOMAIN-cors.$DOMAIN"; fi
if [ -z ${turkeyCfg_features_to_enable+x} ]; then export turkeyCfg_features_to_enable="default_value_for_features"; fi
if [ -z ${turkeyCfg_shortlink_domain+x} ]; then export turkeyCfg_shortlink_domain="$SUB_DOMAIN.$DOMAIN"; fi
if [ "$turkeyCfg_reticulum_server" = "$turkeyCfg_shortlink_domain" ]; then turkeyCfg_shortlink_domain="${turkeyCfg_shortlink_domain}/link"; fi
if [ -z ${turkeyCfg_sentry_dsn+x} ]; then export turkeyCfg_sentry_dsn=""; fi
if [ -z ${turkeyCfg_postgrest_server+x} ]; then export turkeyCfg_postgrest_server=""; fi
# if [ -z ${turkeyCfg_ita_server+x} ]; then export turkeyCfg_ita_server=""; fi
if [ -z ${turkeyCfg_ga_tracking_id+x} ]; then export turkeyCfg_ga_tracking_id=""; fi
export turkeyCfg_ita_server="turkey"

find /www/hubs/ -type f -name *.html -exec sed -i "s/{{rawhubs-base-assets-path}}\//${turkeyCfg_base_assets_path//\//\\\/}/g" {} \;           
find /www/hubs/ -type f -name *.html -exec sed -i "s/{{rawhubs-base-assets-path}}/${turkeyCfg_base_assets_path//\//\\\/}/g" {} \; 
find /www/hubs/ -type f -name *.css -exec sed -i "s/{{rawhubs-base-assets-path}}\//${turkeyCfg_base_assets_path//\//\\\/}/g" {} \; 
find /www/hubs/ -type f -name *.css -exec sed -i "s/{{rawhubs-base-assets-path}}/${turkeyCfg_base_assets_path//\//\\\/}/g" {} \;             
anchor="<!-- DO NOT REMOVE\/EDIT THIS COMMENT - META_TAGS -->" 
for f in /www/hubs/pages/*.html; do 
    for var in $(printenv); do 
    var=$(echo $var | cut -d"=" -f1 ); prefix="turkeyCfg_"; 
    [[ $var == $prefix* ]] && sed -i "s/$anchor/ <meta name=\"env:${var#$prefix}\" content=\"${!var//\//\\\/}\"\/> $anchor/" $f; 
    done 
done 

if [ "${access_log}" = "enabled" ]; then sed -i "s/access_log off;//g" /etc/nginx/conf.d/default.conf; fi

nginx -g "daemon off;"
```

Zelf heb ik de volgende regels toegevoegd om de features door te passen naar de Mozilla Hubs applicatie.
``` Shell
if [ -z ${turkeyCfg_features_to_enable+x} ]; then export turkeyCfg_features_to_enable="default_value_for_features"; fi
```

Maar hoe ontvangt de Hubs Client de environment variables? Deze worden, zoals te zien is in `run.sh`, respectievelijk in een \<\meta>-tag gezet op de `.html`-bestanden van Mozilla hubs.

Dit bestand wordt aangeroepen in het Docker bestand `RetPageOriginDockerfile` die er als volgt uitziet.
```Dockerfile
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

De waarden van `turkeyCfg_features_to_enable` kan worden aangegeven in het `hcce.yam` worden aangegeven. Je leest het correct `.YAM` niet `.YAML`, waarom? Het `.YAM`-bestand moet aanduiden dat het bestand nog niet compleet is een nog variabelen er in heeft verwerkt

``` YAML
# Een ENV variable bij de Spoke Deployment
- name: turkeyCfg_hubs_server
          value: $HUB_DOMAIN
# Bijvoorbeeld een certificaat dynamisch voor een domein genereren
apiVersion: v1
kind: Secret
metadata:
  name: cert-assets.$HUB_DOMAIN
  namespace: $Namespace
type: kubernetes.io/tls
```


Met behulp van de `handlebars`-lib en de volgende regex `/\$([a-zA-Z_]\w*)/g, '{{$1}}'` geeft ik aan dat variabelen die met de syntax die vanuit Hubs is meegegeven met het `$` teken worden omgezet naar `{{WAARDE}}`, omdat `handlebars` werkt met de {{}} syntax.

``` ts
const deploymentTemplatePath = 'src\\app\\api\\kubernetes\\deployment.template.YAM'
    const yamlTemplate = fs.readFileSync(deploymentTemplatePath, 'utf8');


    //Replace all $ variables with {{}} variables
    const convertedVariablesYaml = yamlTemplate.replace(/\$([a-zA-Z_]\w*)/g, '{{$1}}');

    const template = handlebars.compile(convertedVariablesYaml);



    const data = {
        HUB_DOMAIN: variables.HUB_DOMAIN,
        Namespace: variables.Namespace,
        ADM_EMAIL: 'WAARDE',
        DB_USER: 'WAARDE',
        DB_PASS: 'WAARDE',
        DB_NAME: 'WAARDE',
        DB_HOST: 'WAARDE',
        DB_HOST_T: 'WAARDE',
        PGRST_DB_URI: 'WAARDE',
        PSQL: 'WAARDE',
        SMTP_SERVER: 'WAARDE',
        SMTP_PORT: 'WAARDE',
        SMTP_USER: 'WAARDE'
    }
```


### Hubs features beschrijving
#### Text_chat

![[Pasted image 20231002105351.png]]

Keybinds: "t" en "/"
Text chat is ook aanwezig als togglebutton in `RoomSettingsSidebar.js`
``` js
{toggleHubsFeatures("voice_chat", configs.FEATURES_TO_ENABLE) ?
              <ToggleInput
                label={<FormattedMessage id="room-settings-sidebar.voice-chat" defaultMessage="Voice chat" />}
                {...register("member_permissions.voice_chat")}
              /> : null
            }
```
#### Voice_chat

![[Pasted image 20231027091818.png]]

Keybinds: "m"
Voice chat is ook aanwezig als togglebutton in `RoomSettingsSidebar.js`
```js
{toggleHubsFeatures("text_chat", configs.FEATURES_TO_ENABLE) ?
              <ToggleInput
                label={<FormattedMessage id="room-settings-sidebar.text-chat" defaultMessage="Text chat" />}
                {...register("member_permissions.text_chat")}
              /> : null
            }
```
Bij voice_chat hoort ook het volgende scherm. Wanneer voice_chat dus uitgezet is, is het de bedoeling dat dit scherm ook niet getoond hoeft te worden.


![[Pasted image 20231031093747.png]]

Dit wordt uitgezet in `MicSetupModal.js` door de functie gelijk aan te roepen die de gebruiker door moet sturen naar de echte room.

``` js
if (!(toggleHubsFeatures('voice_chat', configs.FEATURES_TO_ENABLE))) { onEnterRoom(); }
```
#### Share
![[Pasted image 20231030161625.png]]

geen keybinds


#### Flying
Keybinds: "g"
De allow flying knop kan gevonden worden in `RoomSettingsSidebar.js`. 


![[Pasted image 20231031113207.png]]

```js
{
              toggleHubsFeatures("flying", configs.FEATURES_TO_ENABLE) ? <ToggleInput
                label={<FormattedMessage id="room-settings-sidebar.fly" defaultMessage="Allow flying" />}
                {...register("member_permissions.fly")}
              /> : null
            }
```

#### Teleport
Keybinds: "Right mouse button"
Het teleporteren zit op 2 plekken namelijk voor het inklikken van de knop en loslaten.
```js
toggleHubsFeatures("teleport", configs.FEATURES_TO_ENABLE) ? {
      src: {
        value: paths.device.mouse.buttonRight
      },
      dest: {
        value: paths.actions.startGazeTeleport
      },
      xform: xforms.rising,
      priority: 100
    } : emptyBinding,

    toggleHubsFeatures("teleport", configs.FEATURES_TO_ENABLE) ? {
      src: {
        value: paths.device.mouse.buttonRight
      },
      dest: {
        value: paths.actions.stopGazeTeleport
      },
      xform: xforms.falling,
      priority: 100
    } : emptyBinding,
```


#### Place
Place staat in de `CenterToolbar`. Maar place bestaat uit 7 andere onderdelen op dit moment. Deze zal ik prefixen als `place_<FEATURE>`. 

![[Pasted image 20231101140000.png]]

De `Create and move objects` toggle-button in de Room settings. Enabled deze feature. Maar create cameras en pin objects zijn nog sub-onderdelen hiervan. `Create cameras` zorgt ervoor dat je een camera kan neerzetten om foto's en video's mee te kunnen maken. `Pin objects` zorgt ervoor dat je elementen kan pinnen. Een element dat gepinned is, blijft in de ruimte staan zelfs als de speler die dit element geplaatst heeft de ruimte heeft verlaten. 
![[Pasted image 20231101152626.png]]
 
Alle place acties
![[Pasted image 20231101140149.png]]

##### Place_Pen
![[Pasted image 20231101143528.png]]

Keybinds: 
- "RightMousebutton" - Drop pen or camera
- "p" - Equip pen
- "Shift + E" / "Shift + Q" - Next/Previous Pen color
- "Shift + {Mousewheel up/down}" - Pen size
- "Ctrl + Z" - Undo pen stroke

De roomsettings in de sidebar `Create drawings` zorgt ervoor dat je deze functionaliteit aan of uit kan zetten!


*Op dit moment nog niet stabiel bij het uitzetten van deze feature!*
##### Place_Camera
![[Pasted image 20231101143537.png]]

Keybinds:
- "RightMousebutton" - Drop pen or camera
- "C" - Spawn een camera in
##### Place_GIF
![[Pasted image 20231101143544.png]]


##### Place_3DModel
![[Pasted image 20231101143552.png]]


##### Place_Avatar
![[Pasted image 20231101143559.png]]


##### Place_Scene
![[Pasted image 20231101143604.png]]


##### Place_Upload
![[Pasted image 20231101143610.png]]



#### React
![[Schermafbeelding 2023-11-01 143817.png]]

React opent net als Place een popup menu, maar het zijn allemaal dezelfde acties. Dus dit beschouw ik ook als 1 feature.
Elke Keybind representeert een emoticon.
Keybinds: "1","2","3","4","5","6","7"

React kan aan- en/of uitgezet worden door `Create emoji` in de `Roomsettings` 
#### avatar_setup
#### enter_on_device
#### spectate
#### create_room
#### change_name_&\_avatar
#### favorite_rooms
#### favorite_room
#### close_room
#### room_info_and_settings
#### people_tab