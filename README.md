# zVM-files
My standard configuration files for setting up z/VM, including TCP/IP.

## Why?
I find myself often having to create a new configuration on a new system, or consulting on how to set up something on z/VM TCP/IP. Rather than creating from scratch (using increasingly rusty memory) or from pages on the Net, it seemed like a good idea to put them somewhere accessible.

Inspired by https://github.com/davewongillies/dot-files :)

## Content
Examples include:

* CP
  * SYSTEM CONFIG
  * LOGO files
    * LOGO CONFIG
    * INPTAREA LOGO
* TCPIP
  * PROFILE TCPIP (with the port reservations I usually run)
    * including TLS for TN3270
  * SYSTEM DTCPARMS 
    * TCPIP via VSwitch
    * Other service configurations (e.g. SSL Servers)
  * SNMPD configuration
  * UFTD configuration
  * FTPD configuration
* DIRMAINT
  * EXTENT CONTROL
  * AUTHFOR CONTROL
* Performance Toolkit
  * FCONX $PROFILE
  * FCONRMT SYSTEMS
  * FCONRMT AUTHORIZ
