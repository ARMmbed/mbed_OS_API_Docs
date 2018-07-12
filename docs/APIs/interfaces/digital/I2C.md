<span class="warnings">**Out of date**: This is not the most recent version of this page. Please see [the most recent version](https://os.mbed.com/docs/latest/reference/i2c.html)</span>
# I2C

The I2C interface provides I2C Master functionality. I2C is a two wire serial protocol that allows an I2C Master to exchange data with an I2C Slave. You can use it to communicate with I2C devices such as serial memories, sensors and other modules or integrated circuits. 

The I2C protocol supports up to 127 devices per bus, and its default clock frequency is 100KHz.

## Hello World!

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/mbed_official/code/I2C_HelloWorld_Mbed/)](https://developer.mbed.org/users/mbed_official/code/I2C_HelloWorld_Mbed/file/tip/main.cpp) 

<span class="warnings">**Warning:** Remember that you will need a pull-up resistor on sda and scl.</br>
All drivers on the I2C bus are required to be open collector, and so it is necessary to use pull-up resistors on the two signals. A typical value for the pull-up resistors is around 2.2k ohms, connected between the pin and 3v3. </span>

## API

<span class="notes">**Note:** The mbed API uses 8 bit addresses, so make sure to left-shift 7 bit address by 1 bit before passing them. </span> 

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/I2C_8h_source.html)


## References

  * [Wikipedia](http://en.wikipedia.org/wiki/I%C2%B2C)
