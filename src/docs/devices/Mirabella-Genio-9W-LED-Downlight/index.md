---
title: Mirabella Genio Wi-Fi 9w CCT Downlight
date-published: 2022-03-04
type: light
standard: au
---

## General Notes

The [Mirabella Genio 9W LED Wi-Fi Dimmable Downlight](https://www.mirabellagenio.com.au/product-range/mirabella-genio-wi-fi-dimmable-9w-led/) 9 Watt, 800 Lumens, Dimmable, Colours: Warm White to Cool White (2700K – 6500K).

![Genio 9W LED Wi-Fi Dimmable Downlight](/9wgeniodownlight.jpg "Genio 9W LED Wi-Fi Dimmable Downlight")

They are sold at Kmart in a [Blister Pack](https://www.kmart.com.au/webapp/wcs/stores/servlet/ProductDisplay?partNumber=P_42799337&storeId=10701&catalogId=10102).

Inside is a TYWE2S module based on the ESP8266 microcontroller. OTA flash not available. Must be done with TTL adapter module with selectable 3.3V. [Programmer](https://www.altronics.com.au/p/z6225-funduino-ftdi-basic-usb-breakout-3.3v-5v/).


## GPIO Pinout

| Pin    | Function        |
| ------ | -------------   |
| GPIO12 | Warm White PWM  |
| GPIO14 | Cool White PWM  |

## Basic Configuration

```yaml
# Config for Mirabella Genio WiFi LED Strip Light
# https://www.esphome-devices.com/devices/Mirabella-Genio-WiFi-LED-Strip-Light/
esphome:
  platform: ESP8266
  board: esp01_1m

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  ap:
    ssid: "down_light"
    password: "ap_password"

# Enable logging
logger:

# Enable Home Assistant API
api:
  password: "api_password"

ota:
  password: "ota_password"

output:
  - platform: esp8266_pwm
    id: output1
    pin: GPIO12
  - platform: esp8266_pwm
    id: output2
    pin: GPIO14

light:
  - platform: cwww
    id: LED
    name: "Deck Kitchen Light 1"
    cold_white: output2
    warm_white: output1
    cold_white_color_temperature: 6500 K
    warm_white_color_temperature: 2700 K

    # Ensure the light turns on by default if the physical switch is actuated.
    restore_mode: ALWAYS_OFF

sensor:
  - platform: uptime
    name: ${friendly_name} Uptime

  - platform: wifi_signal
    name: ${friendly_name} Signal Strength

text_sensor:
  - platform: wifi_info
    ip_address:
      name: ${friendly_name} IP
    ssid:
      name: ${friendly_name} SSID
    bssid:
      name: ${friendly_name} BSSID
    mac_address:
      name: ${friendly_name} Mac
       
switch:
  - platform: restart
    name: ${friendly_name} REBOOT
```
