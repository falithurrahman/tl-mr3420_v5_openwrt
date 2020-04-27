### Description
This folder contains step-by-step tutorial installing exroot on TP Link MR4320 V5 Router running with OpenWRT firmware. This installation step refer to official page. I made this tutorial to summarize scattered info that i found on the internet. If it violate any copyright, i will erase it.

#### Software needed
* Putty <br>
https://www.putty.org/

#### Installation steps
* Make sure we are connected to openWRT router, whether it's through LAN or WiFi.
* Configure /etc/config/fstab to mount the rootfs_data in another directory in case we need to access the original root overlay to change our extroot settings. It can be done by typing command below in putty.
>DEVICE="$(awk -e '/\s\/overlay\s/{print $1}' /etc/mtab)" <br>
>uci -q delete fstab.rwm <br>
>uci set fstab.rwm="mount" <br>
>uci set fstab.rwm.device="${DEVICE}" <br>
>uci set fstab.rwm.target="/rwm" <br>
>uci commit fstab <br>
* Then we need to configure extroot, see what partitions we have by typing this command <br>
> #block info <br>
/dev/mtdblock2: UUID="9fd43c61-c3f2c38f-13440ce7-53f0d42d" VERSION="4.0" MOUNT="/rom" TYPE="squashfs" <br>
/dev/mtdblock3: MOUNT="/overlay" TYPE="jffs2" <br>
/dev/sda1: UUID="fdacc9f1-0e0e-45ab-acee-9cb9cc8d7d49" VERSION="1.4" TYPE="ext4" <br>
* Here we see mtdblock devices (partitions in internal flash memory), and a partition on /dev/sda1 that is on a usb flash drive.<br>
Format the partition /dev/sda1 as ext4 by typing this command. <br>
> mkfs.ext4 /dev/sda1
* Now we configure /dev/sda1 as new overlay via fstab uci subsystem with these command. If we have a swap partition it will also get recognized and added automatically.<br>
> DEVICE="/dev/sda1" <br>
eval $(block info "${DEVICE}" | grep -o -e "UUID=\S*") <br>
uci -q delete fstab.overlay <br>
uci set fstab.overlay="mount" <br>
uci set fstab.overlay.uuid="${UUID}" <br>
uci set fstab.overlay.target="/overlay" <br>
uci commit fstab <br>
* After that, we need to transfer the data. We will move content of the current overlay inside the external drive, by typing these commands.
> mount /dev/sda1 /mnt <br>
cp -a -f /overlay/. /mnt <br>
umount /mnt <br>
* We are finished, now reboot the device and test if it's working or not. To test it using web interface, these are the steps: <br>
LuCI → System → Mount Points should show USB partition mounted as overlay. <br>
LuCI → System → Software should show free space of overlay partition. <br>
* There are some extra steps to preserve software package lists across boots. Saving opkg status in the /usr/lib/opkg/lists stored on the extroot, instead of in RAM, saves some RAM and keeps package lists available after a reboot. We can do that using command line in putty. Here is the command.
>sed -i -r -e "s/^(lists_dir\sext\s).*/\1\/usr\/lib\/opkg\/lists/" /etc/opkg.conf <br>
opkg update <br>
* Congratulations, now we have finished configuring exroot in our router. We have extra space to install any software we need in this router.

#### Reference
* OpenWRT Extroot configuration <br>
Since we are using TL MR3420 v5, just jump into instruction for devices ≥ 8 MiB flash. <br>
https://openwrt.org/docs/guide-user/additional-software/extroot_configuration