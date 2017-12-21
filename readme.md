## Fingerprint FMP10 (ZFM-20 module) to PC with python

Tested with FMP10A sensor on Linux (ubuntu 16.04 LTS, kernel 4.10.0-42-generic) and MAC (MacBook Pro Mid 2015 max-options macOS High Sierra 10.13.2).

### Preparing
- Module fingerprint FMP10 (ZFM-20)
- TTL to USB module. I use USB UART CP2102. Install driver from here: https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers
Linux:
	+ Download from link above for linux
	+ Extract and Cd to source and run
```
make
sudo cp cp210x.ko /lib/modules/$(uname -r)/kernel/drivers/usb/serial/
sudo insmod /lib/modules/$(uname -r)/kernel/drivers/usb/serial/usbserial.ko
sudo insmod /lib/modules/$(uname -r)/kernel/drivers/usb/serial/cp210x.ko
```
Error: insmod ERROR could not insert module cp210x.ko: Required key not available
Fix: You can disable Secure Boot (UEFI) in the BIOS with the following steps:

	Reboot your machine and enter the BIOS Menu (In my case pressing F2)
	Search for Secure Boot and change to Legacy
	In an ASUS motherboard:
	Go to the Advanced Mode (F7)
	Go in the Secure Boot option under the Boot section
	Change "Windows UEFI mode" with "Other OS"
	Save and restart to apply settings (F10)
	Install again

- Connect pin:
```
Sensor				USB.UART
5v		<->		5v
GND		<->		GND
TX		<->		RX
RX		<->		TX
```

### Requirements
```
python3
pip3
pillow
pyserial
```

On both:
```
sudo pip install pillow
sudo pip install pyserial
```

On linux:
```
sudo apt-get install python-serial python3-serial
```

On MAC:
Install python3 & pip3:
```
sudo mkdir /usr/local/Frameworks
sudo chown $(whoami):admin /usr/local/Frameworks

brew install python3
brew postinstall python3
brew link python3

python3 --version
pip3 --version
```
ModuleNotFoundError: No module named 'PIL'
=> 
```
sudo pip3 install image
```
Install serial
```
sudo pip3 install pyserial
```

----------------------
### Check port usb ttl
Linux:
```
dmesg | grep tty
```
=> Use port:  /dev/ttyUSB0

MAC:
```
ls /dev/cu.*
```
=> Use port: /dev/cu.SLAB_USBtoUART

**HELP**
`python3 main.py --help`

To take image to buffer on sensor:
`python3 main.py -p USBTTL_PORT --model-generate`

Ex:
`python3 main.py -p /dev/cu.SLAB_USBtoUART --model-generate`

To download image from sensor to pc:
`python3 main.py -p USBTTL_PORT --image-upload t.bmp`

Ex:
`python3 main.py -p /dev/cu.SLAB_USBtoUART --image-upload t.bmp`

### Estimate
Transfer image to pc around ~7.5 - 8 seconds
![Demo video](./assets/demo.mp4)


Ref: 
```
https://github.com/OLIMEX/FingerPrint
https://github.com/brianrho/FPM
https://github.com/bastianraschke/pyfingerprint
https://github.com/adafruit/Adafruit-Fingerprint-Sensor-Library
```
