### Description
This folder contains step-by-step tutorial on configuring hard disk storage to be accessible from network. OpenWrt in 2019 supports two versions of Samba. Samba3 and Samba4. Many of the existing guides are based on Samba3. It's important before beginning samba setup that you get our disks sorted. Installing filesystem support, mounting, basic os level permissions are all something Samba sits on top of. Beginning the setup of the Samba server before mounting and verifying a working disk setup will make things much harder in the long run.

#### Installation Steps
*  We have to connect our router to internet. If some of you still confused how to do it, please refer to "Android USB Tethering" installation steps.
* Connect to our openWRT router, through LAN cable or Wifi.
* Open Putty, and SSH to our openWRT IP address at 192.168.1.1.


#### Reference
* SMB Samba share overview <br>
https://openwrt.org/docs/guide-user/services/nas/samba_configuration
* Share USB Hard-drive with Samba using the Luci web-interface <br>
https://openwrt.org/docs/guide-user/services/nas/usb-storage-samba-webinterface
