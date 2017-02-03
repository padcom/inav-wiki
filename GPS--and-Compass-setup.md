iNav supports Ublox, DJI NAZA, NMEA, multiwii's i2c-nav board and MultiWiiCopter's i2c-gps modules

Tested and confirming working protocols are Ublox and DJI NAZA


Recommended GPS are M8N versions (e.g. [Ublox NEO-M8N APM version (Galileo compatible)](http://www.banggood.com/Mini-Ublox-M8N-GPS-Module-NEO-M8N-GPS-for-APM-2_52_62_8-CC3D-SP-Racing-F3-Naze32-Flip32-PX4-p-1035454.html) and [Beitian BN-880](http://www.banggood.com/UBLOX-NEO-M8N-BN-880-Flight-Control-GPS-Module-Dual-Module-Compass-p-971082.html?p=ZL241728738232015106) )
Older versions as M6N and M7N also work, but the new M8N is far superior. Most GPS modules have a built in magnetometer (compass), but there are also some available without e.g. [Beitian BN-180](http://www.banggood.com/Beitian-BN-180-Flight-Control-GPS-Module-Dule-Module-without-Compass-p-1040322.html?p=ZL241728738232015106). 

With default settings iNav will configure the GPS automatically, **there is no need for configuring it manually** using software like u-center. Nevertheless you have to configure your FC with iNav to receive the GPS signals.

If you want to use the external magnetometer (built in in your GPS) and you have a FC with the same magnetometer (HMC5883L is very common), you have to disable it physically on your FC: remove chip from board or cut a trace. You can't use two identical chips/magnetometers on the same I2C bus. 
  * When using DJI NAZA gps this is not true, DJI NAZA sends compass over serial and does not use the I2C bus)
  * On MPU9250 board internal magnetometer is an AK8963, most GPS pucks are HMC5883L. So no need to remove hardware, only choose which one to use with cli command `mag_hardware`

Otherwise just use the internal FC magnetometer, but keep aware of magnetic interference (not recommended).

##Getting started with Ublox GPS
- Physically connect your GPS to your FC using UART or softserial. Connect RX from GPS to TX on FC, TX from GPS to RX on FC
- Activate GPS in the ports tab in cleanflight/iNav configurator and set it to 57600 using UART or 19200 using softserial (on your chosen port)
- Activate GPS in the configuration tab, set it to ublox.

- Using external compass:
 * Connect the magnetometer to I2C ports (SCL/SDA) Be aware that with SDA/SLC lines connected the flight battery must often be connected to access configurator and power up the magnetometer. 
 * Select your newly connected magnetometer by using `mag_hardware` CLI command. Example `set mag_hardware = auto` if you only have one magnetometer connected.
 * Most built in magnetometers are on the underside and rotated 180 degrees, use example `align_mag = CW180FLIP`. If compass is not working properly in all directions then either think and figure out the direction of your mag, or go through them all until it works as expected.
 * F3 based board and newer uses default automatic magnetic declination, if your on F1 board or want to change magnetic declination manually you have to set correct declination of your spesific location, which can be found here: www.magnetic-declination.com. If your magnetic declination readings are e.g. +3° 34' , the value entered in the iNav configurator is 3.34 (3,34 in some locales). In the CLI, the same effect would be `set mag_declination = 334`. For west declination, use a minus value, e.g. for 1° 32' W, `set mag_declination = -132`. In all cases (both CLI and GUI), the least significant digits are **minutes**, not decimal degrees.
 * Calibrate your compass according to [compass calibration](https://github.com/iNavFlight/inav/wiki/Sensor-calibration#compass-calibration)


Note to change magnetic declination manually on F3 or newer board you have to turn off automatic function. `set inav_auto_mag_decl = OFF`.


##Getting started with DJI NAZA GPS
NOTE: By default F1 processors do not support DJI GPS. Most F3 processors do - check hardware support map.
F1 can support DJI if you compile your own build with unused features removed.

- Physically connect your GPS to your FC using UART. Connect RX from GPS to TX on FC, TX from GPS to RX on FC
- Activate GPS in the ports tab in cleanflight/iNav configurator and set it to 115 200 on correct UART
- Type this in CLI

`feature GPS`

`set gps_provider = NAZA`

`set mag_hardware = GPSMAG`

`set align_mag = CW180FLIP`

Default DJI GPS puck pointing forward is set with CW180FLIP, but can be changed with CW0FLIP, CW90FLIP, CW180FLIP or CW270FLIP

 * F3 based board and newer uses default automatic magnetic declination, if your on F1 board or want to change magnetic declination manually you have to set correct declination of your spesific location, which can be found here: www.magnetic-declination.com. If your magnetic declination readings are e.g. +3° 34' , the value entered in the iNav configurator is 3.34 (3,34 in some locales). In the CLI, the same effect would be `set mag_declination = 334`. For west declination, use a minus value, e.g. for 1° 32' W, `set mag_declination = -132`. In all cases (both CLI and GUI), the least significant digits are **minutes**, not decimal degrees.
 * Calibrate your compass according to [compass calibration](https://github.com/iNavFlight/inav/wiki/Sensor-calibration#compass-calibration)

Note to change magnetic declination manually on F3 or newer board you have to turn off automatic function. `set inav_auto_mag_decl = OFF`.


Thats it!


## SBAS

When using a UBLOX GPS the SBAS mode can be configured using `gps_sbas_mode`.

The default is AUTO.

| Value    | Region        |
| -------- | ------------- |
| AUTO     | Global        |
| EGNOS    | Europe        |
| WAAS     | North America |
| MSAS     | Asia          |
| GAGAN    | India         |
| NONE     | NONE         |

If you use a regional specific setting you may achieve a faster GPS lock than using AUTO, but keep in mind to change it if you change your location for holidays etc.

This setting only works when `gps_auto_config= ON`


## Issues
- No GPS lock: often due to electric noise from flight controller or other equipment such as 1.2ghz video TX. Try getting the GPS as far away as possible from electric noise emitting parts as the FC, ESC´s or power cables. Placing the GPS on a mast is also a common way, you can further try shielding with aluminum or copper foil. Don´t place the GPS inside the frame.
- "Toilet bowling": in the beginning the copter holds its position and then starts to make bigger and bigger circles, you probably have your magnetometer not calibrated correctly or it’s interfered from the magnetic field of your power lines or the beeper.
If you are using your FC onboard mag, try to place the the FC as far away as possible from the magnetic interference causing parts e.g. mounting it on/under the top plate on small racers.