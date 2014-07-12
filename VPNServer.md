Raspberry Pi L2TP/IPsec VPN Server
===
Introduction
---

Following these steps, I created an RSA-certificate-based L2TP VPN Server on my Raspberry Pi behind a NAT at home. These instructions were based on **2014-06-20-wheezy-raspbian.img**.

Information you'll need
---
You'll need some information to properly configure the VPN software:

1. The IP address of your Raspberry Pi - you do not want this to change as you need to configure port forwarding and you also need to include this address in some configuration files. I will refer to this as the `<RPI-IP>`.

2. A block of IP addresses to assign to VPN clients. This block of IP addresse should not be addresses given out by your DHCP server but should fall in your subnet at home. You'll need to know this block in CIDR format (e.g. 10.0.0.24/29). I will refer to this as the `<netblock-CIDR>`. Also you'll need the first IP address in this block which I'll call `<netblock-FIRST>` and the last address which I'll call `<netblock-LAST>`.

3. A pre-shared key - this is a secret string of characters used as part of the authenticaton process. I will refer to this as `<pre-shared key>`. This can easily be generated with `pwgen` using a command similar to `pwgen -sy 16`. Note: you'll need to type this in to every device you wish to VPN from, so consider typing it in on your mobile device's keyboard before you go crazy with it.

4. A couple of DNS servers, either on your network or public (such as Google's at 8.8.8.8 and 8.8.4.4). I'll refer ot these as `<DNS1>` and `<DNS2>`. If you need to see what DNS servers your Raspberry Pi is using, you can find out with `grep nameserver /etc/resolv.conf`.

5. For each VPN user you'll need a `<username>` and `<password>`. The same `pwgen` advice from above applies.

Initial configuraiton
---
1. Forward TCP ports 500, 4500, 1701 to the Raspberry Pi.

2. On the Raspberry Pi, `sudo apt-get install racoon xl2tpd iptables` (Choose the "direct" configuration method for Racoon).

3. To configure for IPv4 forwarding, uncomment the line `net.ipv4.ip_forward=1` in `/etc/sysctl.conf`. Reload the file with: `sudo sysctl -p /etc/sysctl.conf`.
 
4. To configure iptables: `sudo iptables -t nat -A POSTROUTING -o eth0 -s <netblock-CIDR> -j MASQUERADE`

### Configure racoon

1. Edit the file `/etc/racoon/racoon.conf` and add the following section at the bottom:

		remote anonymous {
			exchange_mode main;

			generate_policy on;
			nat_traversal on;

			dpd_delay 20;

			proposal {
				encryption_algorithm aes;
				hash_algorithm md5;
				authentication_method pre_shared_key;
				dh_group modp1024;
			}
		}

		sainfo anonymous {
			encryption_algorithm aes, 3des;
			authentication_algorithm hmac_sha1, hmac_md5;
				compression_algorithm deflate;
		}

2. Edit the file `/etc/racoon/psk.txt` and ensure every line starts with a number sign (#) (we want everything in the file to be commented out), then add the following:

		*	<pre-shared-key>

3. Edit the file `/etc/ipsec-tools.d/l2tp.conf` and put the following:

		spdadd <RPI-IP>[l2tp] 0.0.0.0/0 udp -P out ipsec
			esp/transport//require;
		spdadd 0.0.0.0/0 <RPI-IP>[l2tp] udp -P in ipsec
			esp/transport//require;
        
### Configure xl2tpd

1. Edit `/etc/xl2tpd/xl2tpd.conf` (this file was not created at 80 columns so you'll want to resize your terminal window wider if possible). The entire file seems to be commented out by default so you can simply add this to the end:

		[global]
		access control = no
		;debug avp = yes
		;debug network = yes
		;debug state = yes
		;debug tunnel = yes

		[lns default]
		ip range = <netblock-FIRST>-<netblock-LAST>
		local ip = <RPI-IP>
		require authentication = yes
		require chap = yes
		refuse pap = yes
		length bit = yes
		name = l2tpd
		pppoptfile = /etc/ppp/xl2tpd-options
		;ppp debug = yes

Lines above which start with a semicolon (;) are commented out - if you need to troubleshoot configuraiton issues, you can uncomment them and you'll get some very verbose logging.

### Configure pppd

1. You'll want to create a file corresponding with the `pppoptfile` we specified above, in this case `/etc/ppp/xl2tpd-options`:

		auth
		nodefaultroute
		lock
		proxyarp
		require-chap
		ms-dns <DNS1>
		ms-dns <DNS2>

2. Edit `/etc/ppp/chap-secrets` to add your user login(s) and password(s):

		<username>	l2tpd	<password>		*

### Restart

1. Restart the Raspberry Pi to reload the configuration we've chagned with `shutdown -r now`.