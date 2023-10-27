#hubs
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
```TS
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

```TS
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

### **Potentiële mogelijkheden**
Er zijn verschillende aanpakken die gebruikt kunnen worden.
- Programmatisch comments toevoegen
- Environment variables boolean meegeven
- Elke mogelijkheid een kopie maken (*Niet aan te raden*)
- Browser extension


**Boolean meegeven**
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
#### text_chat
![[Pasted image 20231002105351.png]]
Keybinds: "t" en "/"
#### voice_chat
![[Pasted image 20231027091818.png]]
Keybinds:
#### allow_flying
#### create_and_move_objects
#### create_drawings
#### create_emoji
![[Pasted image 20231027091858.png]]
Keybinds
#### avatar_setup



#### enter_on_device
#### spectate
#### teleport
#### create_room
#### change_name_&\_avatar
#### favorite_rooms
#### favorite_room
#### close_room
#### room_info_and_settings
#### people_tab