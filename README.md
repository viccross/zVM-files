# tcpip-zVM
My standard configuration files for setting up TCP/IP and services on z/VM.

## Why?
I find myself often having to create a new configuration on a new system, or consulting on how to set up something on z/VM TCP/IP. Rather than creating from scratch (using increasingly rusty memory) or from pages on the Net, it seemed like a good idea to put them somewhere accessible.

Inspired by https://github.com/davewongillies/dot-files :)

## Content
Examples include:
* PROFILE TCPIP (with the port reservations I usually run)
  * including TLS for TN3270
* SYSTEM DTCPARMS 
  * TCPIP via VSwitch
  * Other service configurations (e.g. SSL Servers)
* SNMPD configuration
* UFTD configuration
* FTPD configuration
