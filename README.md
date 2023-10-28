# Home Assistant integrated Animated Rocket Launch
A Rocket-Command Node managed bij Home Assistant with ESPHome and integrated with WLED

# Introduction
As with many of the projects we start, this one started simple and with a single purpose but quickly grew to what it is today. Current day, it’s one of the many devices i have build and configured in Home Assistant that has a crucial role in the area of entertaining my son but also for security. It is build on 2 ESP32 Controllers combined with 1 motion detection sensors, 1 distance sensor, buzzer for audio notifications, speaker & microphone and some controllable leds.

Besides the use of entertaining my son (Animation, countdown, lights) it also services a roles in security. I have created a couple of security display nodes (Security- Clock, Security-Cat) that show by using RGB Led and audio notification triggers of the security nodes.  The security-node project can be found here
https://github.com/zerneo85/ESP32-Cam-Security-Node


# Continuous improvement
As my partner often points out i have a issue with trying to improve everything.
So if you have any suggestions or improvements i would be very happy to implement them! 


# Sensors & Components
I have used the sensors below in the project. 
- ESP32Cam -Microcontroller
- AM312 – PIR Sensor
- Passive piezo buzzer
- Dallas ds18b20 – Temperature Sensor
- Resistor R4,7K
- DHT11 – Temperature Sensor
- RCWL-0516 – Doppler Sensor

# PCB Design
The PCB i used is a PCB Board Hole Grid Board from 7cm x 3cm that i soldered the sensors on. All the sensors (ESP32 Cam & PIR) that are on the front of the PCB are detachable, to achieve this i soldered 2 rows of 8 female pins en for the PIR sensor a 3 row female pin.

For power & ground i sewed a wire on row 3 & 8 over the lengt of the PCB.
The micro USB DIP i soldered on A4 for ground (i connected it to row 3) and A8 for 5v power.

## PCB Front
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node%20PCB%20Front.png)

## PCB Back
![PCB Back](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node%20PCB%20Back.png)


# ESP Home YAML file for Home Assistant
In the directory [Code](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Code) you can find the YAML file that i made and use in ESP Home in Home Assistant. Some important points are:

- I use the secrets file in ESP Home for all sensitive or device related data.
- The file makes use of the ds18b20 temperature sensor and not the DHT11 sensor.
- The ds18b20 has a disadvance in accuracy and requires a offset in temperature.
- To fix false alarms of the PIR & Doppler sensor i use a delayed_on value, for the PIR sensor it solves the false alarms.
- The Doppler sensor still requires some troubleshooting to get more stable.

```
esphome:
  name: rocket-command-node
  friendly_name: rocket-command-node

esp32:
  board: esp32dev
  framework:
    type: arduino


wifi:
  networks:
  - ssid: "SSID1"
    password: !secret wifi_password
  - ssid: "SSID2"
    password: !secret wifi_password
  # Optional manual IP
  manual_ip:
    static_ip: !secret static_ip_rocket-command-node
    gateway: !secret gateway
    subnet: !secret subnet
    dns1: !secret dns1

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Rocket-Command-Node"
    password: !secret ap_pw
    ap_timeout: 15s
captive_portal:


# Enable webserver
web_server:
  port: 80
  auth:
    username: !secret web_server_username
    password: !secret web_server_password

# Enable logging
logger:
  level: INFO

# Enable Time Server
time:
  - platform: homeassistant
    id: esptime


# Enable Home Assistant API
api:
  encryption:
    key: !secret encryption_rocket-command-node
  services:
    - service: play_rtttl
      variables:
        song_str: string
      then:
        - rtttl.play:
            rtttl: !lambda 'return song_str;'
    - service: start_rocket_countdown
      then:
        - display.page.show: countdown10
        - delay: 0.9s
        - display.page.show: countdown9
        - delay: 0.9s
        - display.page.show: countdown8
        - delay: 0.9s
        - display.page.show: countdown7
        - delay: 0.9s
        - display.page.show: countdown6
        - delay: 0.9s
        - display.page.show: countdown5
        - delay: 0.9s
        - display.page.show: countdown4
        - delay: 1s
        - display.page.show: countdown3
        - delay: 1s
        - display.page.show: countdown2
        - delay: 1s
        - display.page.show: countdown1
        - delay: 1s
        - display.page.show: countdownliftoff
        - delay: 3s
    - service: show_distance
      then:
        - display.page.show: distance
        - component.update: rocket_command_node_display
    - service: show_rocket_planet
      then:
        - display.page.show: rocket
        - component.update: rocket_command_node_display
        - delay: 3s
        - display.page.show: rocketstars
        - component.update: rocket_command_node_display
        - delay: 3s
        - display.page.show: rocketplanet
        - component.update: rocket_command_node_display
        - delay: 3s
    - service: play_rocket_launch
      then:
        - display.page.show: rla02
        - delay: 0.5s
        - display.page.show: rla03
        - delay: 0.5s
        - display.page.show: rla04
        - delay: 0.5s
        - display.page.show: rla05
        - delay: 0.5s
        - display.page.show: rla06
        - delay: 0.5s
        - display.page.show: rla07
        - delay: 0.5s
        - display.page.show: rla08
        - delay: 0.5s
        - display.page.show: rla09
        - delay: 0.5s
        - display.page.show: rla10
        - delay: 0.5s
        - display.page.show: rla11
        - delay: 0.5s
        - display.page.show: rla12
        - delay: 0.5s
        - display.page.show: rla13
        - delay: 0.5s
        - display.page.show: rla14
        - delay: 0.5s
        - display.page.show: rla15
        - delay: 0.5s
    - service: show_motion_detected
      then:
        - display.page.show: motion_detected
        - component.update: rocket_command_node_display
        - delay: 10s
        - display.page.show: distance
        - component.update: rocket_command_node_display
    - service: show_goodnight
      then:
        - display.page.show: goodnight
    - service: show_goodday
      then:
        - display.page.show: goodday
        - delay: 2s
        - component.update: rocket_command_node_display
# Enable Home Assistant OTA        
ota:
  password: !secret ota_rocket-command-node


# Sensors
binary_sensor:
  - platform: homeassistant
    entity_id: input_boolean.rocketsleepmode
    name: "sleepmode"
    id: rocket_command_node_sleepmode
  - platform: gpio
    pin: GPIO33
    name: "Rocket PIR Sensor"
    device_class: motion
    filters:
      - delayed_on: 200ms
  - platform: gpio
    name: Button
    pin:
      number: GPIO14
      mode: INPUT_PULLUP
      inverted: true
    filters:
      - delayed_on: 1ms
    on_multi_click:
      - timing:
          - ON for at most 1s
          - OFF for at most 1s
          - ON for at most 1s
          - OFF for at least 0.2s
        then:
          - homeassistant.event:
               event: esphome._rocket_button_pressed
               data:
                 message: "Double"
      - timing:
          - ON for 1s to 4s
          - OFF for at least 0.5s
        then:
          - homeassistant.event:
               event: esphome._rocket_button_pressed
               data:
                 message: "SingleLong"  
      - timing:
          - ON for at most 1s
          - OFF for at least 0.5s
        then:
          - homeassistant.event:
               event: esphome._rocket_button_pressed
               data:
                 message: "SingleShort"

# Speaker and Microfoon
switch:
  - platform: gpio
    pin:
      number: GPIO26
      inverted: False
    name: "Speaker Record"
    restore_mode: RESTORE_DEFAULT_OFF
  - platform: gpio
    pin:
      number: GPIO27
      inverted: False
    name: "Speaker Play"
    restore_mode: RESTORE_DEFAULT_OFF




# Passive Buzzer
output:
  - platform: ledc
    pin: GPIO25
    id: rtttl_out
    channel: 2


 # rtttl   
rtttl:
  output: rtttl_out
  on_finished_playback:
    - logger.log: 'Song ended!'


# Animations
#animation:
 # - file: animations/rocket_fly.gif
 #   id: animation_rocket_fly



# Images
image:
  - file: icons/clock.png
    id: icon_clock
    resize: 80x80
  - file: icons/percentage.png
    id: icon_percentage
    resize: 80x80
  - file: icons/sound.png
    id: icon_sound
    resize: 100x100
  - file: icons/timer.png
    id: icon_timer
    resize: 100x100
  - file: icons/rocket.png
    id: icon_rocket
    resize: 100x100
  - file: icons/rocket_stars.png
    id: icon_rocket_stars
    resize: 100x100
  - file: icons/rocket_planet.png
    id: icon_rocket_planet
    resize: 100x100
  - file: animations/rocketlaunch/rla02.png
    id: animation_rla02
    resize: 100x100
  - file: animations/rocketlaunch/rla03.png
    id: animation_rla03
    resize: 100x100
  - file: animations/rocketlaunch/rla04.png
    id: animation_rla04
    resize: 100x100
  - file: animations/rocketlaunch/rla05.png
    id: animation_rla05
    resize: 100x100
  - file: animations/rocketlaunch/rla06.png
    id: animation_rla06
    resize: 100x100
  - file: animations/rocketlaunch/rla07.png
    id: animation_rla07
    resize: 100x100
  - file: animations/rocketlaunch/rla08.png
    id: animation_rla08
    resize: 100x100
  - file: animations/rocketlaunch/rla09.png
    id: animation_rla09
    resize: 100x100
  - file: animations/rocketlaunch/rla10.png
    id: animation_rla10
    resize: 100x100
  - file: animations/rocketlaunch/rla11.png
    id: animation_rla11
    resize: 100x100
  - file: animations/rocketlaunch/rla12.png
    id: animation_rla12
    resize: 100x100
  - file: animations/rocketlaunch/rla13.png
    id: animation_rla13
    resize: 100x100
  - file: animations/rocketlaunch/rla14.png
    id: animation_rla14
    resize: 100x100
  - file: animations/rocketlaunch/rla15.png
    id: animation_rla15
    resize: 100x100
  - file: icons/astronaut_standing.png
    id: icon_astronaut_standing
    resize: 45x45


# Fonts
font:
  - file: 'arial.ttf'
    id: fontariel10
    size: 10
  - file: 'arial.ttf'
    id: fontariel12
    size: 12
  - file: 'arial.ttf'
    id: fontariel14
    size: 14
  - file: 'arial.ttf'
    id: fontariel18
    size: 18
  - file: 'arial.ttf'
    id: fontariel24
    size: 24
  - file: 'arial.ttf'
    id: fontariel28
    size: 28
  - file: 'arial.ttf'
    id: fontariel30
    size: 30
  - file: 'arial.ttf'
    id: fontariel60
    size: 60

# Text sensors
text_sensor:
  - platform: homeassistant
    entity_id: sensor.rocket_command_node_distance
    name: "distance"
    id: rocket_command_node_distance


# Distance sensor
sensor:
  - platform: ultrasonic
    trigger_pin: GPIO32
    echo_pin: GPIO13
    name: "Distance"
    update_interval: 0.2s
    filters:
    - lambda: return x * 100;
    unit_of_measurement: "cm"
    accuracy_decimals: 1

# I²C Bus Display
i2c:
  sda: GPIO21
  scl: GPIO22
  scan: True
  frequency: 400kHz



# Display
display:
  - platform: ssd1306_i2c
    model: "SH1106 128x64"
    id: "rocket_command_node_display"
    address: 0x3C
    rotation: 180
    update_interval: 1s
    pages:
      - id: distance
        lambda: |-
          if(id(rocket_command_node_sleepmode).state) {
          } else {
          it.printf(0, 0, id(fontariel14), TextAlign::TOP_LEFT, "Distance to Rocket");
          it.printf(50, 30, id(fontariel24), TextAlign::TOP_LEFT, "%s", id(rocket_command_node_distance).state.c_str());
          it.image(0, 20, id(icon_astronaut_standing));
          }
      - id: motion_detected
        lambda: |-
          it.printf(0, 0, id(fontariel18), "Press button to");
          it.printf(0, 30, id(fontariel18), "launch rocket!!!");       
      - id: countdown10
        lambda: |-
          it.print(55, 2, id(fontariel60), TextAlign::TOP_CENTER, "10");
      - id: countdown9
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "9");    
      - id: countdown8
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "8");
      - id: countdown7
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "7");    
      - id: countdown6
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "6");
      - id: countdown5
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "5");    
      - id: countdown4
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "4");
      - id: countdown3
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "3");    
      - id: countdown2
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "2");
      - id: countdown1
        lambda: |-
          it.print(60, 2, id(fontariel60), TextAlign::TOP_CENTER, "1");    
      - id: countdownliftoff
        lambda: |-
          it.print(60, 2, id(fontariel30), TextAlign::TOP_CENTER, "Liftoff");
          it.print(60, 34, id(fontariel30), TextAlign::TOP_CENTER, "!!!");        
      - id: rocket
        lambda: |-
          it.image(32, 0, id(icon_rocket));    
      - id: rocketstars
        lambda: |-
          it.image(32, 0, id(icon_rocket_stars));    
      - id: rocketplanet
        lambda: |-
          it.image(32, 0, id(icon_rocket_planet));      
      - id: rla02
        lambda: |-
          it.image(32, 0, id(animation_rla02));
      - id: rla03
        lambda: |-
          it.image(32, 0, id(animation_rla03));         
      - id: rla04
        lambda: |-
          it.image(32, 0, id(animation_rla04));
      - id: rla05
        lambda: |-
          it.image(32, 0, id(animation_rla05));
      - id: rla06
        lambda: |-
          it.image(32, 0, id(animation_rla06));
      - id: rla07
        lambda: |-
          it.image(32, 0, id(animation_rla07));
      - id: rla08
        lambda: |-
          it.image(32, 0, id(animation_rla08));         
      - id: rla09
        lambda: |-
          it.image(32, 0, id(animation_rla09));
      - id: rla10
        lambda: |-
          it.image(32, 0, id(animation_rla10));
      - id: rla11
        lambda: |-
          it.image(32, 0, id(animation_rla11));
      - id: rla12
        lambda: |-
          it.image(32, 0, id(animation_rla12));
      - id: rla13
        lambda: |-
          it.image(32, 0, id(animation_rla13));         
      - id: rla14
        lambda: |-
          it.image(32, 0, id(animation_rla14));
      - id: rla15
        lambda: |-
          it.image(32, 0, id(animation_rla15));
      - id: goodnight
        lambda: |-
          it.print(0, 15, id(fontariel28), TextAlign::TOP_LEFT, "Goodnight");
      - id: goodday
        lambda: |-
          it.print(0, 15, id(fontariel30), TextAlign::TOP_LEFT, "Goodday");

```

# 3D Print STL files
I have created the box and pole but for the other parts i have used other peoples designs

## PCB Holders
https://www.thingiverse.com/thing:5809510

## Rocket and fume
- Original: https://www.thingiverse.com/thing:3262427
- My Own 


## ESP32 and ISD1820 holder

## Box

## Lid

## Pole



But the STL files can also be downloaded from the directory [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs).

# Images – 3D print & Sensors & PCB
## Security-Node Front Closed
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Front-Closed.png)

## Security-Node Front Open
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Front-Open.png)

## Security-Node Back
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Back.png)



# Images – STL Screenshots
## Security-Node body STL screenshot
![Security-Node body STL screenshot](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node%20body%20STL%20screenshot.png)

## Security-Node ESP32 Cam & AM312 Lid STL screenshot
![Security-Node ESP32 Cam & AM312 Lid STL screenshot](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node%20ESP32%20Cam%20%26%20AM312%20Lid%20STL%20screenshot.png)

## Security-Node RCWL Doppler Lid STL screenshot
![Security-Node RCWL Doppler Lid STL screenshot](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node%20RCWL%20Doppler%20Lid%20STL%20screenshot.png)

## Security-Node ESP32 Cam & HC-SR501 Lid STL screenshot
![Security-Node ESP32 Cam & HC-SR501 Lid STL screenshot](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node%20ESP32%20Cam%20%26%20HC-SR501%20Lid%20STL%20screenshot.png)


# Images – 3D print Pictures
## Security-Node Body Front
![Security-Node Body Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Body-Front.png)

## Security-Node Body Back
![Security-Node Body Back](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Body-Back.png)

## Security-Node RCWL Doppler Lid
![Security-Node RCWL Doppler Lid](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Doppler-Lid-Front.png)

## Security-Node ESP32 Cam & AM312 Lid
![Security-Node ESP32 Cam & AM312 Lid](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-AM312-Lid.png)

## Security-Node ESP32 Cam & HC-SR501 Lid
![Security-Node ESP32 Cam & HC-SR501 Lid](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-HCSR501-Lid.png)


# Images – Sensors & PCB
## PCB empty
![PCB Empty](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/PCB-Empty.png)

## PCB Back
![PCB Back](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-PCB-Back.png)

## PCB Front
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-PCB-Front.png)

