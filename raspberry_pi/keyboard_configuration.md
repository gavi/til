# Keyboard Configuration using Command Line

if you install the Rasbian Lite OS on your Raspberry Pi the default keyboard configuration is “UK”. To change it to “US” keyboard using command line only follow the steps below.

```
sudo nano /etc/default/keyboard
```

Change the following key to `us`

```
XKBLAYOUT="us"
```

Save the file and `sudo reboot`