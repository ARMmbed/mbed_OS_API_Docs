<div side style="float: right"> ![http://www.youtube.com/watch?v=pRiYQ6Dv-uY](http://www.youtube.com/watch?v=pRiYQ6Dv-uY) </div>

The USBMIDI interface can be used to send and receive MIDI messages over USB using the standard USB-MIDI protocol.

Using this library, you can do things like send MIDI messages to a computer (such as to record in a sequencer, or trigger a software synthesiser) and receive messages from a computer (such as actuate things based on MIDI events)

The USB connector should be attached to 

  * **p31 (D+), p32 (D-) and GND** for the **LPC1768 and the LPC11U24**
  * The on-board USB connector of the **FRDM-KL25Z**

## Hello World

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBMIDI_HelloWorld/)](https://developer.mbed.org/users/samux/code/USBMIDI_HelloWorld/file/tip/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/mbed_official/code/USBDevice/)](http://mbed.org/users/mbed_official/code/USBDevice/file/6d85e04fb59f/main.cpp>) 

## More example

In this example, you can control the MIDI message sent with buttons   
[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBMIDI_Receive/)](https://developer.mbed.org/users/samux/code/USBMIDI_Receive/file/tip/main.cpp) 

## Related

  * [USBMouse](USBMouse)
  * [USBKeyboard](USBKeyboard)
  * [USBHID](USBHID)
  * [USBMouseKeyboard](USBMouseKeyboard)
  * [USBSerial](USBSerial)
  * [USBAudio](USBAudio)
  * [USBMSD](USBMSD)
