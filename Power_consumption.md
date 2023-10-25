# Power Consumption

## Modifying file /boot/config.txt

`vi /boot/config.txt`

We add these lines at the end of the file:

`gpu_mem=16`

`dtoverlay=disable-bt`

`dtoverlay=disable-wifi`


We modify the lines:

`dtparam=audio=on` to `dtparam=audio=off`

`camera_auto_detect=1` to `camera_auto_detect=0`

`display_auto_detect=1` to `display_auto_detect=0`

`#arm_freq=800` to `arm_freq=600`

`dtoverlay=vc4-kms-v3d` to `#dtoverlay=vc4-kms-v3d`

We modify in the program:

`sudo raspi-config`

6 Advanced Options => A2 GL Driver => G1 Legacy

Then We reboot:

`sudo reboot`


## Disable HDMI

If you're using the Pi without a monitor, you can simple turn off HDMI output like so:

`sudo /usr/bin/tvservice -o`

In the raspberry 4:

In the file /boot/config.txt

We modify: `dtoverlay=vc4-kms-v3d` to dtoverlay=vc4-fkms-v3d

then we reboot
