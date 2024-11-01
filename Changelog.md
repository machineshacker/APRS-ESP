### Changelog of the project. 

## Version - Date
## 0.0.1 2024-10-31


### Config info
See config.h, main.h, pinout.h for relevant settings and variables. 

### Build info
PIO build variant is in /variants/esp32dev-sa818-868/

Removing the argument -DUSE_SCREEN_SSD1306 will break the firmware. 
-DBOARD_HAS_PSRAM -mfix-esp32-psram-cache-issue is required for the S3. 


### Partition plan. 
# Name,   Type, SubType, Offset,  Size, Flags
nvs,      data, nvs,     0x9000,  0x5000,
otadata,  data, ota,     0xe000,  0x2000,
app0,     app,  ota_0,   0x10000, 0x640000,
app1,     app,  ota_1,   0x650000,0x640000,
spiffs,   data, spiffs,  0xc90000,0x360000,
coredump, data, coredump,0xFF0000,0x10000,


### KNOWN HW ISSUES ###

Bootloop at AT+SETFILTER
[  7473][I][rfModem.cpp:27] rfAnswerCheck(): SA Answer OK
[  7486][I][rfModem.cpp:94] RF_Init(): AT+SETFILTER=1,1,1
[  8986][I][rfModem.cpp:23] rfAnswerCheck(): ->+DMOSETFILTER:0

It _works_ after the dra818 warms up for around 5 min. The person that designed this board put 5V0 to a 3V3 device. sigh. 




### CHANGELOG OF ISSUES 




### Fix for Issue 1. 

This block of code at ln-1345 in main.cpp is commented out. This seems to be clobbering the I2C bus resulting in the below core 1 panic. AFSK_Poll is in /lib/AFSK.cpp ln-891. *** TODO *** here to further debug this. 

// #if defined(BOARD_ESP32DR)
//     if (AFSKInitAct == true) {
//         AFSK_Poll(true);
//     }
// #endif





### Issue 1.

### dump of the core 1 panic
[ 10508][I][rfModem.cpp:116] RF_Init(): AT+DMOSETVOLUME=4
[ 12008][I][rfModem.cpp:23] rfAnswerCheck(): ->+DMOSETVOLUME:0

[ 12008][I][rfModem.cpp:27] rfAnswerCheck(): SA Answer OK
[ 12008][[[12208[[12[ma][.]p][1][5] n.skGaS(.c p:sk :1P49 start49]
Ok> started                                                       tI]kmakN.cwo:1()5] tDDksplS((T sksk etPSr):tTatkd
EDDisplay> Guru Meditation Error: Core  1 panic'ed (LoadProhibited). Exception was unhandled.

Core  1 register dump:
PC      : 0x420279ef  PS      : 0x00060630  A0      : 0x8201cd89  A1      : 0x3fcebde0  
A2      : 0x00000000  A3      : 0x3fcebe40  A4      : 0x00000600  A5      : 0x3fcebe3c  
A6      : 0xffffffff  A7      : 0x3fc9c174  A8      : 0x00000000  A9      : 0xac060c2f  
A10     : 0x3fcec46c  A11     : 0x00000000  A12     : 0x00000001  A13     : 0x2dda0000  
A14     : 0x00000001  A15     : 0x3fcea640  SAR     : 0x00000000  EXCCAUSE: 0x0000001c  
EXCVADDR: 0x0000001c  LBEG    : 0x40056f5c  LEND    : 0x40056f72  LCOUNT  : 0xffffffff  


Backtrace: 0x420279ec:0x3fcebde0 0x4201cd86:0x3fcebe20 0x4200995d:0x3fcec460 0x42025895:0x3fcec4c0



### Second instance. 


Rebooting...
ESP-ROM:esp32s3-20210327
Build:Mar 27 2021
rst:0xc (RTC_SW_CPU_RST),boot:0xb (SPI_FAST_FLASH_BOOT)
Saved PC:0x420a762a
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fce3808,len:0x3ac
load:0x403c9700,len:0x9bc
load:0x403cc700,len:0x27c0
entry 0x403c98c0
[   256][I][esp32-hal-psram.c:96] psramInit(): PSRAM enabled
[   265][I][main.cpp:753] setup(): Start APRS-ESP V1.54.05c3b9a
[   265][I][main.cpp:754] setup(): Press and hold BOOT button for 3 sec to Factory Default config
[   268][I][esp32-hal-i2c.c:75] i2cInit(): Initialising I2C Master: sda=1 scl=2 freq=400000
[   278][W][Wire.cpp:301] begin(): Bus already started in Master Mode.
[   284][I][oled.cpp:51] OledStartup(): SSD1306 init ok
[   313][I][config.cpp:234] LoadConfig(): Loading config from EEPROM...
[  3320][I][config.cpp:262] LoadConfig(): EEPROM Check 87h=87h(708Byte)
[  3544][I][config.cpp:299] LoadConfig(): SPIFFS Check 87h=87h(708Byte)
[  3945][W][config.cpp:29] checkCallsignValid(): Callsign is invalid
[  3945][I][rfModem.cpp:39] RF_Init(): SA818/SA868 Init
[  4959][I][rfModem.cpp:56] RF_Init(): RF Modem powered up
[  5972][I][rfModem.cpp:85] RF_Init(): AT+DMOSETGROUP=1,144.8000,144.8000,0000,1,0000
[  7473][I][rfModem.cpp:23] rfAnswerCheck(): ->+DMOSETGROUP:0

[  7473][I][rfModem.cpp:27] rfAnswerCheck(): SA Answer OK
[  7486][I][rfModem.cpp:94] RF_Init(): AT+SETFILTER=1,1,1
[  8986][I][rfModem.cpp:23] rfAnswerCheck(): ->+DMOSETFILTER:0

[  8986][I][rfModem.cpp:27] rfAnswerCheck(): SA Answer OK
[  8998][I][rfModem.cpp:103] RF_Init(): AT+SETTAIL=0
[ 10499][I][rfModem.cpp:23] rfAnswerCheck(): ->+DMOSETTAIL:0

[ 10499][I][rfModem.cpp:27] rfAnswerCheck(): SA Answer OK
[ 10512][I][rfModem.cpp:116] RF_Init(): AT+DMOSETVOLUME=4
[ 12012][I][rfModem.cpp:23] rfAnswerCheck(): ->+DMOSETVOLUME:0

[ 12012][I][rfModem.cpp:27] rfAnswerCheck(): SA Answer OK
[ 12012][[[ 2002][I12ma3].c]p[I90m] t.ck[Pai): pps1 :1P49 stasted9
<L> started                                                        t[mkOnNeppo1k()] TaskDPsp()y(Tskk NetS>rkt:rtasd
EDDisplay> sGuru Meditation Error: Core  1 panic'ed (LoadProhibited). Exception was unhandled.

Core  1 register dump:
PC      : 0x420279ef  PS      : 0x00060630  A0      : 0x8201cd89  A1      : 0x3fcebde0  
A2      : 0x00000000  A3      : 0x3fcebe40  A4      : 0x00000600  A5      : 0x3fcebe3c  
A6      : 0xffffffff  A7      : 0x3fc9c174  A8      : 0x00000000  A9      : 0xac060c2f  
A10     : 0x3fcec46c  A11     : 0x00000000  A12     : 0x00000001  A13     : 0x2dde0000  
A14     : 0x00000001  A15     : 0x3fcea640  SAR     : 0x00000000  EXCCAUSE: 0x0000001c  
EXCVADDR: 0x0000001c  LBEG    : 0x40056f5c  LEND    : 0x40056f72  LCOUNT  : 0xffffffff  


Backtrace: 0x420279ec:0x3fcebde0 0x4201cd86:0x3fcebe20 0x4200995d:0x3fcec460 0x42025895:0x3fcec4c0




ELF file SHA256: a385f615e875d0a3

Rebooting...