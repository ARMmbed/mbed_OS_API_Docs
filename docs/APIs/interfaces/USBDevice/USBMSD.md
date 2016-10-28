# USBMSD

The USBMSD interface is used to emulate a mass storage device over USB. You can use this class to store or load data to and from a storage chip (SD card, flash and so on). 

This class implements the MSD protocol and calls pure virtual functions such as ``disk_initialize``, ``disk_write`` or ``disk_read`` to interact with a storage chip. 

The USB connector should be attached to:

* **p31 (D+), p32 (D-) and GND** for the **LPC1768 and the LPC11U24**.
* The on-board USB connector of the **FRDM-KL25Z**.

## Hello World with an SD card

The following program has been tested with a micro SD card (Transcend micro SD 1GB):

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBMSD_SD_HelloWorld_Mbed/)](https://developer.mbed.org/users/samux/code/USBMSD_SD_HelloWorld_Mbed/file/tip/main.cpp) 

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBMSD_SD_HelloWorld_FRDM-KL25Z/)](https://developer.mbed.org/users/samux/code/USBMSD_SD_HelloWorld_FRDM-KL25Z/file/tip/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/classUSBMSD.html) 

## How to use USBMSD with your own chip

The USBMSD class implements the MSD protocol. It gives access to a memory chip from a computer over USB. But this class doesn't work as a standalone; you need to subclass it and define pure virtual functions that are called in USBMSD.

Please inherit from USBMSD and define **all** of the following pure virtual functions:

* virtual ``int disk_read(uint8_t * data, uint64_t block)``: read a block.
* virtual ``int disk_write(const uint8_t * data, uint64_t block)``: write a block.
* virtual ``int disk_initialize()``: initialize the memory.
* virtual ``uint64_t disk_sectors()``: return the number of blocks.
* virtual ``uint64_t disk_size()``: return the memory size.
* virtual ``int disk_status()``: return the status of the storage chip (0: OK, 1: not initialized, 2: no medium in the drive, 4: write protection).

All function names are compatible with the FAT filesystem library. So you can imagine using your own class with USBMSD and the FAT filesystem library in the same program. Just be careful, because there are two different parts that will access the SD card. You can do a master/slave system using ``disk_status()``.

Once these functions are defined, you can call ``connect()`` (at the end of the constructor of your class, for instance) of USBMSD to connect your mass storage device. ``connect()`` will first call ``disk_status()`` to test the status of the disk. If ``disk_status()`` returns 1 (disk not initialized), then ``disk_initialize()`` is called. After this step, ``connect()`` will collect information such as the number of blocks and the memory size.

The flowing is a sample class, inheriting from USBMSB and accessing an SD card. You can follow this example and write your own class to access your storage chip:

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/samux/)](http://mbed.org/users/samux/file/USBMSD_SD/main.cpp>) 

