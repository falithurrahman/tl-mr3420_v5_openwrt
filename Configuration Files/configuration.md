### Description
This folder contains basic configuration files from my TPLink MR3420 v5 router. These configuration files are created after TPLink MR3420 v5 is installed with exroot and android usb tethering. Then i make wireless network to share internet connection from android usb tethering.

To create these files, i used these command on putty command line.
| Command  | File  |
| --- | ----------- |
| uci export network | Package_network |
| uci export dhcp | Package_dhcp |
| uci export firewall |  Package_firewall |
| uci export wireless | Package_wireless |