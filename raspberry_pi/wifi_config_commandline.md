# WIFI Config using Command Line

If you setup Raspberry Pi using Rasbian Lite, you do not have a graphical interface. In cases like these, you might need to setup WIFI manually

## For WIFI with security

Edit the file `/etc/wpa_supplicant/wpa_supplicant.conf` using sudo and a text editor such as `nano`

```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Add the network section at the bottom

```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
        ssid=“your_ssid_name”
        psk=“your_password”
}
```

Save the file and reboot

## For WIFI with no security (Open)

Edit the same file as above, but put this at the end of the file

```
network={
        key_mgmt=NONE
        priority=-999
}
```