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
- TODO: Enable GPIO at startup for hotend fans
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

## Mainsail Config

https://docs.mainsail.xyz/overview/quicktips/slicer-upload