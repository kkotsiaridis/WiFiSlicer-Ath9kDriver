
For the 2nd experiment:

Coordinator Side:
--------------
	Alter file: hostap.conf --> ssid = COORDINATOR, channel= 6.
	Create a monitor interface for coordinator.

####  Terminal 1 ####
	```
	modprobe -r ath9k
	modprobe ath9k
	iw wlan0 interface add mon0 type monitor
	ifconfig mon0 up
	ifconfig wlan0 down
	iw mon0 set freq 2437
	ifconfig wlan0 192.168.2.2 up
	./coord_listen
	```
####  Terminal 2 ####	
	Update the files and Makefile for module that is responsible for proc files: proc_read_write.c
	```make``` (for proc module)
	```insmod proc_read_write.ko```
	-Update file: coord_listen.c
	-Update file: backports-5.3.6-1/drivers/net/mac80211/tx.c
	```
	cd  backports-5.3.6-1
	make
	make install
	reboot
	modprobe -r ath9k
	modprobe ath9k
	ifconfig wlan0 192.168.2.2 up
	hostapd -dd hostap.conf
	```

APs Side:
--------------
	Alter file: hostap.conf --> ssid = HOME/HOTSPOT/INTERFIERER, channel= 6.
####  Terminal 1 ####
	Create a monitor interface for APs.
	```
	modprobe -r ath9k
	modprobe ath9k
	iw wlan0 interface add mon0 type monitor
	ifconfig mon0 up
	ifconfig wlan0 down
	iw mon0 set freq 2437
	ifconfig wlan0 192.168.2.6 up
	./manip COORDINATOR HOME / ./manip COORDINATOR HOTSPOT / ./manip COORDINATOR INTERFIERER
	````

Note: For the 2_b experiment monitor interface for interfierer is not needed.
Note: Interfierer's ip shhould be: 192.168.2.9 in order for coord_listen.c to function properly.
####  Terminal 2 #### 
	Update the files and Makefile for module that is responsible for proc files: proc_read_write.c
	```make``` (for proc module)
	```insmod proc_read_write.ko```
	-Update file: backports-5.3.6-1/drivers/net/wireless/ath/ath9k/xmit.c
	-Update file: backports-5.3.6-1/drivers/net/wireless/ath/ath9k/main.c
	-Update file: backports-5.3.6-1/drivers/net/wireless/ath/ath9k/mac.c
	-Update file: backports-5.3.6-1/drivers/net/wireless/ath/ath9k/mac.h
	```
	cd  backports-5.3.6-1
	make
	make install
	cd
	ifconfig wlan0 192.168.2.1 up / ifconfig wlan0 192.168.2.2 up 
	hostapd -dd hostap.conf
	```
####  Terminal 3 ####

	```iperf -c 192.168.2.X -u -b 100M -t 200 -i 1 ```
	X is the number of connections between ap and station

Stations Side:
--------------
	```
	modprobe -r ath9k
	modprobe ath9k
	ifconfig wlan0 192.168.2.X up
	iw dev wlan0 connect HOTSPOT / HOME / INTERFIERER
	iperf -s -u -i 1
	```

		