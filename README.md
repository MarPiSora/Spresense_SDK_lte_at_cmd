# Spresense LTE-M IoT Connectivity Kit Spresense_SDK_lte_at_cmd

## Overview
Spresense lte_at_cmd application provides as simple at-cmd possibility for Sony Spresense LTE-M IoT Connectivity Kit

```AT+ ``` commands are defined in 3GPP standard [3GPP 27.007 commands](https://www.etsi.org/deliver/etsi_ts/127000_127099/127007/13.03.00_60/ts_127007v130300p.pdf)

```AT% ``` commands are proprietary ones for the Murata 1SC-DM module.

## How to use
Spresense lte_at_cmd application is to be build via Sony Spresense SDK or pre built application can be flushed to board directly.
It implements simple at-cmd parser using lte_send_atcmd_sync function.

#### Prerequisites:
- This code uses [SDK and repository of Sony Spresense](https://github.com/sonydevworld/spresense.gi). 
Please setup your Sony SDK to include the libraries following [Sony SDK Setup Guide](https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html) 
Inclusive flashing bootloader following [Sony SDK Flash Bootloader](https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html#_flashing_bootloader)

#### Installation: 
- You can flash precompiled 'nuttx.spk' directly to the device if above mentioned Sony SDk is set up already:

  ```tools/flash.sh -c /dev/ttyUSB0 -b 500000 ./<yourpath>/nuttx.spk```

#### Build application: 
- Setup example applications according to Sony Spresense website and add lte_at_cmd example:
  Get Sony Spresense repository from Github: 
  ```git clone --recursive https://github.com/sonydevworld/spresense.git```
     
     According to: https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html#_build_method

- Copy the lte_at_cmd files from this repository to ```./spresense``` directory created from above github clone:
  
  ```
    ./spresense
        |_  /examples/lte_at_cmd
        |_  /sdk/configs/examples/lte_at_cmd
  ```

- Build System according to:
     https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html#_build_system
  ```cd sdk
     tools/config.py default
     ```
- If needed follow the procedure to update the spresense binaries to latest version:
  ```./tools/flash.sh -e ../../spresense-binaries-v2.4.0.zip``` 
 
- Prepare the build:	
  ```make clean```
- Check if files are in correct folders:	
  ```tools/config.py -i examples/lte_at_cmd```

- Start configure script:	
  ```tools/config.py examples/lte_at_cmd```

- Start make script:  
  ```make```

- Plugin Sony Spresense board and flash nuttx.spk to device:

  ```tools/flash.sh -c /dev/ttyUSB0 nuttx.spk
  ```
 
- After successfull flash open a terminal program or minicom
   (e.g. use [Chrome browser serial terminal](https://googlechromelabs.github.io/serial-terminal) )
 
- Connect to device and start module via 'lte_sysctl start':
  ```minicom -D /dev/ttyUSB0 -b 115200```

### Console output at the Serial port
```text
nsh> lte_sysctl start                   // start module via lte_sysctl start
nsh> lte_at_cmd ati                     // pass any at-cmd to module
ATI

Manufacturer: Murata Manufacturing Co., Ltd.
Model: LBAD0XX1SC-DM
Revision: RK_03_00_00_00_04121_001

OK
nsh> lte_at_cmd at+cgdcont?
AT+CGDCONT?
+CGDCONT: 1,"IP","soracom.io",,0,0,0,0,0,,0,,,,,
OK

nsh> lte_at_cmd at+cereg=2
AT+CEREG=2
OK

nsh> lte_at_cmd at+cereg?
AT+CEREG?
+CEREG: 2,0
OK

nsh> ifup eth0
ifup eth0...OK
nsh> lte_at_cmd at+cereg?
AT+CEREG?
+CEREG: 5,5,"05D4","019B6403",7
OK
```

## License

The sketches in this repository are released under [The MIT License](./LICENSE), and libraries used by these sketches are subject to their respective licenses.    
For example, the library of [Arduino Core codes for Spresense reference board](https://github.com/sonydevworld/spresense-arduino-compatible/tree/master/Arduino15/packages/SPRESENSE/hardware/spresense/1.0.0) used for cellular communications is released under the LGPL v2.1 license.

## Disclaimers

The sketches in this repository are examples only and are not guaranteed to work. In addition, the content of this sketch is not intended for commercial use.
Please use them at your own risk.
