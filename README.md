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

## Tuning

Filament rotation distance

70 - 17.66 = 52.34
7 * 52.34 / 50 = 7.328


## Mainsail Config

https://docs.mainsail.xyz/overview/quicktips/slicer-upload

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