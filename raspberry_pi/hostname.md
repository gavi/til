# Setting HostName on Raspberry Pi 3
 
 Raspberry Pi has the default hostname of `raspberrypi`, Default username as `pi` and password as `raspberry`. Typically when you are setting up your cluster, you might want to rename the hostnames after setting IP Addresses manually. Here are the steps for setting the hostname

Edit the hosts file

```
sudo vi /etc/hosts
```

You should see a file like this

```
127.0.0.1       localhost
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters

127.0.1.1       raspberrypi
```

Change only the last line where it says `raspberrypi` to the desired hostname. In my case, I wanted to rename it as `node1`. So here is my file after the change

```
127.0.0.1       localhost
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters

127.0.1.1       node1
```

Next Edit the file `hostname` under `etc`

```
sudo vi /etc/hostname
```

Change `raspberrypi` to `node1`

Reboot

```
sudo reboot
```

