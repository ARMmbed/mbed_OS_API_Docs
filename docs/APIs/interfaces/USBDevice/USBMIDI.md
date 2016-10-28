# USBMIDI


Use the USBMIDI interface to send and receive MIDI messages over USB using the standard USB-MIDI protocol.

You can:

* Send MIDI messages to a computer. For example,record in a sequencer or trigger a software synthesizer.
* Receive messages from a computer. For example, actuate things based on MIDI events.

The USB connector should be attached to: 

* **p31 (D+), p32 (D-) and GND** for the **LPC1768 and the LPC11U24**.
* The on-board USB connector of the **FRDM-KL25Z**.

## Video tutorial 

<span class="images">[![Video tutorial](http://img.youtube.com/vi/pRiYQ6Dv-uY/0.jpg)](http://www.youtube.com/watch?v=pRiYQ6Dv-uY)</span>

## Hello World

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBMIDI_HelloWorld/)](https://developer.mbed.org/users/samux/code/USBMIDI_HelloWorld/file/tip/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.2/api/classUSBMIDI.html) 

## Example

Send a MIDI message if the buttons change state:

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBMIDI_Receive/)](https://developer.mbed.org/users/samux/code/USBMIDI_Receive/file/tip/main.cpp) 


