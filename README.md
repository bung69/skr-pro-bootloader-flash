# skr-pro-bootloader-flash
Instructions to flash SKR PRO bootloader for use with kipper, using a Rpi and usb serial programer.



find latest hid_bootloader_skr_pro.bin here and copy the link:

https://github.com/Arksine/STM32_HID_Bootloader/releases

SSH in to RPI
download to Rpi with Wget:

    wget https://github.com/Arksine/STM32_HID_Bootloader/releases/download/v0.7/hid_bootloader_skr_pro.bin


Power off SKRpro, Disconnect USB cable.

Use a Jumpper to connect Boot 0 pin to 3.3V. the top 2 pins of a 2x2 header above extention 1 header. 

Set Serial programer to 3.3V

Connect serial programer 3.3V and GND  to 3.3v and GND pins on the SKR - Recomend the 3.3v/GND headders in teh bottom right corner.

Connect Serial programer TX to SKRPRO TFT RXD  and serial programer RX to SKRPRO TFT TXD
    TX > TFT RXD
    RX > TFT TXD

Before plugging in USB serial programer to RPI, 
    ls /dev
Then plug in usb serial programer to RPI and ls /dev again to get the id of serial programer. 
for example /dev/ttyUSB0

Flash the bootloader with:

    stm32flash -w hid_bootloader_skr_pro.bin -v -g 0x08000000 /dev/ttyUSB0


configure klipper firmware with:
    cd ~/klipper
    make menuconfig

then set:

    Micro-controller Architecture (STMicroelectronics STM32)
    Processor model (STM32F407)  --->
    Bootloader offset (16KiB bootloader (HID Bootloader))

Escape key and save

    make

remove serial programer from SKR PRO
remove jumper from BOOT0 and place on BOOT1
set board power to USB with USB power jumper
plug usb cable from skr pro in to RPi

in RPI SSH terminal:
    lsusb

look for simmiler:  Bus 001 Device 010: ID 1209:beba Generic serasidis.gr STM32 HID Bootloader

use device id to flash klipper firmware to SKR pro:

    make flash FLASH_DEVICE=1209:beba

when flashing compleate, disconect SKR pro usb, remove boot1 jumper and replace USB power jumper to previous location.

plug in SKR pro USB.

NO MORE MESSING AROUND WITH SD CARD FIRMWARE UPDATES

