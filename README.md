Tor Pi
====================


Requirements :
-------------

1. A Raspberry Pi 3B+ or 4B

2. An active ethernet connection


Setting up Access Point :
-------------

* In your Pi's Terminal clone repository with : `git clone https://github.com/ArsalanSahab/Tor-Pi.git`.

* Navigate to Tor-Pi folder(in the terminal), and execute following command in the terminal : `sudo ./install.sh` or `sudo ./install`.

* Select 'Y' for :

	1. Terms and Conditions
	2. preconfigured DNS.
	3. to use Unblock-Us DNS servers

* Select 'N'  :

	1. When asked if you want to use Defaults(optional).
	2. When asked - "Are you using a rtl871x chipset?".
	3. chromecast support.


Setting up Tor :
-----------------------

After Rebooting ...

* Install Tor by executing the following command in the terminal : `sudo apt install tor`.

* Then execute the following command in the terminal : `sudo nano /etc/tor/torrc`.

* Copy and Paste the below between any two set of comments :

	        Log notice file /var/log/tor/notices.log
		VirtualAddrNetwork 10.192.0.0/10
		AutomapHostsSuffixes .onion,.exit
		AutomapHostsOnResolve 1
		TransPort 192.168.42.1:9040
		TransListenAddress 192.168.42.1
		DNSPort 192.168.42.1:53
		DNSListenAddress 192.168.42.1 

* Save the file and exit.

* Execute the following commands in the terminal to configure the IPTABLES :
	
	
		 `sudo iptables -F && sudo iptables -t nat -F`
		
		 `sudo iptables -t nat -A PREROUTING -i wlan0 -p udp --dport 53 -j REDIRECT --to-ports 53`

		 `sudo iptables -t nat -A PREROUTING -i wlan0 -p tcp --syn -j REDIRECT --to-ports 9040`

		 `sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"`


* Execute the following commands in the terminal to configure the LOG FILES :

		
		 `sudo touch /var/log/tor/notices.log`

		 `sudo chown debian-tor /var/log/tor/notices.log && sudo chmod 644 /var/log/tor/notices.log`


* Execute the following commands in the terminal to configure the TOR :

		
		 `sudo service tor start`
		
		 `sudo service tor status`

		 `sudo update-rc.d tor enable`

		 `sudo reboot`



	




