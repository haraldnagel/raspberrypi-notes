Raspberry Pi Headless Configuration
===
When configuring a headless Raspberry Pi, I like to start with a basic install featuring as few packages as possible. The following is what I run to purge packages which I feel are not necessary for remote-only use based on my exploration of the default packages installed with **2014-06-20-wheezy-raspbian.img**. Some packages to remove were ascertained by following instructions presented at [Minimalistic Raspbian … removing X](http://www.tweaking4all.com/forums/topic/minimalistic-raspbian-removing-x/).

	apt-get purge alsa-base alsa-utils aptitude aptitude-common aspell aspell-en \
	blt cifs-utils consolekit cups-bsd cups-client cups-common dbus-x11 \
	desktop-base dillo esound-common fontconfig fontconfig-config fonts-droid \
	fonts-freefont-ttf fuse galculator gconf-service gconf2 ghostscript \
	gir1.2-glib-2.0 gksu -accessibility-themes gnome-icon-theme \
	gnome-themes-standard-data gpicview gsfonts gsfonts-x11 gtk2-engines gvfs \
	gvfs-backends gvfs-daemons gvfs-fuse idle idle-python2.7 idle-python3.2 idle3 \
	jackd jackd2 leafpad lesstif2 libarchive12 libasound2 libasyncns0 libaudio2 \
	libaudiofile1 libaudit0 libcaca0 libcairo-gobject2 libcairo2 libcroco3 \
	libcups2 libcupsimage2 libcwiid1 libdirectfb-1.2-9 libept-dev libept1.4.12 \
	libesd0 libfftw3-3 libfile-copy-recursive-perl libflac8 libfltk1.3 libfm-data \
	libfm-gtk-bin libfm-gtk1 libfm1 libfuse2 libgail-3-0 libgail18 libgd2-xpm \
	libgdk-pixbuf2.0-0 libgeoclue0 libgif4 libgirepository-1.0-1 libgksu2-0 \
	libgl1-mesa-glx libglade2-0 libglapi-mesa libgphoto2-2 libgs9 libgs9-common \
	libgstreamer-plugins-base0.10-0 libgstreamer0.10-0 libgtk-3-0 libgtk-3-bin \
	libgtk-3-common libgtk2.0-0 libgtk2.0-bin libgtop2-7 libice6 libicu48 \
	libid3tag0 libijs-0.35 libimlib2 libimobiledevice2 libjack-jackd2-0 \
	libjbig2dec0 libjson0 liblightdm-gobject-1-0 liblvm2app2.2 libmad0 \
	libmenu-cache1 libmikmod2 libmtdev1 libnotify4 libobrender27 libobt0 libogg0 \
	libopenjpeg2 libpango1.0-0 libpaper-utils libpaper1 libpciaccess0 libplist1 \
	libportmidi0 libpulse0 libqscintilla2-8 libqt4-dbus libqt4-network libqt4-svg \
	libqt4-xml libqtdbus4 libqtgui4 libqtwebkit4 librsvg2-2 librsvg2-common \
	libruby1.9.1 libsamplerate0 libscsynth1 libsdl-image1.2 libsdl-mixer1.2 \
	libsdl-ttf2.0-0 libsdl1.2debian libsm6 libsmpeg0 libsndfile1 \
	libsoup-gnome2.4-1 libsoup2.4-1 libstartup-notification0 libts-0.0-0 \ 
	libunique-1.0-0 libusbmuxd1 libvorbis0a libvorbisenc2 libvorbisfile3 \
	libvte-common libvte9 libwayland0 libwebkitgtk-1.0-0 libwebkitgtk-3.0-0 \
	libwebkitgtk-3.0-common libwebp2 libwnck-common libwnck22 libx11-6 libx11-data \libx11-	xcb1 libxapian-dev libxapian22 libxau6 libxaw7 libxcb-glx0 \
	libxcb-render0 libxcb-shape0 libxcb-shm0 libxcb-util0 libxcb-xfixes0 libxcb1 \
	libxcomposite1 libxcursor1 libxdamage1 libxdmcp6 libxext6 libxfixes3 libxfont1 \libxft2 	libxi6 libxinerama1 libxkbcommon0 libxkbfile1 libxklavier16 libxml2 \
	libxmu6 libxmuu1 libxp6 libxpm4 libxrandr2 libxrender1 libxres1 libxslt1.1 \
	libxss1 libxt6 libxtst6 libxv1 libxxf86dga1 libxxf86vm1 libyaml-0-2 lightdm \
	lightdm-gtk-greeter lxappearance lxde lxde-common lxde-core lxde-icon-theme \
	lxinput lxmenu-data lxpanel lxpolkit lxrandr lxsession lxsession-edit \
	lxshortcut lxtask lxterminal midori netsurf-gtk obconf omxplayer openbox \
	oracle-java7-jdk pcmanfm pistore policykit-1 poppler-data poppler-utils \
	pypy-setuptools pypy-upstream pypy-upstream-dev pypy-upstream-doc python-dbus \
	python-dbus-dev python-gi python-pifacecommon python-pifacedigitalio \
	python-pygame python-tk python3-pifacecommon \
	python3-pifacedigital-scratch-handler python3-pifacedigitalio python3-tk qdbus \
	qjackctl ruby1.9.1 samba-common scratch shared-mime-info smartsim smbclient \
	sonic-pi squeak-plugins-scratch squeak-vm supercollider supercollider-common \
	supercollider-server tasksel tasksel-data tcl8.5 timidity tk8.5 tsconf \
	update-inetd usbmuxd weston wolfram-engine wpagui x11-common x11-utils \
	x11-xkb-utils -xserver-utils x2x xarchiver xauth xfonts-encodings xfonts-utils \xinit 	xpdf xserver-common xserver-xorg xserver-xorg-core \
	xserver-xorg-input-all xserver-xorg-input-evdev xserver-xorg-input-synaptics \
	xserver-xorg-video-fbdev xserver-xorg-video-fbturbo zenity zenity-common
	apt-get autoremove
	apt-get autoclean

The apt-get purge should free up about 1 GB of space and the autoremove about another 100 MB.

Note: if you need any of these packages, you can always opt to reinstall them with `apt-get install <packagename>`.