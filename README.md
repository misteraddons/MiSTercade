# MiSTercade
 MiSTer FPGA JAMMA adapter

## Installation
* Choose a MiSTer.ini (15 kHz or 31 kHz) and rename it to MiSTer.ini
** (31 kHz is untested)
* Copy these files to your MiSTer's micro SD card, overwriting existing files
* Toggle SW0 on DE10-nano (one of the 4 large switches near the GPIO header) towards the center of the board. SW1-4 should remain off (towards edge of board)
* Plug MiSTercade PCB into top of DE10-nano, mating all pin headers
* Install USB bridge to both the DE10-nano and MiSTercade PCB
* Set pin jumper next to JAMMA edge to either Direct or Protect (see table below)
* CHECK 5V LINE ON ARCADE POWER SUPPLY TO ENSURE IT'S BETWEEN 5.1V DC AND 5.3V DC. 
** If Bluetooth/Wifi dongles are used, set voltage closer to 5.3V so USB ports get a full 5V.

## Controls
### Button mapping:
``` 
1 2 3    B A R
4 5 6    Y X L
Coin = Select
Start = Start
Menu/OSD = Down + Start
```

Feel free to remap as needed

Several core input mappings have been done, but not all. Many of the arcade cores have R or L mapped to P2 start and coin. Fixing those is outside the scope of this project, but I'd like to standardize them to the above mapping, if developers permit. MAME does this well and I think it's the way to go.

### Updating controller firmware:
Should the controller firmware need updating do as follows:
* Put hid-flash_MiSTer and firmware files in the root of micro SD card
* Toggle "STM32 DFU" dip switch to on
* Toggle "USB Controls" dip switch to off then back to on
* Plug in USB keyboard
* Press F9 on main menu of MiSTer
* username: root
* password: 1
* Enter the following commands:
```
cd /media/fat/
lsusb
(Check for 1209:babe in the device list - this is the STM32 microcontroller bootloader)
hid-flash_MiSTer STM32_2P_Encoder_whateveritscalled.bin
lsusb
(Check for 8888:8888 in the device list - this is the STM32 microcontroller firmware)
exit
```
* Press F12 to return to main menu
* Turn STM32 DFU dip switch off
* Toggle "USB Controls" dip switch to off then back to on

## Sound
Sound varies wildly across cores. The best thing to do is to set the blue potentiometer low enough so most cores don't clip, and amplifier is still loud enough.
Volume can be adjusted using the protruding potentiometer, or in the menu as follows:
* Bring up the OSD Menu by pressing down + start (or whatever you mapped it to)
* Press left
* Adjust core volume or main volume
Core volume settings are saved to the SD card for future use

## Video
RGBS Video levels are ~3V

## SNAC
Built-in SNAC is intended for light guns, but works with controllers as well. The usual caveats with SNAC apply - can't control OSD menu, 1 player, no native arcade controls, etc.

## Built-in Switches
| Switch Name | Left Position | Right Position |
| --- | --- | --- |
| Power Jumper | Protected (uses built-in protection circuit) | Direct (straight from the arcade PSU to the DE10-nano) |
| Video | To JAMMA / 40 Pin Header | To VGA video |
| Audio | Onboard Volume Adjustment | External Volume Adjustment |
| Audio DAC | Off | On |
| USB CONTROLS | Off | On
| MCU BOOTLOADER | Normal mode | Bootloader Mode |
| CC1=CC2 | Coin Counter signals separate | Coin Counter signals merged |
| COIN1=COIN2 | Coin signals separate | Coin signals merged (candy cabinets) |
| P1B5 | JAMMA 26 Disconnected | JAMMA 26 Connected |
| P1B6 | JAMMA 27 Disconnected | JAMMA 27 Connected |
| P2B5 | JAMMA d Disconnected | JAMMA d Connected |
| P2B6 | JAMMA e Disconnected | JAMMA e Connected |
