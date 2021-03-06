# rpi-mjpg-streamer

Instructions and helper scripts for running mjpg-streamer on Raspberry Pi.


## A. Setup mjpg-streamer

### Enable Raspberry Pi Camera module from raspi-config

```
$ sudo raspi-config
```

### Install necessary packages for mjpg-streamer

```
$ sudo apt-get update
$ sudo apt-get install build-essential libjpeg8-dev imagemagick libv4l-dev git cmake uvcdynctrl
```

### Build mjpg-streamer

```
$ sudo ln -s /usr/include/linux/videodev2.h /usr/include/linux/videodev.h
$ git clone https://github.com/jacksonliam/mjpg-streamer
$ cd mjpg-streamer/mjpg-streamer-experimental
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=.. .
$ make install
```

### Setup video4linux for Raspberry Pi Camera module

```
$ sudo modprobe bcm2835-v4l2
$ sudo vi /etc/modules

# add following line:
bcm2835-v4l2

$ sudo vi /boot/config.txt

# add following line if you want to disable RPi camera's LED:
disable_camera_led=1
```

### Add yourself to the video group

```
$ sudo usermod -a -G video $USER
```

## B. Run mjpg-streamer

### 1. Clone this repository

```
$ git clone https://github.com/meinside/rpi-mjpg-streamer.git
```

### 2-a. Run mjpg-streamer from the shell directly

```
# copy & edit run-mjpg-streamer.sh to your environment or needs
$ cp rpi-mjpg-streamer/run-mjpg-streamer.sh.sample somewhere/run-mjpg-streamer.sh
$ vi somewhere/run-mjpg-streamer.sh

# then run
$ somewhere/run-mjpg-streamer.sh
```

### 2-b. Or run mjpg-streamer as a service

#### systemd

```
# copy & edit systemd/mjpg-streamer.service file,
$ sudo cp rpi-mjpg-streamer/systemd/mjpg-streamer.service.sample /lib/systemd/system/mjpg-streamer.service
$ sudo vi /lib/systemd/system/mjpg-streamer.service

# then register as a service
$ sudo systemctl enable mjpg-streamer.service

# or remove it
$ sudo systemctl disable mjpg-streamer.service

# and start/stop it
$ sudo systemctl start mjpg-streamer.service
$ sudo systemctl stop mjpg-streamer.service
```

## C. Connect

Connect through the web browser:

![Connected](https://cloud.githubusercontent.com/assets/185988/2740477/3501d5b0-c6d3-11e3-85de-de3ceb302325.png)

Most modern browsers(including mobile browsers like Safari and Chrome) will show the live stream immediately.

