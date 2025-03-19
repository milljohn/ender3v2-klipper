# Creality Ender 3V2 Klipper
A guide on setting up Klipper on the Ender 3V2.

I relied heavily on [this guide](https://www.obico.io/blog/install-klipper-ender-3/) for the initial setup and installation.

I used the macros from [this repo](https://github.com/LeeOtts/Ender3v2-Klipper-Configs/tree/main). I modified a few of the settings for the Creality 4.2.2 board.

This has configs for the [EZABL](https://www.th3dstudio.com/product/ezabl-ng-auto-bed-leveling-kit-for-unified-2-klipper-marlin-firmware/). The klipper config was [found here](https://tickets.th3dstudio.com/products/article/ezabl-klipper-setup-guide).

------------------------------------

## Using a printer I setup

1. Download and install [OrcaSlicer](https://github.com/SoftFever/OrcaSlicer/releases). At the time of this writing [V2.2.0](https://github.com/SoftFever/OrcaSlicer/releases/tag/v2.2.0) is the latest stable release.
2. Setup the profile for the Ender 3V2.
3. Change the gcode to klipper.
4. Change the starup gcode.
5. Setup network

### Change the gcode to klipper
Click on the ![printer config](img/edit_printer.png) icon to edit the printer settings.

![orca](img/orca.png)

Change the "G-code flavor" to "Klipper"
![](img/gcode.png)


### Change the startup gcode
Click on the ![printer config](img/edit_printer.png) icon to edit the printer settings.

![orca](img/orca.png)

Select the Machine G-code tab
![Machine G-code](img/startup.png)

Add the following for "Machine start G-code":
```
 M117
M140 S0
M104 S0 
START_PRINT EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[bed_temperature_initial_layer_single]
```

Add the following to "Machine end G-code":
```
END_PRINT
```

## Setup KlipperScreen

- Hardware:
    -[I used this screen](https://www.amazon.com/dp/B0BZGW51FY?ref=ppx_yo2ov_dt_b_fed_asin_title)
    - [USB OTG Cable](https://www.amazon.com/gp/product/B07TYQLQQ2?smid=ATJ0SDIGG41D4&th=1)
    - [HDMI Cable](https://www.amazon.com/RIIEYOCA-Adapter-Degree-Compatible-Camcorder/dp/B0D2NNX88J?crid=3IVEU75U419CR&dib=eyJ2IjoiMSJ9.YAqx3vtaS26NyXE2VHTxh1WAYA9jPgCO93TWr3PzoM5uri6tfK3V4ZLqGL832eNpW3tpLQ-kqVGRaNmZgkQc_sATRvymd3a-DujHdqX1MOBYTlHag0MiU6wIF5EeTx5U-trJ2HZ0e4WG6GqBEZNU97Bpeeok1F_15lKYGltvbmgdUC7VVH8ljwHWx1mgM1UA2iPnsn-yKtyhl4Bay8TochccF7IvS755almAxMWbmSZWOeaXdKe3N0a6eAaNLSLyiUwqdPKk1PW92qJhPpf5HK9AmPiAgl8Clvk4TZbdx9HriL4HvJ_k-DIK9L5zT4PoWvOxbWhaqjnMPi9hSqJ3UmfiqV3BofkSbSetJTaWWsw.50qtwzQDPRAegoc87i142gf7wR9bvySpE8W9DzrDsmk&dib_tag=se&keywords=hdmi%2Bto%2Bmini%2Bhdmi%2B90%2Bdegree%2Badapter&qid=1742310853&s=industrial&sprefix=hdmi%2Bto%2Bmini%2Bhdmi%2B90%2Bdegree%2Badapter%2Cindustrial%2C118&sr=1-9&th=1)
    - [HDMI Adapter](https://www.amazon.com/gp/product/B0BN697DWH?smid=A2I8OB8UELUM5B&th=1)
    - [Case](https://www.thingiverse.com/thing:6961003)
        - 6 - M3x10
        - 4 - M3x6
    - [VESA75 Mount](https://www.thingiverse.com/thing:6379235/files)
    - [Articulating Arm](https://www.thingiverse.com/thing:2194278/files)
    - [V-slot mount](https://www.thingiverse.com/thing:5690403/files)
- SSH into Pi:
    - ``cd ~ && git clone https://github.com/dw-0/kiauh.git``
    - ``./kiauh/kiauh.sh``
        - Use the default for all prompts

See: 
- [KlipperScreen](https://klipperscreen.readthedocs.io/en/latest/Installation/)
- [Klipper Installation And Update Helper](https://github.com/dw-0/kiauh)

## Calibrate Z Offset

- home all axies
- in the console: ``PROBE_CALIBRATE``
- use controls to calibrate
- ``SAVE_CONFIG``
- ``PROBE_ACCURACY``

## Mainboard Configuration
- [SKR mini E3 v3.0](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/hardware/BTT%20SKR%20MINI%20E3%20V3.0)
- [Runs Klipper](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3/tree/master/firmware/V3.0/Klipper)

## PID Calibration

In the console:
- ``PID_CALIBRATE HEATER=extruder TARGET=200``
- ``PID_CALIBRATE HEATER=heater_bed TARGET=60``
- ``SAVE_CONFIG``

NOTE: You only need to do this if you replace the thermistor or hotend, OR on first calibration.

## LCD Interface

NOTE: I didn't get this setup. Using the OEM screen requires using the the UART pins on the Raspberry Pi, which are used for comminicating with the main board.

It is possible to use a BTT LCD, however the pins on the mainboard are being used. The SKR Mini E4 does have other UART pins, but I used the TFT pins.

[DWIN_T5UIC1_LCD](https://github.com/bustedlogic/DWIN_T5UIC1_LCD) is a Python class for the Ender 3 V2 LCD runing klipper3d with Moonraker. The interface board used the original cable and uses the power from the mainboard to power the LCD and use and external power supply for the raspberry pi. A single cable runs to the raspberry pi.

![lcd to raspberry pi interface board](img/Ender3v2-screen-to-serial.drawio.png)

## Printed Parts

- [Ender 3/3v2 Cable Chain](https://www.printables.com/model/349525-ender-33v2-cable-chain/comments) (`X_Hotend_Connector.stl` doesn't fit correctly)
- [Solid Ender 3 Cable Chains for all Axes](https://www.thingiverse.com/thing:4316238/files) (only need `X_axis_hotend_connector_20mm_longer.stl`)
- [Case with 4040 Mount for Raspberry Pi Zero REMIX](https://www.thingiverse.com/thing:5239378)
- [Utra-slim Raspberry Pi Zero 2 W case for 2020 profile](https://www.printables.com/model/316538-utra-slim-raspberry-pi-zero-2-w-case-for-2020-prof/)
- [Raspberry Pi Zero 2 W Case](https://www.printables.com/model/361093-raspberry-pi-zero-2-w-case)