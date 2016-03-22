# Compiling Swift on Raspberry Pi

Compiling swift requires a lot of memory and patience.Raspberry Pi 3 comes with 1 GB of RAM and 4 Cores.

If you follow the instructions on Apple’s swift github repo, the default build script will try to use all the cores by spawning multiple jobs thus requiring a lot of RAM which Raspberry Pi does not have.

I found running with jobs set to 1 and swap space set to 2048 MB worked well.

## Setting Swap Space in Raspberry Pi

The default swap space on Rasbian running on Rapsberry Pi 3 is 100MB. The configuration is stored in the file `/etc/dphys-swapfile`. Lets edit this file

```
sudo vi /etc/dphys-swapfile
```

Change the following key to the 2048 MB

```
CONF_SWAPSIZE=2048
```

Now restart the swap service

```
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```

Check to see if it took effect

```
free -m
``` 

The output should be as follows

```
pi@raspberrypi:~$ free -m
             total       used       free     shared    buffers     cached
Mem:           925         87        838          6         10         41
-/+ buffers/cache:         35        890
Swap:         2047          0       2047
```

## Compiling Swift 

Install Prerequisites

```
sudo apt-get install git cmake ninja-build clang python uuid-dev libicu-dev icu-devtools libbsd-dev libedit-dev libxml2-dev libsqlite3-dev swig libpython-dev libncurses5-dev pkg-config
```

Get Latest version of swift from apple’s githb repo

```
git clone https://github.com/apple/swift.git
```

Compile Release version using jobs=1

```
cd swift
utils/update-checkout —clone
utils/build-script -R -j 1
```

Please be patient…

After compilation is done, the binaries are stored under

```
~/build/Ninja-ReleaseAssert/swift-linux-armv7/bin 
```

At the time of this writing REPL does not work on Raspberry Pi, but you can compile using `swiftc`

Add the binary path to `.bash_profile`

```
vi ~/.bash_profile
```

Add the following line

```
export PATH=~/build/Ninja-ReleaseAssert/swift-linux-armv7/bin:$PATH
```

Save the file. Then source it

```
source ~/.bash_profile
```

## Testing basic Swift program

Create a file called `test.swift` and add the following

```
print(“Hello World”)
```

Save the file. Compile using the following command

```
swiftc test.swift
```
Run this program using 

```
./test
```

You should see “Hello World” in the output


