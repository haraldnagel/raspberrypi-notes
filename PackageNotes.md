Raspberry Pi Package Notes
===
Here are some notes about various packages that I've installed in Raspbian with apt.

nmap
---
By default, nmap installs a ton of additional stuff which is not appropriate for a console-only machine. This is due to the fact that, by default, apt installs "recommended" packages as well as "required" packages. This is resolved by either using `apt-get --no-install-recommends install nmap` or putting the following lines in `/etc/apt/apt.conf`:

	APT::Install-Recommends "0";
	APT::Install-Suggests "0";

Question found at: [apt-get installing unnecessary packages](http://www.raspberrypi.org/forums/viewtopic.php?f=66&t=23318) and the solution there links to [Tip: Tell apt-get not to install “recommends” packages in Debian Linux](http://linux.koolsolutions.com/2009/01/07/howto-tell-apt-get-not-to-install-recommends-packages-in-debian-linux/).

transmission-daemon
---
By default, transmission-daemon spews tons of text into `/var/log/syslog`. To resolve, create the file  `/etc/rsyslog.d/transmission.conf` containing the following lines:

	:programname,isequal,"transmission-daemon"   /var/log/transmission.log
	:programname,isequal,"transmission-daemon"   ~

To ensure that the log file is rotated regularly, in `/etc/logrotate.d/rsyslog`, under the "/var/log/messages" line, add:

	/var/log/transmission.log
