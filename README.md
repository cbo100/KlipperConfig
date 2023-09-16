# Klipper Config

* Sidewinder X1
* BLTouch/CRTouch on Z-min
* Filament on Z-max
* MKS Mini12864 V3

## Firmware Build

```
cd ~/klipper/
make menuconfig
```

Seems to be defaults:
- Atmega AVR
- atmega2560
- 16Mhz
- 250000 baud
- Enable GPIO at startup for hotend fans
    - AR9,AR7

```
make clean
make
```

Flashing

```
ls /dev/serial/by-id/*
> /dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
sudo service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
sudo service klipper start
```

## Configuration

https://www.klipper3d.org/Config_Reference.html
https://github.com/tispokes/klipper_on_SWX1
https://docs.arduino.cc/hacking/hardware/PinMapping2560
https://www.klipper3d.org/Config_checks.html

## Hardware

Acceleration: 3000 might have been high

Vref = I x 8 x R
1.02 = I x 8 x 0.08

1.02 = 1.3 x 8 x 0.1

Vref
1.02V = 1.6 A

E = 1A = 0.64 Vref
X,Z = 1.7A = 1.088 Vref
Y = 1.7A = 1.088 Vref

XYZ = 1A = 0.68 Vref
E = 1.4A = 0.9 Vref

Random internet person found same default:

> Stock is Set XYZ = 1.0V E = 1.0V , I dialed my 2208's down a bit. I like to run XYZ = 0.68V E= 0.9V anywhere in this ange is perfectly fine just don't' exceed 1.0V



## Tuning

Filament rotation distance

70 - 17.66 = 52.34
7 * 52.34 / 50 = 7.328



## Mainsail Config

https://docs.mainsail.xyz/overview/quicktips/slicer-upload

## Leveling

https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging

## Prusa Slicer Config

* Start Gcode

``` gcode
SET_PRINT_STATS_INFO TOTAL_LAYER=[total_layer_count]
START_PRINT
```

* End Gcode
``` gcode
END_PRINT
```

* After layer change

``` gcode
;AFTER_LAYER_CHANGE
;[layer_z]

SET_PRINT_STATS_INFO CURRENT_LAYER={layer_num + 1}
```


### Revised Setup Instructions...

A mix of configs, trying to use github to manage all via moonraker conf

``` shell
mkdir ~/custom/
cd custom
git clone https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging.git
ln -s ./Klipper-Adaptive-Meshing-Purging/Configuration/ ~/printer_data/config/KAMP
git clone https://github.com/cbo100/KlipperConfig.git
ln -s ./KlipperConfig/ ~/printer_data/config/SidewinderX1
```

``` ini (printer.cfg)
[include mainsail.cfg]
[include timelapse.cfg]
[include ./SidewinderX1/SidewinderX1.cfg]

[virtual_sdcard]
path: ~/printer_data/gcodes
```

``` ini (moonraker.conf)
 [update_manager Klipper-Adaptive-Meshing-Purging]
 type: git_repo
 channel: dev
 path: ~/custom/Klipper-Adaptive-Meshing-Purging
 origin: https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging.git
 managed_services: klipper
 primary_branch: main

 [update_manager SidewinderX1-Config]
 type: git_repo
 channel: dev
 path: ~/custom/KlipperConfig
 origin: https://github.com/cbo100/KlipperConfig
 managed_services: klipper
 primary_branch: main

```
