The USBAudio class enables the mbed to be recognized as an audio device. With this interface, you can receive and send audio packets from and to a computer (play a music,...) over USB. For instance you can connect a speaker or an I2S/I2C chip to the mbed and play the stream received from the computer.

The USB connector should be attached to 

  * **p31 (D+), p32 (D-) and GND** for the **LPC1768 and the LPC11U24**
  * The on-board USB connector of the **FRDM-KL25Z**

<div class="alert-box warning" title="Change the default sound board"> To send audio packets to the mbed, you have to change the default sound board used by the Operating system.   
On Windows, you can do this by clicking on:

  * **control panel**
  * **Hardware and Sound**
  * **Manage audio device** in the Sound section
  * Select the Mbed Audio device and press **Set default** </div>

## Hello World

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBAudio_HelloWorld/)](https://developer.mbed.org/users/samux/code/USBAudio_HelloWorld/file/tip/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/mbed_official/code/USBDevice/)](http://mbed.org/users/mbed_official/code/USBDevice/file/eaa07ce86d49/main.cpp>) 

## More examples

The following program is sending to a speaker all audio packets received. This means that you can play a music on your computer and listen it on your Mbed.

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/samux/)](http://mbed.org/users/samux/file/USBAUDIO_speaker/main.cpp>) 

The USBAudio playback example sends back to the computer all audio packets received. You can then listen for incoming audio packets with [audacity](http://audacity.sourceforge.net/) for instance.

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/samux/)](http://mbed.org/users/samux/file/USBAudioPlayback/main.cpp>) 

## In details

### Audio packet length

In this section, I will explain what kind of packets are received according to the frequency and the number of channels.   
  
An audio packet is received each millisecond. So let's say that a frequency of 48 kHz has been chosen with 2 channels (stereo). Knowing that each sample of a packet are 16 bits long, 48 * 2 bytes will be received each millisecond for one channel. In total, for 2 channels, 48 * 2 * 2 bytes will be received.

<div class="alert-box info" title="Compute the length packet"> **AUDIO_LENGTH_PACKET = (FREQ / 500) * nb_channel** </div>

### How to interpret an audio packet ?

The read() function fills an uint8_t array. But these data has to be interpreted as 16 bits signed data (PCM). Then PCM values can be handled according to the number of channels.

** MONO: single channel **

![https://developer.mbed.org/media/uploads/samux/pcm.png](https://developer.mbed.org/media/uploads/samux/pcm.png)

** STEREO: 2 channels **

When there are 2 channels, values for channel 1 and values for channel 2 will alternate as explained in the following diagram:

![https://developer.mbed.org/media/uploads/samux/pcm_stereo.png](https://developer.mbed.org/media/uploads/samux/pcm_stereo.png)

## Related

  * [USBMouse](USBMouse)
  * [USBKeyboard](USBKeyboard)
  * [USBMouseKeyboard](USBMouseKeyboard)
  * [USBHID](USBHID)
  * [USBMIDI](USBMIDI)
  * [USBSerial](USBSerial)
  * [USBMSD](USBMSD)
