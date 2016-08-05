# USBHostHub

<span class="warnings">**Warning:** Library in beta</br>This library is in beta. If you have any problems using the USBHost library, please send a bug report to [support@mbed.org](support@mbed.org). </span>

The USB Host connector should be attached to:

* **p31 (D+), p32 (D-) and GND** for the **LPC1768**.
* Add **two 15k resistors tied to GND on D+ and D-**.

## Hello World

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBHostHub_HelloWorld/)](https://developer.mbed.org/users/samux/code/USBHostHub_HelloWorld/file/tip/main.cpp) 

As you can see there is no instance of USBHostHub in the code. All the hubs are automatically enumerated by the USB thread.

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/USBHostHub_8h_source.html)

## Troobleshooting

If your mbed board is automatically reset when you plug a USB device, you should consider using an external power supply.
