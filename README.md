# enable-aspm


ASPM Tuning script

This script lets you enable ASPM on your devices in case your BIOS
does not have it enabled for some reason. If your BIOS does not have
it enabled it is usually for a good reason so you should only use this if
you know what you are doing. Typically you would only need to enable
ASPM manually when doing development and using a card that typically
is not present on a laptop, or using the cardbus slot. The BIOS typically
disables ASPM for foreign cards and on the cardbus slot. Check also
if you may need to do other things than what is below on your vendor
documentation.

To use this script You will need for now to at least query your device
PCI endpoint and root complex addresses using the convention output by
lspci: [<bus>]:[<slot>].[<func>]

For example:

03:00.0 Network controller: Atheros Communications Inc. AR9300 Wireless LAN adaptor (rev 01
00:1c.1 PCI bridge: Intel Corporation 82801H (ICH8 Family) PCI Express Port 2 (rev 03)

The root complex for the endpoint can be found using lspci -t

For more details refer to:

http://wireless.kernel.org/en/users/Documentation/ASPM

You just need to modify these three values:

 ~~~
#ROOT_COMPLEX="00:1c.1"
#ROOT_COMPLEX="00:1e.0"
ROOT_COMPLEX="00:1c.3"
#ENDPOINT="03:00.0"
#ENDPOINT="05:00.0"
ENDPOINT="05:00.0"
~~~
  
We'll only enable the last 2 bits by using a mask
of :3 to setpci, this will ensure we keep the existing
values on the byte.


| Hex | Binary | Meaning |
|---|---|---|
| 0  | 0b00 | L0 only |
| 1  | 0b01 | L0s only |
| 2  | 0b10 | L1 only |
| 3  | 0b11 | L1 and L0s |
