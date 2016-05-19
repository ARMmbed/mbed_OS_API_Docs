The I2C interface provides I2C Master functionality.

This interface can be used for communication with a I2C devices, such as serial memories, sensors and other modules or integrated circuits.

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/I2C_HelloWorld_Mbed/)](https://developer.mbed.org/users/mbed_official/code/I2C_HelloWorld_Mbed/file/tip/main.cpp) 

<div class="alert-box warning"> Remember, you will need a pull-up resistor on sda and scl.

All drivers on the I2C bus are required to be open collector, and so it is necessary for pull up resistors to be used on the two signals. A typical value for the pullup resistors is around 2.2k ohms, connected between the pin and 3v3. </div>

## API

<note> The mbed API uses 8 bit addresses, so make sure to take that 7 bit address and left shift it by 1 before passing it. </note> [![View code](https://www.mbed.com/embed/?type=library)](https://developer.mbed.org/users/mbed_official/code/mbed/docs/tip/classmbed_1_1I2C.html) 

## Interface

[![/media/uploads/chris/pinout-thumbnails.jpg](https://developer.mbed.org/media/uploads/chris/pinout-thumbnails.jpg)](https://developer.mbed.org/handbook/Pinouts)  
---  
[See the Pinout page for more details](https://developer.mbed.org/handbook/Pinouts)  
  
The default frequency of the I2C interface is 100KHz.

I2C is a two wire serial protocol that allows an I2C Master exchange data with an I2C Slave. The I2C protocol support upto 127 devices per bus. The I2C interface can be used for writing data words out of the I2C port, returning the data recieved back from I2C slave. The I2C clock frequency can be configured.

## References

  * [Wikipedia](http://en.wikipedia.org/wiki/I%C2%B2C)
