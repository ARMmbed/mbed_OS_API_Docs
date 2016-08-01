# Introduction to the mbed OS API 

mbed OS lets you write applications that run on embedded devices, by providing the layer that interprets your application's code in a way the hardware can understand.

Your application code - written in C++ - uses the application programing interfaces (APIs) presented by mbed OS to receive information from the hardware and send instructions to it. This means that a lot of the challenges in getting started with microcontrollers or integrating large amounts of software is already taken care of.

The APIs in this document are organized by the feature, or group of features, they enable.

* [Hardware inputs and outputs](APIs/io/inputs_outputs.md): analog, digital, bus, port, PwmOut and interrupts.
* [Digital interfaces and USB](APIs/interfaces/interfaces.md): serial, SPI, I2C, CAN and USB.
* [Networking and communication](APIs/communication/communication.md): network stack, BLE, Ethernet, WiFi and radio. 
* [Device and networking security](APIs/security/security.md): mbed uVisor and mbed TLS.
* [Task management](APIs/tasks/tasks.md): timers and RTOS.
* [Memory and file system](APIs/memory_files/memory_files.md): memory and file systems.


