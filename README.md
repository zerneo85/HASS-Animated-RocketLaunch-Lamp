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
The PCB i used is a PCB Board Hole Grid Board from 7cm x 3cm that i soldered the sensors on. All the sensors (ESP32 Cam & PIR) that are on the front of the PCB are detachable, to achieve this i soldered 2 rows of 8 female pins en for the PIR sensor a 3 row female pin.

For power & ground i sewed a wire on row 3 & 8 over the lengt of the PCB.
The micro USB DIP i soldered on A4 for ground (i connected it to row 3) and A8 for 5v power.



# ESP Home YAML file for Home Assistant
In the directory [Code](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Code) you can find the YAML file that i made and use in ESP Home in Home Assistant. Some important points are:

- I use the secrets file in ESP Home for all sensitive or device related data.
- The file makes use of the ds18b20 temperature sensor and not the DHT11 sensor.
- The ds18b20 has a disadvance in accuracy and requires a offset in temperature.
- To fix false alarms of the PIR & Doppler sensor i use a delayed_on value, for the PIR sensor it solves the false alarms.
- The Doppler sensor still requires some troubleshooting to get more stable.



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


But the STL files can also be downloaded from the directory [Designs](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Designs).

# Images – General
Images, animation and used icon files can also be downloaded from the directory [Media](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/tree/main/Media).


## Security-Node Front Closed
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Front-Closed.png)

## Security-Node Front Open
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Front-Open.png)

## Security-Node Back
![PCB Front](https://github.com/zerneo85/HASS-Animated-RocketLaunch-Lamp/blob/main/Images/Security-Node-Back.png)

