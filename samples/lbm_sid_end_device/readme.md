This is a copy of nordic's ``sid_end_device``, adding LoRa Basics Modem functionality.  

LBM libraries on this platform only support Class A operation in the US 915 region with LR11xx.  
Other features such as relay, Class B, Class C, FUOTA, CSMA and other such features are **NOT** supported or validated at this time.

During LBM operation, the four LEDs on the NRF52840-DK are not driven.
## build
The following below provides command line build examples.  
The vscode equivalent is at **Edit Build Configuration** in the nrf connect plugin for vscode, select a ``prj.conf`` for **Base Configuration file**, and then ``overlay-*.conf`` for **Extra Kconfig fragment**
### LoRaWAN build
for all devices LR1110, LR1120, and LR1121
LoRaWAN credentials are declared in ``example_options.h``  
``west build -b nrf52840dk_nrf52840 -- -DOVERLAY_CONFIG=overlay-lbm.conf``

button 4 toggles between operating LBM vs sidewalk  
button 1 in sidewalk operation, sends uplink, the same as original sid_end_device  

### NAV3 on sidewalk build
only for LR1110 and LR1120, not for LR1121
``west build -b nrf52840dk_nrf52840 -- -DOVERLAY_CONFIG=overlay-nav3sid.conf``

### NAV3 on LBM build
only for LR1110 and LR1120, not for LR1121
lorawan credentials are declared in ``example_options.h``  
``west build -b nrf52840dk_nrf52840 -- -DOVERLAY_CONFIG=overlay-lbm-nav3lbm.conf``
