### Description
This folder contains step-by-step tutorial installing openWRT on TP Link MR4320 V5 Router. There are so many tutorials scattered on the internet on how to flash openWRT firmware to this router. I had to test it one by one, whether it's working or not. That waste a lot of time. Since no clear tutorial on how to flash openWRT firmware on TP Link MR4320 V5, so I decided to make it myself. The only working steps to flash an OpenWrt image to TL-MR3420 v5 is to use tftp recovery mode in U-Boot. I'll explain it below.

#### Software Needed
* TFTP Server <br>
https://tftpd64.software.informer.com/
* OpenWRT Firmware for TL MR4320 V5<br>
http://downloads.openwrt.org/releases/19.07.2/targets/ramips/mt76x8/openwrt-19.07.2-ramips-mt76x8-tplink_tl-mr3420-v5-squashfs-tftp-recovery.bin

#### Installation Steps
* Download and install TFTP Server with link i provide above.
* Download openWRT firmware with link i provide above.
* Configure Ethernet static IP on your computer to '192.168.0.225'.<br>
If you're using windows, go to Control Panel\Network and Internet\Network and Sharing Center. Then choose 'Change adapter setting'. You will see ethernet and WiFi adapter. Choose ehternet adapter and configure IPv4 to '192.168.0.225'.
* Rename openWRT firmware that has been downloaded to 'tp_recovery.bin'. Then, place it in the tftp server directory. By default, tftp server directory will be 'C:\Program Files\Tftpd64'. You can go to that directory and copy-paste 'tp_recovery.bin' or you can choose other directory with browse command and copy-paste the firmware to directory of your desire.
* Connect PC with one of LAN ports available on router using LAN cable.
* Press the reset button on the router, power up the router and keep button pressed for around 6-7 seconds, until device starts downloading the file.
* Router will automatically download file from server, write it to flash and reboot.
* Wait for a few minutes, configure ethernet static IP on your computer to '192.168.1.225'.
* Type '192.168.1.1' on your web browser. Congratulations, you've successfully flash openWRT firmware to your router.