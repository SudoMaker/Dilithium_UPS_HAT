# Dilithium UPS HAT

English | 中文 //TODO

## Disclaimer
This product is a DIY product. Users should have certain hands-on skills, general computer knowledge, basic electrical knowledge, general safety knowledge, and a very strong sense of safety. Neither the author nor the copyright holder of this product shall be held liable for any adverse consequences resulting from the use of this product.

## Where to buy
[Tindie](https://www.tindie.com/products/sudomaker/dilithium-mini-ups-hat/) (for worldwide buyers)

[Taobao](https://item.taobao.com/item.htm?id=636945235597) (for China mainland buyers)

## Hardware
### Supported platforms
Almost every Linux SBC with a 40Pin Raspberry Pi compatible header is supported.

Tested ones:
- Raspberry Pi Zero
- Raspberry Pi 3A+
- Raspberry Pi 4B
- NVIDIA Jetson Nano 2GB/4GB
- Pine A64 (A64-DB-V1.1)

Support for MCU boards with a 40Pin Raspberry Pi compatible header (STM32/PIC32/ESP32/etc) is coming soon.

### Thermal notices
#### Raspberry Pi 4
Raspberry Pi 4's CPU is very hot even in idle. If Dilithium UPS HAT is directly placed above the CPU, it will be "roasted" by the hot CPU. You may buy the Pi Zero to A+ adapter (which has a lot of holes and supports installing a fan) to improve this condition.

#### High output current
Sustained 4A output should only be used with proper active cooling. Otherwise it may shorten the board's lifespan or destroy the board.

#### Fast battery charge
If you are charging the battery with a high current (2A or more), and the powered SBC is running an intensive task, the board will still be overheat. If you choose not to use active cooling, which is fine as long as output current doesn't exceed 3A, charging current will be automatically decreased when in a overheat condition. 

## Software

### Installation
#### Debian-ish (Debian/Ubuntu/Mate/Armbian/etc)
64bit:
```
dpkg --add-architecture armhf
apt update
apt install libc6:armhf libstdc++6:armhf libncurses5:armhf libncursesw5:armhf libtinfo5:armhf
apt install upower kmod
dpkg -i dilithium-tools_<version>_armhf.deb
```
32bit:
```
apt install libncurses5 libncursesw5 libtinfo5
apt install upower kmod
dpkg -i dilithium-tools_<version>_armhf.deb
```

#### Raspbian
```
apt install libncurses5 libncursesw5 libtinfo5
apt install upower kmod
dpkg -i dilithium-tools_<version>_armhf_raspbian.deb
```

#### Other distros / hack by yourself
Unpack the .deb file and copy the files to proper destinations.

or if you prefer not to use a set of closed source tools, you can:
- Use TI's bqStudio + EV2400 combo to design your battery profile
- Write device trees for these devices:

|Device|I2C addr|
|---|---|
|bq27510g3|0x55|
|ds1340|0x68|
|bq25895|0x6a|

### Language support
Currently this software supports only English and Simplified Chinese (UTF8).

Set appropriate `LANG` & `LC_*` environment variables to select different languages.

### Usages
#### Get battery stats
##### From GUI
The battery icon is in the tray area just like you're using a Linux laptop.

You can view detailed battery stats, including history curves, in system settings. Support in different desktop environments may vary.

##### From command line
Simply run `upower -i /org/freedesktop/UPower/devices/battery_bq27510g3_0`

Example output:
```
  native-path:          bq27510g3-0
  vendor:               Texas Instruments
  power supply:         yes
  updated:              Fri Mar 19 20:41:28 2021 (2 seconds ago)
  has history:          yes
  has statistics:       yes
  battery
    present:             yes
    rechargeable:        yes
    state:               charging
    warning-level:       none
    energy:              3.79976 Wh
    energy-empty:        0 Wh
    energy-full:         12.3072 Wh
    energy-full-design:  12.288 Wh
    energy-rate:         7.02059 W
    voltage:             4.077 V
    time to full:        1.2 hours
    percentage:          30%
    temperature:         58.5 degrees C
    capacity:            100%
    technology:          lithium-ion
    icon-name:          'battery-good-charging-symbolic'
  History (rate):
    1616157688  7.021   charging
    1616157686  7.029   charging
    1616157684  7.025   charging
    1616157679  7.025   charging
    1616157677  7.029   charging
```
##### From a scripting language
Simply read from the files in `/sys/class/power_supply/bq27510g3-0/`. Values are in different files.
e.g.:
```
➜  ~ cat /sys/class/power_supply/bq27510g3-0/capacity 
32
➜  ~ cat /sys/class/power_supply/bq27510g3-0/uevent
POWER_SUPPLY_NAME=bq27510g3-0
POWER_SUPPLY_STATUS=Charging
POWER_SUPPLY_PRESENT=1
POWER_SUPPLY_VOLTAGE_NOW=4099000
POWER_SUPPLY_CURRENT_NOW=1721000
POWER_SUPPLY_CAPACITY=33
POWER_SUPPLY_CAPACITY_LEVEL=Normal
POWER_SUPPLY_TEMP=333
POWER_SUPPLY_TECHNOLOGY=Li-ion
POWER_SUPPLY_CHARGE_FULL=3205000
POWER_SUPPLY_CHARGE_NOW=1010000
POWER_SUPPLY_CHARGE_FULL_DESIGN=3200000
POWER_SUPPLY_CYCLE_COUNT=2
POWER_SUPPLY_HEALTH=Good
POWER_SUPPLY_MANUFACTURER=Texas Instruments
```

#### Maintaince tools
##### Calibration wizard
A TUI wizard to help you set and calibrate the basic parameters of the battery.

Installed path: `/usr/bin/Dilithium_Calibration_Wizard`

If you use ssh on Windows, please use a `xterm`-compatible terminal.

Depending on the capacity of your optional battery, this calibration process may take from a few hours to a full day.

**Upon completion, you will get accurate battery capacity information. This is useful when using a suspected fake battery. Then you can get accurate power measurement on any supported battery.**

##### Profile tool
A command line tool to help you import & export calibrated battery parameters.

Installed path: `/usr/bin/Dilithium_Profile_Tool`

Examples:
```
Dilithium_Profile_Tool --export --file some_4cell_pack.json --cell-count 4 --cell-capacity 2000
Dilithium_Profile_Tool --import --file LGChem_INR18650M26_2600mAh_x3.json
```

See its `--help` for more usages.
