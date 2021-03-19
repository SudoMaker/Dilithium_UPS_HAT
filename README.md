# Dilithium UPS HAT

English | 中文

## Disclaimer
This product is a DIY product. Users should have certain hands-on skills, general computer knowledge, basic electrical knowledge, general safety knowledge, and a very strong sense of safety. Neither the author nor the copyright holder of this product shall be held liable for any adverse consequences resulting from the use of this product.

## Hardware
### Specifications
- 5V 3A output with passive cooling, 5V 4A with active cooling

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

### Language support
Currently this software supports only English and Simplified Chinese (UTF8).

Set appropriate `LANG` & `LC_*` environment variables to select different languages.

### Usages
#### Command-line tools
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
