<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/introduction/index.html)</span>
# Introduction to the mbed OS API 

mbed OS lets you write applications that run on embedded devices, by providing the layer that interprets your application's code in a way the hardware can understand.

Your application code - written in C++ - uses the application programing interfaces (APIs) presented by mbed OS to receive information from the hardware and send instructions to it. This means that a lot of the challenges in getting started with microcontrollers or integrating large amounts of software is already taken care of.

<span class="tips">**Tip:** You can view [the full doxygen](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/index.html) on our site, or explore the [code on GitHub](https://github.com/ARMmbed/mbed-os/tree/mbed-os-5.2).</span>

The APIs in this document are organized by the feature, or group of features, they enable.

* [Task management](APIs/tasks/tasks.md): handling tasks and events in mbed OS.
* [Inputs and outputs](APIs/io/inputs_outputs.md): analog, digital, bus, port, PwmOut and interrupts.
* [Digital interfaces](APIs/interfaces/interfaces.md): serial, SPI, I2C and CAN.
* [Communication](APIs/communication/communication_index.md): network sockets, Ethernet, WiFi and BLE.
* [Security](APIs/security/security.md): working with mbed uVisor and mbed TLS in the context of mbed OS.

We also provide guidelines [for using the API documentation in the mbed Online Compiler](APIs/API_Documentation.md). 
