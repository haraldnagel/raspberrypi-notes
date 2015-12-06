Raspberry Pi Initial Configuration
===
These are the steps that I follow when initially configuring a Raspberry Pi. This is based on the *Raspberry Pi Software Configuration Tool (raspi-config)* which comes with **2015-11-21-raspbian-jessie-lite.img**. The document originally here was for Raspbian Wheezy and has [moved](InitialConfigurationWheezy.md).

First Boot
---
To access raspi-config you have two options:

1. Plug in a monitor and keybard.

2. The system has sshd listening so if you can figure out its IP address remotely (possibly consulting your router or DHCP server's logs, or using `nmap`) you can configure it by accessing via ssh. The pi user's default password is: raspberry. Once in you can execute `sudo raspi-config`.

raspi-config
---
To get around you typically use the arrow keys, tab, and enter, as you do in the default Debian setup environment.

1. Expand Filesystem - if you are using a card larger than the Raspbian image this is a no-brainer.

2. Change User Password - the default Raspbian distribution creates a standard login: "pi". This option allows you to change that default user's password. Highly recommended.

3. Boot Options - I select `Console` (which I believe is the default) becuase I don't want any autologin. I don't think Desktop is going to work without manually installing all the X packages.

4. Wait for Network at Boot - I choose `Fast Boot without waiting for network connection`.

5. Internationalisation Options - as you can tell by the spelling here, US English is not the default i18n configuration. In this option, I choose:
	1. Change Locale - I deselect the default (en_GB.UTF-8) and select the appropriate locale for me (en_US.UTF-8). It will then confirm that you want to use this locale for the system environment (you do). This will dump you back to the main configuration menu, you'll have to go back into "I18n Options" to change further settings.
	2. Change Timezone - The default timezone is UTC which is appropriate for a Unix-like system. A timezone can be selected for each individual user by following the steps described by running `tzselect`. If you go into this option, UTC is under "None of the above". I leave my systems set to UTC.
	3. Change Keyboard Layout - unless you are accessing `raspi-config` via ssh, you'll almost certainly want to do this and change to an appropriate keyboard layout. For example, in the default UK keyboard layout Shift-3 results in a pound sign (£) rather than a hash (#) which makes commenting lines out (for example, in `/etc/network/interfaces`) a challenge (the backslash (\\) key in a UK keyboard layout types the hash).

6. Enable Camera - this is for the [Raspberry Pi Camera Board](http://www.adafruit.com/products/1367).

7. Add to Rastrack - if you want your installation to show up on the [Rastrack Raspberry Pi Map](http://rastrack.co.uk), choose this.

8. Overclock - To date, I do not overclock my Raspberry Pis.

9. Advanced Options - there are a few things to change here:
	1. Overscan - this setting may be needed when using the Raspberry Pi in desktop mode.
	2. Hostname - obviously it's good to set a hostname, by default it selects "raspberrypi".
	3. Memory Split - configures the memory split between video memory and available memory. In Jessie Lite this defaults to 0 which is where I leave it.
	4. SSH - I'm mostly accessing over the network so I enable this.
	5. Device Tree, SPI, I2C, Serial - this stuff largely depends on what the usage of your Pi will be. For general headless network use, I leave these all as-is.
	7. Update - updates the `raspi-config` tool we are currently using. A little late for that, don't you think?

We're done! If you select "Finish" it will prompt for a reboot and you are good to go!

Initial Login
---
Upon first boot, you'll want to log into the Raspberry Pi with the "pi" login and the password you specified during configuration. If you didn't change the default password, it is: raspberry. Some steps you might take at this point:

* Remote access is enabled, however by default the Raspberry Pi is configured to use DHCP. I generally use DHCP reservations for headless Pis these days but I used to configure a static IP address. If you'd like to take that approach, it's done following the same steps as in Debian:
	1. Edit `/etc/network/interfaces` (I use `vi`. By default, `nano` is installed and may be easier if you are unfamiliar with vi).
	2. Comment out `iface eth0 inet dhcp` by putting a hash (#) in front of the line (if you didn't change your keyboard layout from UK and you are using a US keyboard, you may need to to use the backslash (\\) key to type a hash).
	3. Add the following (obviously customize for your address and gateway):
	
			iface eth0 inet static
				address 10.0.0.23
				netmask 255.255.255.0
				gateway 10.0.0.1
	4. If you want to enable IPv6, as well, you can add `ipv6` on its own line to the end of `/etc/modules` and `iface eth0 inet6 auto` on its own line in `/etc/network/interfaces`.
	5. You can either reboot at this point or `sudo ifdown eth0 && sudo ifup eth0` to reset the network interface (you can see the current configuration with `/sbin/ifconfig`).
	6. If you do intend to continue working remotely, don't forget test SSH access before disconnecting the monitor and keyboard so that you don't have to hook everything back up if you've made a typo. I do not speak from personal experience on this at all.

From this point on, you can SSH in from another workstation to continue configuration if you have been working at the console and remote access would be easier (it might be due to the ability to have cut and paste).

* You may wish to change the time zone for the "pi" user. You can run `tzselect` to walk you through the time zone options which will end up providing you a line to add to the end of your `.profile` file. Cut and paste is handy here.

* At this point it's probably a good idea to ensure Raspbian is up-to-date. I typically use `sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade`.

* The Raspberry Pi does not have a real-time clock so it appears to [use the time saved during the last shutdown](http://captainbodgit.blogspot.com/2013/10/raspberrypi-keeping-time-without.html) to set the clock upon boot if it cannot reach a time server. It might be a good idea to take a look at the NTP configuration and ensure that it's appropriate for your environment. By default, the Raspberry Pi will use NTP servers sent out by your DHCP server. You can see the current status with `ntpq -p`, you are looking for a line with an asterisk (*) in the first column indicating that there's a valid time source (you might need to wait a while after a reboot for things to sync up). If you need assistance in configuring NTP, I'd recommend reviewing: [How do I use pool.ntp.org?](http://www.pool.ntp.org/en/use.html).

  If you wish to manually configure NTP while still using DHCP, you can comment out the `option ntp_servers` line in `/etc/dhcpcd.conf` (put a `#` at the beginning of the line or delete the line entirely), remove the `/var/lib/ntp/ntp.conf.dhcp` file, and then configure NTP normally (`/etc/ntp.conf`). If you do not take this step, every time the DHCP lease is renewed it will update the `/var/lib/ntp/ntp.conf.dhcp` file and use those NTP servers.
 
  Or, with a GPS, you can set up your [Raspberry Pi as an authoritative NTP server](NTPServer.md).