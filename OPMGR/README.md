# OPMGR
Files for configuration of the IBM Operations Manager for z/VM (OPMGR).

## Content
There's a directory for each minidisk for which I have files.
Applying the file is as simple as copying the file to the named minidisk, using appropriate transfer attributes (usually RECFM F, LRECL 80).

* OPMGRM1.198

### OPMGRM1.198

`COMMON.CONFIG`

This file is as-installed in z/VM ESI.
It contains the activating clauses for the syslog enablement.

`SYSLOG2.EXEC`

This file was developed using REXX Sockets to transmit a message via Syslog (UDP port 514).
Standard OPMGR configuration for Syslog uses a defined remote in OPMGR, but we are directly calling REXX Sockets here.
This implementation allows us to do pre-processing of the message before sending.

The target IP address of the Syslog server is currently hard-coded in the EXEC.
This could be enhanced to make it a parameter, or included from a configuration file or GLOBALV.
If this is re-implemented, care should be taken to ensure that performance is not impacted.

> Note: Take care with the `[` and `]` characters on line 48.
These characters are often not correctly preserved through codepage translation.
By the time they reach the Syslog daemon in ASCII/UTF-8, these characters are intended to be `[` and `]` and are used to delimit the selector of the structured field message.
You may have to experiment with these due to the codepage in use and the translation table you select.
The subsequent line (commented) shows one example of characters that may have to be used to get the right result at the Syslog daemon.

`VMEVDECO.EXEC`

This file is an example of the extensive pre-processing available by using an external sender routine.
It takes a VMEVENT in 'binary' form and expands it into a text representation.
VM Events decoded are current up to the enhancement APAR of 2023.

