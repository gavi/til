# Install Rasbian Lite using Mac

Rasbian Lite removes all the GUI software from Rasbian. Since there is no GIU, you will have to configure the Keyboard and WIFI settings manually.

Step 1: 

Connect MicroSD card to your Mac


```
diskutil list
```

This will show all the disks in your Mac. Identify the one which is your microSD card by looking at the size. In the example below my disk is `/dev/disk2` which is 16GB

```
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage Macintosh HD            999.7 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           +999.3 GB   disk1
                                 Logical Volume on disk0s2
                                 5367767B-B981-41E0-83C0-FD25B6087137
                                 Unlocked Encrypted
/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.9 GB    disk2
   1:             Windows_FAT_32 NO NAME                 15.9 GB    disk2s1
```


Step 2:

Unmount the disk

```
diskutil unmountDisk /dev/disk2
```

Step 3:

Copy the image using `dd`. Be extremely careful as you can wipe out your hard disk if you choose the wrong disk. Remember `dd` stands for “Disk Destroyer”. Also note, I am writing to the raw disk below (rdisk2)


```
sudo dd if=2016-03-18-raspbian-jessie-lite.img of=/dev/rdisk2 bs=1m
```
