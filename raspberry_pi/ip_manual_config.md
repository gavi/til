# Setting Static IP Adress on Raspberry Pi 3 - Wireless LAN

Edit the file 

```
sudo nano /etc/dhcpcd.conf

```

Since we are configuring on Raspberry pi 3 which includes wireless on interface wlan0, Append the following to the end

```
interface wlan0
static ip_address=192.168.1.202/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

Reboot