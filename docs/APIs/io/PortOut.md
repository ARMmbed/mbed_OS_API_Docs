# PortOut

The PortOut interface is used to write to an underlying GPIO port as one value. This is much faster than [BusOut](BusOut.md) as you can write a port in one go, but much less flexible as you are constrained by the port and bit layout of the underlying GPIO ports.

A mask can be supplied so only certain bits of a port are used, allowing other bits to be used for other interfaces. 

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/PortOut_8h_source.html) 

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/PortOut_HelloWorld/)](https://developer.mbed.org/users/mbed_official/code/PortOut_HelloWorld/file/9bbfdb1487ff/main.cpp) 

