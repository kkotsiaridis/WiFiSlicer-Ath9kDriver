In order to execute the experiments follow the instructions below:

For the 1st experiment:
	
Coordinator Side:
--------------
	Alter file: hostap.conf --> ssid = COORDINATOR, channel= 6.
	Update file: backports-5.3.6-1/drivers/net/mac80211/tx.c.
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
	Alter file: hostap.conf --> ssid = HOME/HOTSPOT, channel= 6
	Create a monitor interface for APs.
####  Terminal 1 ####
	```
	modprobe -r ath9k
	modprobe ath9k
	iw wlan0 interface add mon0 type monitor
	ifconfig mon0 up
	ifconfig wlan0 down
	iw mon0 set freq 2437
	ifconfig wlan0 192.168.2.6 up
	./manip COORDINATOR HOME / ./manip COORDINATOR HOTSPOT
	```

#### Terminal 2 #### 
Before the monitor interface is activated, the driver files should be updated.
	Update the files  and Makefile for module that is responsible for proc files: proc_read_write.c
	```make``` (for proc module)
	```insmod proc_read_write.ko```
	-Update file: backports-5.3.6-1/drivers/net/wireless/ath/ath9k/xmit.c
	-Update file:  backports-5.3.6-1/drivers/net/wireless/ath/ath9k/main.c
	-Update file:  backports-5.3.6-1/drivers/net/wireless/ath/ath9k/mac.c
	-Update file:  backports-5.3.6-1/drivers/net/wireless/ath/ath9k/mac.h
	````
	cd  backports-5.3.6-1
	make
	make install
	cd
	ifconfig wlan0 192.168.2.1 up
	ifconfig wlan0 192.168.2.2 up 
	hostapd -dd hostap.conf
	```

#### Terminal 3 ####
	```iperf -c 192.168.2.X -u -b 100M -t 200 -i 1 ```
	X is the number of connections between ap and station


Stations Side:
--------------
	```
	modprobe -r ath9k
	modprobe ath9k
	ifconfig wlan0 192.168.2.X up
	iw dev wlan0 connect HOTSPOT / iw dev wlan0 connect HOME
	iperf -s -u -i 1
	```
