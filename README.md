# k4a-setup
set up kinect 4 azure on jetson xavier nx


### Create a python environement

Install numpy, opencv, matplotlib, etc


### Install the k4a sdk for arm64

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/multiarch/prod
sudo apt-get update


sudo apt install k4a-tools
sudo apt install libk4a1.4-dev


```


### Increase the default USB for 
To acquire images greater than 2 MB in resolution, add the following to the APPEND line:

usbcore.usbfs_memory_mb=1000

to this file:

/boot/extlinux/extlinux.conf

If this method fails to set the memory limit, run the following command:

sudo sh -c 'echo 1000 > /sys/module/usbcore/parameters/usbfs_memory_mb'


### Add udev rules 
Create file '99-k4a.rules' with following and copy into '/etc/udev/rules.d/'.
```bash
# Bus 002 Device 116: ID 045e:097a Microsoft Corp.  - Generic Superspeed USB Hub
# Bus 001 Device 015: ID 045e:097b Microsoft Corp.  - Generic USB Hub
# Bus 002 Device 118: ID 045e:097c Microsoft Corp.  - Azure Kinect Depth Camera
# Bus 002 Device 117: ID 045e:097d Microsoft Corp.  - Azure Kinect 4K Camera
# Bus 001 Device 016: ID 045e:097e Microsoft Corp.  - Azure Kinect Microphone Array

BUS!="usb", ACTION!="add", SUBSYSTEM!=="usb_device", GOTO="k4a_logic_rules_end"

ATTRS{idVendor}=="045e", ATTRS{idProduct}=="097a", MODE="0666", GROUP="plugdev"
ATTRS{idVendor}=="045e", ATTRS{idProduct}=="097b", MODE="0666", GROUP="plugdev"
ATTRS{idVendor}=="045e", ATTRS{idProduct}=="097c", MODE="0666", GROUP="plugdev"
ATTRS{idVendor}=="045e", ATTRS{idProduct}=="097d", MODE="0666", GROUP="plugdev"
ATTRS{idVendor}=="045e", ATTRS{idProduct}=="097e", MODE="0666", GROUP="plugdev"

LABEL="k4a_logic_rules_end"

```

### Setup X for loading OpenGL libraries in headless mode, if you have monitor attached you won't need to do this as X server is started by default

```bash
/usr/bin/X :0 &
export DISPLAY=:0

```


### Instally python wrappers for k4a
https://github.com/etiennedub/pyk4a

Notes to make sure k4a.lib ins in your LD_LIBRARY_PATH, which is should be if you installed via debian package manager
pip install pyk4a
```bash

```

