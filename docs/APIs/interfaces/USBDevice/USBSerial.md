# USBSerial

Use the USBSerial interface to emulate a serial port over USB. You can use this serial port as an extra serial port or as a debug solution. It's also a great solution to easily communicate between your mbed board and a computer.

The USB connector should be attached to: 

* **p31 (D+), p32 (D-) and GND** for the **LPC1768 and the LPC11U24**.
* The on-board USB connector of the **FRDM-KL25Z**.

## If you're on Windows

You need a configuration file to use USBSerial on Windows:

1. Download this [archive](https://developer.mbed.org/media/uploads/samux/serial.zip) and extract the `.ini` file it contains.
1. When you plug your USBSerial serial device, Windows will try to find an existing driver for it; wait for this process to fail.
1. Go into the device manager to find the unknown device:
	1. Right click on the device.
	1. Click on **Update driver software**.
	1. Click on **Browse my computer for driver software**.
	1. Select the path to which you extracted ``serial.inf``.
	1. Click **Next**.
	1. Windows will raise a warning; accept it. 
1. You should now have an mbed Virtual Serial Port.

Because ``product_id`` and ``vendor_id`` are hardcoded in the ``.inf`` file, if you don't want to use default values, you will have to change them in your program _AND_ in the ``.inf`` file.

## Hello World

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBSerial_HelloWorld/)](https://developer.mbed.org/users/samux/code/USBSerial_HelloWorld/file/tip/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/mbed_official/code/USBDevice/)](http://mbed.org/users/mbed_official/code/USBDevice/file/6d85e04fb59f/main.cpp) 

## Example

In this example, the program waits for a line on the virtual serial port. When it receives a line, it sends it to the usual mbed serial port (the one used to flash a new program) as well as to the virtual one:

```
#include "mbed.h"
#include "USBSerial.h"

//Virtual serial port over USB
USBSerial serial;
Serial pc(USBTX, USBRX);

int main(void) {
    uint8_t buf[128];
    while(1)
    {
        serial.scanf("%s", buf);
        serial.printf("recv: %s", buf);
        pc.printf("recv: %s\r\n", buf);
    }
}
```
