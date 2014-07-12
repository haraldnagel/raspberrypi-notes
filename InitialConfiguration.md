Raspberry Pi Initial Configuration
===
These are the steps that I follow when initially configuring a Raspberry Pi. This is based on the *Raspberry Pi Software Configuration Tool (raspi-config)* which comes with **2014-06-20-wheezy-raspbian.img**.

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

3. Enable Boot to Desktop/Scratch - the default is a boot to console - if you wish to boot to the X Window desktop it can be selected here. For headless use, you want to boot to the console.

4. Internationalisation Options - as you can tell by the spelling here, US English is not the default i18n configuration. In this option, I choose:
	1. Change Locale - I deselect the default (en_GB.UTF-8) and select the appropriate locale for me (en_US.UTF-8). It will then confirm that you want to use this locale for the system environment (you do). This will dump you back to the main configuration menu, you'll have to go back into "I18n Options" to change further settings.
	2. Change Timezone - The default timezone is UTC which is appropriate for a Unix-like system. A timezone can be selected for each individual user by following the steps described by running `tzselect`. If you go into this option, UTC is under "None of the above". I leave my systems set to UTC.
	3. Change Keyboard Layout - you'll almost certainly want to do this and change to an appropriate keyboard layout. For example, in the default UK keyboard layout Shift-3 results in a pound sign (£) rather than a hash (#) which makes commenting lines out (for example, in `/etc/network/interfaces`) a challenge (the backslash (\\) key in a UK keyboard layout types the hash).

5. Enable Camera - this is for the [Raspberry Pi Camera Board](http://www.adafruit.com/products/1367).

6. Add to Rastrack - if you want your installation to show up on the [Rastrack Raspberry Pi Map](http://rastrack.co.uk), choose this.

7. Overclock - To date, I do not overclock my Raspberry Pis.

8. Advanced Options - there are a few things to change here:
	1. Overscan - this setting may be needed when using the Raspberry Pi in desktop mode.
	2. Hostname - obviously it's good to set a hostname, by default it selects "raspberrypi".
	3. Memory Split - configures the memory split between video memory and available memory. Since my Raspberry Pis are mostly headless, I typically choose this option and enter the minimum allocation for the GPU (16).
	4. SSH - I'm mostly accessing over the network so I enable this.
	5. SPI - I do not enable, this, I do not use the [PiFace](http://www.piface.org.uk).
	6. Audio - whether you want audio to come out the HDMI jack or the 3.5mm headphone jack. Irrelevant for most headless use.
	7. Update - updates the `raspi-config` tool we are currently using. A little late for that, don't you think?

We're done! If you select "Finish" it will prompt for a reboot and you are good to go!

Initial Login
---
Upon first boot, you'll want to log into the Raspberry Pi with the "pi" login and the password you specified during configuration. If you didn't change the default password, it is: raspberry. Some steps you might take at this point:

* Remote access is enabled, however by default the Raspberry Pi is configured to use DHCP. For headless Raspberry Pis, I like to configure a static IP address. This is done following the same steps as in Debian:
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

From this point on, you can SSH in from another workstation to continue configuration if that is easier (it might be due to the ability to have cut and paste).

* You may wish to change the time zone for the "pi" user. You can run `tzselect` to walk you through the time zone options which will end up providing you a line to add to the end of your `.profile` file. Cut and paste is handy here.

* You may want to remove some packages that you aren't likely to use. The scope of that is well outside this document, however here's a line I have used in the past to remove packages that I don't typically use on a headless Raspberry Pi. These are hand-selected by me and will certainly cause issues if your Pi is trying to boot to the X Window environment as these packages are critical for that environment to run. Safety not guaranteed:

		sudo apt-get remove cups-bsd cups-client cups-common ghostscript libaudio2 \
		libcups2 libcupsimage2 libgail-3-0 libgl1-mesa-glx libglapi-mesa libgs9 \
		libgs9-common libgtk-3-0 libgtk-3-bin libgtk-3-common libicu48 liblvm2app2.2 \
		libopenjpeg2 librsvg2-2 librsvg2-common omxplayer python-pifacecommon \
		python-pifacedigitalio python3-pifacecommon \
		python3-pifacedigital-scratch-handler python3-pifacedigitalio samba-common \
		scratch smbclient squeak-vm xserver-common xserver-xorg-core fonts-droid \
		gir1.2-glib-2.0 gnome-accessibility-themes gnome-themes-standard-data jackd \
		jackd2 lesstif2 libcwiid1 libfftw3-3 libfile-copy-recursive-perl \
		libgirepository-1.0-1 libijs-0.35 libjack-jackd2-0 libjbig2dec0 \
		liblightdm-gobject-1-0 libpaper-utils libpaper1 libqt4-dbus libqt4-network \
		libqt4-xml libqtdbus4 libruby1.9.1 libscsynth1 libwebkitgtk-3.0-common \
		libxklavier16 libxp6 libyaml-0-2 poppler-data python-dbus python-dbus-dev \
		python-gi qdbus ruby1.9.1 squeak-plugins-scratch supercollider-common \
		supercollider-server update-inetd zenity-common alsa-base alsa-utils aptitude \
		aptitude-common aspell aspell-en dbus-x11 fontconfig fontconfig-config \
		fonts-freefont-ttf gsfonts gsfonts-x11 x11-common x11-xkb-utils \
		x11-xserver-utils xfonts-encodings xfonts-utils xinit tasksel tasksel-data
		sudo apt-get autoremove
		sudo apt-get autoclean
 This currently results in savings of about 1 GB of storage.
 
  Another option for cleaning up the X Window System can be found [here at tweaking4all.com](http://www.tweaking4all.com/forums/topic/minimalistic-raspbian-removing-x/). Note: those instructions recommend running `sudo apt-get clean` which will remove all packages from the package cache which you probably don't want - just skip that step.

* At this point it's probably a good idea to ensure Raspbian is up-to-date. I typically use `sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade`.

* If you are working in an environment with OS X, you may want to enable ZeroConf/Bonjour name resolution. You can do that using the [Avahi](https://wiki.debian.org/ZeroConf) package with `sudo apt-get install avahi-daemon avahi-discover libnss-mdns`. Once this is installed, you will be able to resolve the machine as *hostname*.local. This is very handy if your headless Raspberry Pi is configured with DHCP.

* The Raspberry Pi does not have a real-time clock so it appears to [use the time saved during the last shutdown](http://captainbodgit.blogspot.com/2013/10/raspberrypi-keeping-time-without.html) to set the clock upon boot if it cannot reach a time server. It might be a good idea to take a look at the NTP configuration and ensure that it's appropriate for your environment. By default, the Raspberry Pi will use NTP servers sent out by your DHCP server. You can see the current status with `ntpq -p`, you are looking for a line with an asterisk (*) in the first column indicating that there's a valid time source (you might need to wait a while after a reboot for things to sync up). If you need assistance in configuring NTP, I'd recommend reviewing: [How do I use pool.ntp.org?](http://www.pool.ntp.org/en/use.html).

  If you wish to manually configure NTP while still using DHCP, you can remove `ntp-servers` from the "request" line in `/etc/dhcp/dhclient.conf`, remove the `/var/lib/ntp/ntp.conf.dhcp` file, and then configure NTP normally (`/etc/ntp.conf`). If you do not take this step, every time the DHCP lease is renewed it will update the `/var/lib/ntp/ntp.conf.dhcp` file and use those NTP servers.
