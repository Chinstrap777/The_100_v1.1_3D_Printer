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

Use SSH client like putty or WebSSH to login as Username: root Password: 1234

Change default root password

Choose BASH

create a user and password

Choose your real name

Choose “Y” for detected time zone

Choose “Y” for language based on location

Choose language locale (en-us for me)

sudo reboot now

Change login to new user and password or new root and password

sudo debian-config -Network

WiFi
Select WiFi network and enter password -Quit -Back -System
Firmware
Update board firmware
reboot Y
Find IP address and login via SSH using new IP/user/password

sudo apt update (shouldn’t have any updates)

sudo apt upgrade (shouldn’t have any updates)

sudo reboot now

sudo apt-get install git -y (To update Git)

To install KIAUH viaSSH Computer Type: cd ~ && git clone https://github.com/th33xitus/kiauh.git

Computer Type: ./kiauh/kiauh.sh To open the Klipper Installation and Update Helper (KIAUH)

type sudo password (will not show any characters or *****

Uninstall everything using KIAUH

Install Klipper using KIAUH and when prompted choose 1 instance

Select python 3.x

Install Moonraker and select same# of instances when prompted.

Install “Fluid” or “Mainsail” for web interface to the printers

Install “Klipper screen” For screen support

Install “crowsnest” For webcam support.

Install “Obico” for webcam and AI print failure detection

Go back by typing “B” Quit KIAUH by typing “Q”

Computer Type: sudo reboot To reboot the BTT pi

Check your connection and that you can still SSH after installing Klipper

sudo shutdown now

Remove SD card, insert into computer, insert USB flash drive into computer

Open Balena Etcher and clone the SD card to the USB flash drive

Eject USB drive and SD card

######################## DON’T USE THIS YET UNTIL WE FIGURE OUT HOW TO CHANGE BOOT ORDER INSERT HOW TO SETUP USB DRIVE BOOT ORDER sudo armbian-install

sudo armbian-config to change boot order to usb or copy to usb

Insert USB flash drive to the BTT Pi #################

USB ports are USB0 (top right) USB1 (top left) USB2 (bottom right, but used for U2C module for IO2CAN/CANBUS) USB3 (bottom left)

Printer.cfg reference serial: /dev/ttyACM0 serial: /dev/ttyUSB0 restart_method: command

To find more details: ls /dev/serial/by-id/*

ls /dev/serial/by-path/*

udevadm info —query=all —name=/dev/ttyACM0

lsusb







# How to use Klipper on SKR-3

## NOTE: 

* This motherboard comes with bootloader which allows firmware update through SD card.

## Build Firmware Image

1. Precompiled firmware(The source code version used is [Commits on Mar 18, 2022](https://github.com/Klipper3d/klipper/commit/b4b19b8fc127051e12a9891990070b98bc6eac76))
   * [firmware-USB.bin](./firmware-USB.bin) Use USB to communicate with raspberry pi. Directly connect the raspberry pi with the motherboard through the USB cable to communicate normally.
   * [firmware-USART1.bin](./firmware-USART1.bin) Use TFT port USART1 to communicate with raspberry pi. Connect the UART-TX of raspberry pi with the USART-RX1 of motherboard and connect the UART-RX of raspberry pi with the USART-TX1 of motherboard directly to communicate normally.

2. Build your own firmware<br/>
   1. Refer to [klipper's official installation](https://www.klipper3d.org/Installation.html) to download klipper source code to raspberry pi.
   2. `Building the micro-controller` with the configuration shown below. (If your klipper cannot select the following configuration, please update your klipper source code)
      * [*] Enable extra low-level configuration options
      * Micro-controller Architecture = `STMicroelectronics STM32`
      * Processor model = `STM32H743`
      * Bootloader offset = `(128KiB bootloader (SKR SE BX v2.0))`
      * Clock Reference = `25 MHz crystal`
      * IF USE USB
         * Communication interface = `USB (on PA11/PA12)`
      * ElSE IF USE USART1
         * Communication interface = `(Serial (on USART1 PA10/PA9))`
      * ELSE
         * Communication interface = `The port you want`

      <img src=Images/menuconfig.png width="800" /><br/>
   3. Once the configuration is selected, press `q` to exit,  and "Yes" when  asked to save the configuration.
   4. Run the command `make`
   5. The `klipper.bin` file will be generated in the folder `home/pi/kliiper/out` when the `make` command completed. And you can use the windows computer under the same LAN as raspberry pi to copy `klipper.bin` from raspberry pi to the computer with `pscp` command in the CMD terminal. such as `pscp -C pi@192.168.0.101:/home/pi/klipper/out/klipper.bin c:\klipper.bin`(The terminal may prompt that `The server's host key is not cached` and ask `Store key in cache?((y/n)`, Please type `y` to store. And then it will ask for a password, please type the default password `raspberry` for raspberry pi)

## Firmware Installation
1. You can use the method in [Build Firmware Image 2.5](#build-firmware-image) or use a tool such as `cyberduck` or `winscp` to copy the `klipper.bin` file from your pi to your computer.
2. Renamed the `firmware-USB.bin`, `firmware-USART1.bin` or the `klipper.bin`(in folder `home/pi/kliiper/out` build by yourself) to `firmware.bin`<br/>
**Important:** If the file is not renamed, the bootloader will not be updated properly.
3. Copy the `firmware.bin` to the root directory of SD card (make sure SD card is in FAT32 format)
4. power off the motherboard
5. insert the microSD card
6. power on the motherboard
7. after a few seconds, the motherboard should be flashed
8. you can confirm that the flash was successful, by running `ls /dev/serial/by-id`.  if the flash was successful, this should now show a klipper device, similar to:

   <img src=Images/stm32h743_id.png width="600" /><br/>

   (note: this test is not appicable if the firmware was compiled for UART, rather than USB)

## Configure the printer parameters
### Basic configuration
1. Refer to [klipper's official installation](https://www.klipper3d.org/Installation.html) to `Configuring OctoPrint to use Klipper`
2. Refer to [klipper's official installation](https://www.klipper3d.org/Installation.html) to `Configuring Klipper`. And use the configuration file [SKR-3-klipper.cfg](./generic-bigtreetech-skr-3.cfg) as the underlying `printer.cfg`, which includes all the correct pinout for Octopus
3. Refer to [klipper's official Config_Reference](https://www.klipper3d.org/Config_Reference.html) to configure the features you want.
4. If you use USB to communicate with raspberry pi, run the `ls /dev/serial/by-id/*` command in raspberry pi to get the correct ID number of the motherboard, and set the correct ID number in `printer.cfg`.
    ```
    [mcu]
    serial: /dev/serial/by-id/usb-Klipper_stm32h743xx_41003D001751303232383230-if00
    ```
5. If you use USART1 to communicate with raspberry pi, you need to modify the following files by inserting the SD card into the computer or by SSH command.
   * Remove `console=serial0,115200` in `/boot/cmdline.txt`
   * Add `dtoverlay=pi3-miniuart-bt` at the end of file `/boot/config.txt`
   * Modify the configuration of `[mcu]` in `printer.cfg` to `serial: /dev/ttyAMA0` and enable `restart_method: command` by SSH
     ```
     [mcu]
     serial: /dev/ttyAMA0
     restart_method: command
     ```
     <img src=Images/cfg_uart.png/><br/>

### BigTreeTech TFT TouchScreen emulated 12864 mode: Set the `display` in `printer.cfg` to the following parameters
   ```
   [display]
   lcd_type: emulated_st7920
   spi_software_miso_pin: PA14 # status led, Virtual MISO
   spi_software_mosi_pin: EXP1_3
   spi_software_sclk_pin: EXP1_5
   en_pin: EXP1_4
   encoder_pins: ^EXP2_5, ^EXP2_3
   click_pin: ^!EXP1_2

   [output_pin beeper]
   pin: EXP1_1
   ```
   <img src=Images/cfg_tft_emulated_12864.png/><br/>
