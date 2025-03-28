# Homeassitant


## Homeassitant installation by docker

docker-compose.yml

```yml
version: "3"

services:
  homeassitant:
    container_name: homeassistant
    image: homeassistant/home-assistant
    restart: unless-stopped
    network_mode: host
    privileged: true
    # environment:
    # - https_proxy=http://172.16.8.168:8123
    volumes:
      - /root/homeassistant/config:/config
      - /etc/localtime:/etc/localtime:ro
```

Create and Run docker

```bash
docker-compose up -d
```

## HACS installation (Integrations of the community store)

If hacs not installed, manually install hacs

```bash
# Go inside the container
docker exec -it homeassistant bash

# Run the hacs download script
wget -O - https://get.hacs.xyz | bash -

# Exit the container
exit
```

Restart homeassistant docker or restart server in Server Controls in Configuration.
If you are in China with very low speed of connecting hacs.xyz website, add http proxy in environment variable of docker compose file. It might to help to fast connect.

More details, following up with [official instruction](hacs.xyz/docs/configuration/basic)

## Integrations

Because we have a bad connection to github. I manuel install most of them from github.

### Themes

There is lovelace-ios-dark-mode-theme as an example

```bash
# download theme
git clone https://github.com/basnijholt/lovelace-ios-dark-mode-theme.git PATH/TO/HOMESSISTANT/config/themes
```

Add two lines to PATH/TO/HOMESSISTANT/config/configuration.yaml

```yml
frontend:
  themes: !include_dir_merge_named themes
```

Finally, restart homeassistant service and setup theme at theme of user's configuration

### HomeKit

1. Directly install 'HomeKit' in homeassistant official integration.
2. Apple devices pairing with QR code or 8 digits in Notification.
3. Setup configuration in homeassistant integration

### XiaoMi MIoT (Pending)

```bash
git clone https://github.com/ha0y/xiaomi_miot_raw.git
cp -dr ./xiaomi_miot_raw/custom_components/xiaomi_miot_raw PATH/TO/HOMESSISTANT/config/custom_components
# restart homeassistant docker or restart server in Server Controls in Configuration
docker restart homeassistant
```

Finally, set up XiaoMi account at Configuration/Integrations

## Setup Home&Shortcut in IOS

There are several issues I met in IOS setting up:

- Home app cannot connect to smart devices outside of home networks
  - But do not have a solution yet
- Home & Shortcut app cannot setup switch on or off only actions on smart devices
- After solving switch on or off only actions, the button light on and off in Home app is not a correct. That light on or off does not perform correctly as smart devices real switch state. Because unidirectional action only need to run it and do not need to 'off' run it

### Switch on or off only actions

The default home assistant switch button can trigger on and off. But cannot switch on or off only. An example of that, In my computer room I place 2 NFC tags to control computer on or off individually. But I don't want computer turn off by mis-touch when computer is on.

The best way to achieve this function is making a `script` in home assistant configuration.
Which applies to any other presetting action as well, such as trigger a specific light color value.

### Home&Shortcut in IOS

As all script set done, the Home app may not display the action of scripts. Under homeassistant Integration menu, `HomeKit integration must be re-add script element`.
At HomeKit integration icon. A configure model shall be popped up after click its configure button. Remove script first, and then re-open configure to add scripts.

`In shortcut app, the action need to be doubled`, because second action can turn icon button light off.

