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