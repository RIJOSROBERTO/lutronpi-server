# lutronpi-server
LutronPi server connecting Lutron bridges to SmartThings hub - node.js

LutronPi 2.x server is based on Nate Schwartz' LutronPro 1.x package for node.js and SmartThings
Due to essential divergences, LutronPi server is not backwards compatible with Nate's earlier work,
and will work only with the corresponding LutronPi device handlers and service manager running
on the SmartThings platform (namespace: lutronpi)

FUNCTION: The LutronPi application serves to connect Lutron lighting bridges (SmartBridge, SmartBridge Pro,
RA2/Select repeater) to a local hub of the Samsung SmartThings home authomation platform. There is an
'official' Lutron-to-SmartThings integration, which unfortunately does not integrate the Lutron Pico
remote button fobs into SmartThings.  LutronPi does connect the Pico buttons to SmartThings, so long as
the Picos are paired to a Lutron SmartBridge Pro or RA2/Select repeater (not to a standard retail SmartBridge).

FORM: The LutronPi application comprises two elements:
  1. A server application running under node.js on an independent computer (e.g. a Raspberry Pi, or a desktop,
  laptop, or potentially a NAS drive, etc.).  The server must be on the same local Ethernet subnet as the
  Lutron bridge(s) and the SmartThings hub.
  2. A SmartThings "SmartApp" service manager application, along with its associated device handlers. These
  Groovy modules all run on the SmartThings platform, with functions both local to the hub and in the cloud.

UPDATES: beyond the LutronPro 1.x package and its support of Lutron dimmers, switches and 3BRL Picos:
  * Connects to multiple Lutron bridges simultaneously, including:
    standard SmartBridge, SmartBridge Pro, & RA2/Select repeater
  * Supports all (almost all?) Pico models: 2B, 2BRL, 3B, 3BRL, 4B
  * Supports Lutron shades (only partially tested, feedback solicited!)
  * Interactive authentication of Lutron bridges, so user/password info need not be stored in the clear
  * Automatically enable Telnet interface on Pro and RA2/Select to allow Pico interface to SmartThings
  * Maintains and automatically restores connections to bridges and hub over a variety of communications
    disruptions, including temporary disconnections, loss of bridge/hub power, change of IP address
  * Automatically discovers Lutron bridges and SmartThings hub on the local network, with manual overrides
  * Notification to SmartThings service manager of server restart and/or change of IP address, with
    automatic restoration of configurations and device status refresh (e.g. light levels)
  * Pico button handling modified to reliably handle multiple simultaneous Pico events
  * All Pico configuration is maintained in the SmartThings service manager and Pico device handlers;
    no configuration is required at the (node.js) server for push-to-hold times, hold-to-repeat, or timeouts.
  * Pico buttons transmit press and release events to SmartThings in addition to push and hold events
  * Pico buttons will (optionally) time out push/hold after 6 seconds
  * SmartThings can (optionally) trigger Pico-associated events on the Lutron bridge(s)
  * SmartThings can trigger Lutron scenes
  * Dimmers now support level raise/lower commands from SmartThings
  * Dimmers now support fade-over-time (on and/or off) - only on Pro and RA2/Select
  * Supervisory and bridge-related functions divided into separate modules and heavily refactored
  * Infrastructure for plug-in modules to allow connection of other 'bridge' types alongside Lutron
 
 INSTALLATION: npm install lutronpi
 
 It is most convenient to install and start the SmartThings SmartApp service manager and device handlers
 before running the LutronPi server on your node.js platform.

 N.B. Neither LutronPi nor node.js provides automatic restart of the server application when its host
 computer shuts down, power-fails, or otherwise restarts.  This restart capability must be set up separately
 on your host platform.  The PM2 process manager is one of several viable tools for this purpose.

 OPERATION: node lutronpi/lutronpi   or   node lutronpi/LutronPiServer
 (example is assuming the server application has been installed in the lutronpi/ directory)
 
 Bridge authentication .key files will be stored in (and expected in) the current directory from which
 the application was started, regardless of where the application itself is installed.
 Running lutronpi.js directly, as above, will provide automatic discovery of Lutron bridges and the
 SmartThings hub.   If (Bonjour/mDNS) discovery does not work on your platform, modify LutronPiServer.js
 as required for manual specification of your bridge and/or hub connections (details within).
 