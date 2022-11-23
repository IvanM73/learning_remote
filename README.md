
Introduction
------------

The packages are designed to simplify programming and use of your IR remotes.
It suport **multiple** remotes in a single UI and has a **Learning mode** to easy program your remote IR.

All test was done using **RM4 mini universal remote** but potentially can be used for any remote control that implement **remote.send_command** and **remote.learn_command**


PREREQUISITE
------------
- RI remote like **RM4 mini universal remote** properly configured and enabled in your system.
- [custom:button-card](https://github.com/custom-cards/button-card) installed in HA


HOW IT WORKS:
------------

The package define :

- **remote_type** input_select that define the list of remote you want to use in your dashboard.

Example : 

```yaml
  remote_type:
      name: REMOTE_TYPE
      options:
        - STEREO
        - TV
        - Decoder
      initial: STEREO
      icon: mdi:remote
```

- **remote_learning** input_boolean that define if the learning nmode is on or off.

- one **remote_changesource**  script to switch between remote
- one **remote_press_button**  script to implement the logic when a remote button is pressed. Use active remote and use send/learn command based on **remote_learning** flag.

- one new lovelace dashboard that define the remote.
the dashboard does use [custom:button-card](https://github.com/custom-cards/button-card)
the dasboard does include 
- multiple buttons to switch between remotes
  
```yaml
      - type: custom:button-card
	color_type: card
	tap_action:
	  action: call-service
	  service: script.remote_changesource
	  service_data:
	    device: STEREO
	name: STEREO
	show_icon: false
	entity: input_select.remote_type
	color: rgb(255, 255, 255)
	state:
	  - value: STEREO
	    color: rgb(0, 255, 0)
```

- multiple buttons to learn/send commands:
```yaml
  - type: custom:button-card
	color_type: card
	tap_action:
	  action: call-service
	  service: script.remote_press_button
	  service_data:
		command: Power on
	icon: mdi:power
	color: rgb(75, 75, 77)
```

- one button to enable/disable the learning mode
```yaml
  - show_name: true
	show_icon: false
	type: button
	hold_action:
	  action: toggle
	entity: input_boolean.remote_learning
	name: LEARNING MODE
	show_state: true
	icon_height: 10px
	tap_action:
	  action: toggle
```

HOW TO DEPLOY:
------------

- copy auto_learning_remote.yaml in config/packages directory
- configure the list of remote you want to use 

```yaml
  remote_type:
      name: REMOTE_TYPE
      options:
        - STEREO
        - TV
        - Decoder
      initial: STEREO
```

- define the IR remote control your are using
  entity_id: remote.wi_fi_universal_remote_remote	  => entity_id: remote.yourremotecontroller
	  
- make sure your configuration.yaml include packages

```yaml
  homeassistant:
  ...
  packages: !include_dir_named packages/
```

- create a new lovelace card based on **remoteLovelace.yaml**


HOW TO PROGRAM YOUR REMOTES
------------
- press the button of the remote you want to program (the selected remote become **green**)
- enable learning mode, pressing the dedicated UI buton
- press the button you wan to program
- press the real remote button (It need to readed by your remote IR)
- repeat for all buttons you want to program
- disable learning mode

HOW TO USE YOUR REMOTE
------------
- make suere learning mode is disabled 
- Select the remote to use
- Press any already programmed button
- ENJOY!


