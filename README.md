# Home Assistant integrated Animated Rocket Launch
A Rocket-Command Node managed bij Home Assistant with ESPHome and integrated with WLED

# Introduction
As with many of the projects we start, this one started simple and with a single purpose but quickly grew to what it is today. Current day, it’s one of the many devices i have build and configured in Home Assistant that has a crucial role in the area of entertaining my son but also for security. It is build on 2 ESP32 Controllers combined with 1 motion detection sensors, 1 distance sensor, buzzer for audio notifications, speaker & microphone and some controllable leds.

Besides the use of entertaining my son (Animation, countdown, lights) it also services a roles in security. I have created a couple of security display nodes (Security- Clock, Security-Cat) that show by using RGB Led and audio notification triggers of the security nodes.  The security-node project can be found here
https://github.com/zerneo85/ESP32-Cam-Security-Node


# Continuous improvement
As my partner often points out i have a issue with trying to improve everything.
So if you have any suggestions or improvements i would be very happy to implement them! 

# Global setup
I made the choise to use 2 ESP32 modules, 1 for intergation with Home Assistant that controls the system sensors and 1 with WLED installed. I have done this because it is easier to use WLED to make nice lights and then integrate WLED to Home Assistant. I will reference to these 2 systems as ESP32-Hass or ESP32-WLED. That being said i started with WLED on a D1 but there where issues. I also know there is a library in ESPHome to control led like WLED but i didn't put much effort in it.  That being said i realise the overkill of controllers but since they are so cheap i took the easy way out.

# Sensors & Components
I have used the sensors below in the project. I have sorted them to show if it was ESP32-Hass or ESP32-WLED.

 ESP32-Hass
- 1 x ESP32 Breakout Board Expansion Board Kits GPIO 1 in 2 with Wireless WiFi Bluetooth for 38 Pin Narrow Version ESP32 ESP-WROOM-32 Microcontroller Development Board
- 1 x CQRobot Speaker 3 Watt 8 Ohm / 3 Watt 4 Ohm / 5 Watt 8 Ohm, 3W 4O JST-PH2.0, Speaker 3 Watt 4 Ohm-JST-PH2.0 Interface
- 1 x AZDelivery ESP32 NodeMCU Module WLAN WiFi Development Board with CP2102 
- 1 x AM312 IR Human Sensor AM312 Mini Detector Module HC-SR312 Pyroelectric Infrared PIR Motion Automatic
- 1 x Passive piezo buzzer
- 1 x ISD1820 Voice Recorder
- 1 x 1.3 inch OLED SSH1106 128 x 64 pixel I2C
- 1 x Push Button
- 1 x HC-SR04 Ultrasonic Module

ESP32-WLED
- 1 x AZDelivery ESP32 NodeMCU Module WLAN WiFi Development Board with CP2102
- 1 x Prototype PCB 
- 4 to 6 Terminal Block Connector
- 1 x MAX9814 Microphone AGC Amplifier Module
- 20 to 30 controllable leds



# PCB Design
- The PCB i used is a PCB Board Hole Grid Board from 7cm x 5cm that i soldered the Terminal Block Connector on. 
- For the ESP32-WLED i soldered 2 rows of 8 female pins so that it is detachable. 
- All the sensors except the MAX9814 are connected to (ESP32-HASS) Breakout Board.
- For power & ground i used the Terminal Block Cinnectors over the lengt of the PCB.


# ESP Home YAML file for Home Assistant
In the directory [Code](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Code) you can find the YAML file that i made and use in ESP Home in Home Assistant. Some important points are:
- I use the secrets file in ESP Home for all sensitive or device related data.

# Scripts and automations Home Assistant
In the directory [Code](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Code) you can find the YAML file that i made and use in Home Assistant for the scripts and automations. You need to change these accordingly 


# 3D Print STL files
I have created the box and pole but for the other parts i have used other peoples designs

## PCB Holders
https://www.thingiverse.com/thing:5809510

## Rocket and fume
- Original: https://www.thingiverse.com/thing:3262427
- My Own: [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs)


## ESP32 and ISD1820 holder
STL files can also be downloaded from the directory [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs).

## Box
STL files can also be downloaded from the directory [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs).

## Lid
STL files can also be downloaded from the directory [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs).

## Pole
STL files can also be downloaded from the directory [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs).


# Images – General
- Icons and animations i used can also be downloaded from the directory [Media](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Media/icons).
- Images and animations i used can also be downloaded from the directory [Media](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Media/rocketlaunch).
- Images of components i used can also be downloaded from the directory [Media](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Media/components).



## Images – Design STL files
![Design STL files](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Media/Rocket-Command-Node_Box_Front.png)

## Images – Picture Internal
![Picture Internal](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Media/Rocket-Internal.jpg)

## Images – Picture Home Assistant Dashboard 
![Picture Home Assistant Dashboard](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Media/Home_Assistant_Dashboard.png)


