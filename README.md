# Spresense_SDK_lte_at_cmd
Spresense lte_at_cmd application to be build via Sony Spresense SDK
 
 * Implements simple at-cmd parser using lte_send_atcmd_sync
 *
 * Prerequisite: 
 * - Installation and setup of Sony Spresense SDK
 *   https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html
 * - inclusive flashing bootloader
 *   https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html#_flashing_bootloader
 * 
 * Either flash nuttx.spk directly:
 * - flash nuttx.spk to device
 *   >tools/flash.sh -c /dev/ttyUSB0 -b 500000 ./<yourpath>/nuttx.spk
 * 
 
 * Or setup example applications according to Sony Spresense website and add lte_at_cmd example:
  * Get Sony Spresense repository from Github: 
   - git clone --recursive https://github.com/sonydevworld/spresense.git
     According to: https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html#_build_method

 * Copy the lte_at_cmd files from this repository to ./spresense directory created from above
   - ./spresense
        |_/examples/lte_at_cmd
        |_/sdk/configs/examples/lte_at_cmd

 * Build System according to:
     https://developer.sony.com/develop/spresense/docs/sdk_set_up_en.html#_build_system
  -  cd sdk
  -  tools/config.py default
  * If needed follow the procedure to update the spresense binaries to latest version:
  -  ./tools/flash.sh -e ../../spresense-binaries-v2.4.0.zip 
 
 * Prepare the build:	
  -  make clean
 * check if files are in correct folders:	
  -  tools/config.py -i examples/lte_at_cmd

 * start configure script:	
  -  tools/config.py examples/lte_at_cmd

 * start make script:  
  -  make

 * plugin Sony Spresense board and flash nuttx.spk to device:  
  -  tools/flash.sh -c /dev/ttyUSB0 nuttx.spk
 
 * after successfull flash open a terminal program or minicom
   (e.g. use Chrome browser serial terminal: https://googlechromelabs.github.io/serial-terminal/ )
 
 * Connect to device and start module via 'lte_sysctl start':
 *   > minicom -D /dev/ttyUSB0 -b 115200

nsh> lte_sysctl start
nsh> lte_at_cmd ati
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
