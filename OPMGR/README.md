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
See the notes below for more details.

`VMEVDECO.EXEC`

This file is an example of the extensive pre-processing available by using an external sender routine.
It takes a VMEVENT in 'binary' form and expands it into a text representation.
VM Events decoded are current up to the enhancement APAR of 2023.

## Additional Notes

Here are some additional points on the config.

### Translation Table

For this full set of configurations to work, including sending to Syslog, a translation table must be made available.
If an explicit translation table is not provided, each run of `SYSLOG2 EXEC` will result in a message appearing in the OPMGR log.
The translation table is selected in a `Socket()` call on line 33 of `SYSLOG2 EXEC`:
```
RetVal=Socket('SETSOCKOPT',Sock,'SOL_SOCKET','SO_ASCII','On SYSLOG')
``` 
The final parameter specifies the name of the translation table.
With this call, REXX Sockets looks for a file called `SYSLOG TCPXLBIN` on an accessed disk.
This table is not a supplied table, but is a copy of one of the IBM-supplied tables (to avoid clashing with other translation uses).

The table that provides a suitable translation from EBCDIC is the supplied table `00370850 TCPXLBIN`.
Copy this file from the TCPMAINT 592 disk to the OPMGRM1 198 disk (or SFS directory) as `SYSLOG TCPXLBIN`.

### Code page issues
As described in the notes above, the characters `[` and `]` seem to be problematic when it comes to translation.
The original 7-bit ASCII to EBCDIC tables do not appear to have these characters at all, and there are two different mappings present in various tables.
The `00370850 TCPXLBIN` table maps `[` (ASCII '5B'X) to EBCDIC as 'BB'X, and `]` (ASCII '5D'X) to EBCDIC as 'BD'X.

However, the default code page of the x3270 emulator is a custom code page called "Bracket" and described as "CP037, modified".
This codepage maps `[` to EBCDIC as 'AB'X and `]` to EBCDIC as 'AD'X.
This means that if you display the code on the x3270 emulator using its default code page, the code looks corrupted: instead of `[` and `]` one sees `Ý` and `¨`.
This can be resolved by changing the code page in x3270 to a 'standard' setting like "U.S. English (CP 037)" (or the same-named setting under the "Euro" menu, that provides CP 1140).

> The x3270 wiki does not explain why the default code page is this modified version.
The choice of the name "Bracket", however, implies that it is correcting a perceived issue with the display of these characters.
