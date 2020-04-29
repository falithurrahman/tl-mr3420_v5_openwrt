### Description
This folder contains step-by-step tutorial to forward internet access from android phone to TP Link MR3420 router through android USB tehtering connection. By using this method, we can have wireless acess point with internet access provided from Android phone. This installation step refer to official page.

#### Software needed
* Putty <br>
https://www.putty.org/

#### Installation steps
* In order to finish this setup, we need an active internet connection. So we have to connect this router to internet. I can provide you with 2 solutions. First, if you have another router with internet access, you can just plug LAN cable to that router since we have 4 LAN connection in TP Link MR3420. Second, connect to another wireless access point with internet access. I used my android portable hotspot and make it wireless access point. Here is the complete step: <br>
    1. Open UCI web interface at 192.168.1.1 on web browser.
    2. Go to Network -> Wireless.
    3. On Wireless overview, click scan on radio0.
    4. Seek the android wireless portable hotspot, and when it's found click join network.
    5. Now we have internet access in our openWRT router.
* Connect to our openWRT router, through LAN cable or Wifi.
* Open Putty, and SSH to our openWRT IP address at 192.168.1.1.
* After successfully connected, type command below. It will install some package to get usb tethering on android support.
    ```
    opkg update
    opkg install kmod-usb-net kmod-usb-net-cdc-ether kmod-usb-net-rndis
    ```
* Connect android to the USB port of the router with the USB cable and then enable USB Tethering from the Android settings.
* Back to putty command line, type 'dmesg' and you will see something like this
    ```
    [55100.327595] rndis_host 1-1.1:1.0 usb0: register 'rndis_host' at usb-101c0000.ehci-1.1, RNDIS device, 0a:94:ee:f0:44:b6
    ```
    The last line tells us that this new “RNDIS device” was bound to interface usb0. This is our android usb tethering device. 
* Create a new interface called TetheringWAN (or however you like), and bind to it the new *usb0* network device  set the protocol to DHCP client mode, and place it into the WAN firewall zone. To do that using putty, type command below.
    ```
    uci set network.TetheringWAN=interface
    uci set network.TetheringWAN.proto='dhcp'
    uci set network.TetheringWAN.ifname='usb0'
    uci set firewall.@zone[1].network='wan wan6 TetheringWAN'
    uci commit
    ```
* After committing the changes the new TetheringWAN should be activated.
If it does not, write command below in putty
    ```
    ifup TetheringWAN
    ```
    or restart it with the buttons you find in the Interface page of LuCi Web interface.