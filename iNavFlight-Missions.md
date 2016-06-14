# Overview

iNav supports autonomous flight using waypoints. In order to use this capability, it is also necessary to utilise and configure some supporting technologies, including:

* A GCS (Ground Control Station). The GCS will typically provide functions to create waypoint (WP) missions, upload WP missions to the flight controller (FC), validate the mission, execute the mission and log the mission;
* Telemetry Hardware. In order to transfer the mission to the FC and monitor the mission in real time during mission execution it is necessary to install and configure a telemetry system between the GCS and the multicopter.

This wiki topic describes the software currently available and some of the telemetry options. 

# Ground Control Stations

Currently there are two GCS applications widely used for iNav mission management, ezgui <http://ez-gui.com/> (Android) and mwp <https://github.com/stronnag/mwptools> (Linux). In future, other options may become available, particularly if the MAVLink protocol becomes supported by iNav.

ezgui and mwp both support mission planning (they share a common mission definition file format, so missions can be used in either tool), mission monitoring and mission logging. ezgui also provides a FC configuration capability. 

## EZGui (Android)

ezgui can be downloaded from the [Google Play store](https://play.google.com/store/apps/details?id=com.ezio.multiwii&hl=en_GB). There is a free version and a (very reasonably priced) paid-for version with additional functionality. The application is not open source. 

## mwp (Linux)

mwp can be downloaded from [Github](<https://github.com/stronnag/mwptools>). mwp is open source (GPL 2). It is available only as a source distribution and it is necessary to compile and install the application. Build instructions and dependencies are provided for Ubuntu and Fedora. Arch Linux users can install mwp from the AUR ([Arch User Repository](https://aur.archlinux.org/packages/mwptools-git/)). 

In addition to mission planning and logger, mwp also supports the replay of blackbox logs against a geospatial background (requires [blackbox-tools](https://github.com/cleanflight/blackbox-tools)). mwp also includes numerous poorly documented scripts for mission and blackbox analysis.

## Potential solutions for other platforms

mwp can be run in a virtual machine on MS Windows and OSX / macOS, using virtualisation tools such as VirtualBox and Parallels. 

Mobile Flight: Configuration and ground control app for Cleanflight on iPhone http://www.rcgroups.com/forums/showthread.php?t=2601895&highlight=ios appears to have some support for iNav an GPS logging at least. This application is in an early stage of development.

WinGUI is a Windows program developed for Multiwii nav. It is currently somewhat abandoned, but would be a viable basis for developing a Windows program for iNav navigation (or better, supporting both Multiwii and iNav, as do the other tools described here. Should anyone wish to rescue this fine application, the source code (GPL v3) may be found at https://code.google.com/archive/p/mw-wingui/.

# Telemetry Hardware

In order to transfer missions from the GCS to the flight controller, and to monitor / log flight data, it is necessary to   establish a data link between the GCS and the multirotor. Some popular technologies include:

* Bluetooth
* 3DR (433Mhz / 915Mhz)
* WiFi (ESP8266)
* HR-12 (433Mhz, similar to 3DR)
 
## Bluetooth

Bluetooth is the easiest solution to get working with minimal effort. A cheap HC-06 BT module is all that is needed (the phone or laptop built-in BT is used on the ground station). It's disadvantage is the range, for most users data loss / dropout will occur over 20m. It's thus useful for testing out configurations, but for many users the limitation of range will call for another solution. 

# Telemetry Protocols
## MSP
## LTM
## MAVLink

# Configuring the Flight Controller
## Ports & port sharing

# Mission Planning

# Mission / Flight Monitoring
