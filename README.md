BeagleBoneNeoPixelDocs
======================

Just some doco on how to play with neopixels on the beaglebone


Hardware
========

While the doco on the internet suggested that neopixels will only work on 5v logic, they seem to work on 3.3v logic if you are providing the neopixel with only 3.3v supply. As such I just connected up the positive and negative pins of the neopixels to the ground and 3.3v output on the beaglbone (as I only have 2 neopixels the beaglebone voltage regulator can handle this. More LEDs require an alternative power source), and the data pin to pin 22 on P9.

Install LEDscape
================

1. ssh into the beaglebone (use PuTTY to connect to 192.168.7.2 - username root)
1. Plug in ethernet port on beaglebone to the internet
1. In the terminal type "dhclient eth0"
1. Clone the LEDscape directory from github (type in the terminal "git clone https://github.com/TheSkorm/LEDscape"
1. Change directory into the LEDscape folder (type in the terminal "cd LEDscape")
1. Make LEDscape source (type in the terminal "make"). This take a little while to complete and will flash up a bunch of commands. Just wait until your brought back to a terminal window.
1. Copy the dtb file from the LEDscape folder to the uboot folder (type in the terminal "cp am335x-boneblack.dtb /boot/uboot/dtbs/am335x-boneblack.dtb")
1. Edit the crontab to start OPC server on boot (type "crontab -e" into the terminal and add a line to the end of this file with the following "@reboot cd /root/LEDscape && /root/LEDscape/opc-server -c 2 -s 1 -D -i -t -1 NOP" )
1. Download the python lib (type in the terminal wget -P /usr/lib/python2.7/ "https://raw.githubusercontent.com/TheSkorm/fadecandy/master/examples/python/opc.py" )
1. Reboot

If everything has gone well you can now start using the neopixels in the cloud9 editor. Here is an example on how to turn two of the neopixels red.

```python
import opc
import time
client = opc.Client('localhost:7890')
my_pixels = [(255, 0, 0), (255, 0, 0)]
while 1:
    client.put_pixels(my_pixels)
    time.sleep(0.1)
```
