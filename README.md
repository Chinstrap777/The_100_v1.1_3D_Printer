# The_100_v1.1_3D_Printer

My goal is to greatly improve the documentation for The 100 3D Printer v1.1. This will become my central repository for designs, BOMs, configs, macros, images, troubleshooting guides and much more.

Please feel free to add issues as you see them, and I'll do my best to make improvements as soon as I'm able to.

BTT SKR 3 EZ setup guide with better details for Klipper https://www.ifixit.com/Guide/4.+Installing+Klipper+on+SKR3+EZ/152185

BTT Pi v1.2 Armbian setup

Download Armbian image at:

https://www.armbian.com/bigtreetech-cb1/

Use Balena Etcher to write image to class 1 SD card

Insert SD card into BTT Pi

Connect BTT Pi to LAN using Ethernet

Use SSH client like putty or WebSSH to login as 
Username: root
Password: 1234

Change default root password

Choose BASH

create a user and password

Choose your real name

Choose “Y” for detected time zone 

Choose “Y” for language based on location

Choose language locale (en-us for me)

sudo reboot now

Change login to new user and password or new root and password

sudo debian-config
-Network
- WiFi 
- Select WiFi network and enter password
-Quit
-Back
-System
- Firmware
- Update board firmware
-  reboot Y

Find IP address and login via SSH using new IP/user/password

sudo apt update (shouldn’t have any updates)

sudo apt upgrade (shouldn’t have any updates)

sudo reboot now

sudo apt-get  install git -y
(To update Git)

To install KIAUH viaSSH  Computer Type:
cd ~ && git clone https://github.com/th33xitus/kiauh.git

Computer Type:
./kiauh/kiauh.sh
To open the Klipper Installation and Update Helper (KIAUH)

type sudo password (will not show any characters or *****

Uninstall everything using KIAUH

Install Klipper using KIAUH and when prompted choose 1 instance

Select python 3.x

Install Moonraker and select same# of instances when prompted.

Install “Fluid” or “Mainsail” for web interface to the printers

Install “Klipper screen” For screen support 

Install “crowsnest” For webcam support.

Install “Obico” for webcam and AI print failure detection

Go back by typing “B”
Quit KIAUH by typing “Q”

Computer Type: 
sudo reboot
To reboot the BTT pi

Check your connection and that you can still SSH after installing Klipper

sudo shutdown now

Remove SD card, insert into computer, insert USB flash drive into computer

Open Balena Etcher and clone the SD card to the USB flash drive

Eject USB drive and SD card


########################
DON’T USE THIS YET UNTIL WE FIGURE OUT HOW TO CHANGE BOOT ORDER
INSERT HOW TO SETUP USB DRIVE BOOT ORDER
sudo armbian-install

sudo armbian-config to change boot order to usb or copy to usb

Insert USB flash drive to the BTT Pi
#################


USB ports are 
USB0 (top right) 
USB1 (top left) 
USB2 (bottom right, but used for U2C module for IO2CAN/CANBUS)
USB3 (bottom left)

Printer.cfg reference
serial: /dev/ttyACM0 
serial: /dev/ttyUSB0
restart_method: command

To find more details:
ls /dev/serial/by-id/*

ls /dev/serial/by-path/*

udevadm info —query=all —name=/dev/ttyACM0

lsusb


