# SWSD006 - LR11xx Multi-stack Software Development Kit for nRF52840

SWSD006 is a collection of driver, protocol stack and utility software that facilitates development of Sidewalk
applications. The software includes numerous examples of how to leverage the unique capabilities of Semtech's LR11xx silicon. 
While the software targets the Nordic nRF52840 MCU, it is designed and distributed as a "full-source" offering, it enables users to modify
the contents across multiple layers of software stack. Potential modifications include re-targeting of host MCU and LR11xx silicon,
changes to the platform abstraction layer and enhancement of the packet fragmentation scheme. 
Please note that while software enhancement is enabled and encouraged, validation of described functionality was exclusively performed on the specified silicon variants and software component versions.

This repository implements the following functionality:
- LR11xx transceiver silicon support for Sidewalk MAC v1.16 CSS and FSK modulation 
- Drivers and examples demonstrating WIFI/GNSS NAV3 geolocation features of LR11xx silicon
- LoRaWAN Class A multi-stack operation using SWL2001 - LoRa Basics MOdem 4.5.0; programmatic control over both LoRaWAN and Sidewalk in one FW image
- LR11xx transceiver firmware upgrade via SWTL001 port to nRF52840
- LoRaWAN Class A + WIFI/GNSS NAV3 operation example
- An example of packet fragmentation and re-assembly (overcomes CSS packet size limitations in Sidewalk)
- AWS lambda example code demonstrating End-to-End handling of LoRa EDGE application data: from transceiver to cloud service 
- A Zephyr compliant T2 topology supporting 3rd party extension of Nordic's VS Code IDE
- 'sid_dut' operation with LR11xx silicon, enabling Sidewalk Device Qualification testing
- A full-source platform abstraction layer supporting user customization of RF front-end designs and control paradigms 

## Getting started

Before getting started, make sure you have a proper nRF Connect SDK development environment.
Follow the official
[Installation guide](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/installation/install_ncs.html).

### Initialization

The first step is to initialize the workspace folder (``my-workspace``) where
the ``example-application`` and all nRF Connect SDK modules will be cloned. Run the following
command:

```shell
# when doing this from bash terminal started from nordic's toolchain manager, unset ZEPHYR_BASE
# initialize my-workspace, where 'my-workspace" can be a directory name of your choice
west init -m https://github.com/Lora-net/SWSD006 --mr v2.6.1 my-workspace

# update nRF Connect SDK modules
cd my-workspace
west update
```
### configuring project for LR11xx
edit the project's ``.conf`` (default is ``prj.conf``)
```
CONFIG_SIDEWALK_LINK_MASK_LORA=y # run sidewalk in CSS, or..
CONFIG_SIDEWALK_LINK_MASK_FSK=y # run sidewalk in FSK
CONFIG_RADIO_LR11XX=y  # use LR11xx instead of sx126x
CONFIG_RADIO_TCXO=y  # set to n if using crystal on LR11xx instead of TCXO
```
for the building & running  ``template_lbm`` application, see the readme in ``template_lbm`` subdirectory  

### Available example applications:
all example apps are built using ``west build -b <board> -- -DOVERLAY_CONFIG=foobar.conf``.  The app to build is selected by adding ``-- -DOVERLAY_CONFIG=foobar.conf``
 * the full build command, for example: ``west build -b nrf52840dk_nrf52840 -- -DOVERLAY_CONFIG=overlay-nav3sid.conf``
#### apps provided by Nordic:
from the directory ``samples/sid_end_device`` enables LR11xx in ``prj.conf`` -->
* hello
   * ``-DOVERLAY_CONFIG=overlay-hello.conf``
* sensor_monitoring
   * ``-DOVERLAY_CONFIG=overlay-demo.conf``
* dut, aka ``sid_dut``
   * ``-DOVERLAY_CONFIG=overlay-dut.conf``
#### apps added by Semtech:
from the directory ``samples/lbm_sid_end_device`` -->
* lora basics modem (lorawan end device), running in sidewalk environment
   * ``-DOVERLAY_CONFIG=overlay-lbm.conf``
* NAV3 running simultanously with sidewalk (aka NAV3 in bypass mode):
  * ``-DOVERLAY_CONFIG=overlay-nav3sid.conf``
* NAV3 running in lora basics modem:
  * `-DOVERLAY_CONFIG=overlay-nav3lbm.conf``
 * LR11xx firmware update, and almanac erase:
   * build in ``samples/SWTL001``directory


# GNSS Performance Evaluation Notice

The included GNSS example source code is provided solely to demonstrate the GNSS scan functionality under ideal conditions. 
The source code and GNSS scan results are not representative of the optimal configuration or performance characteristics 
of the silicon. The LR11xx product family is flexible and can be emobodied and configured in a multitude of ways to realize 
various trade-offs regarding
performance, battery life, PCB size, cost, etc. The GNSS example included in this release and the corresponding evaluation 
kits are designed & configured by the included source code in a default capacity which is sufficient to demonstrate functional
GNSS scan capability only. Care must be taken if/when attempting to assess performance characterstics of GNSS scan functionality 
and we strongly encourage those conducting such analysis to contact Semtech via the provided support channels so that we can 
ensure appropriate configuration settings are employed for a given performance evaluation use-case.
