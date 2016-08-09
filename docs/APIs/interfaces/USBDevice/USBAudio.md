# USBAudio

If you want to use your mbed board as an audio device, you can use the USBAudio class to receive and send audio packets from and to a computer (play music) over USB. For instance, you can connect a speaker or an I2S/I2C chip to the board and play the stream received from the computer.

The USB connector should be attached to:

* **p31 (D+), p32 (D-) and GND** for the **LPC1768 and the LPC11U24**.
* The on-board USB connector of the **FRDM-KL25Z**.

## Change the default sound board

To send audio packets to the board, you have to change the default sound board used by your computer's operating system. 

On Windows:

1. Click on **Control panel** > **Hardware and Sound** > **Manage audio device**. 
1. In the **Sound** section, select the mbed audio device.
1. Click **Set default**.

## Hello World

[![View code](https://www.mbed.com/embed/?url=https://developer.mbed.org/users/samux/code/USBAudio_HelloWorld/)](https://developer.mbed.org/users/samux/code/USBAudio_HelloWorld/file/tip/main.cpp) 

## API

[![View code](https://www.mbed.com/embed/?type=library)](https://docs.mbed.com/docs/mbed-os-api/en/mbed-os-5.1.0/api/classUSBAudio.html) 

## Examples

Send received audio packets to a speaker. This means that you can play music on your computer and listen to it on your mbed board.

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/samux/)](http://mbed.org/users/samux/file/USBAUDIO_speaker/main.cpp>) 

A second version of this example allows you to listen to the music using [audacity](http://audacity.sourceforge.net/) for instance.

[![View code](https://www.mbed.com/embed/?url=<http://mbed.org/users/samux/)](http://mbed.org/users/samux/file/USBAudioPlayback/main.cpp>) 

## In detail

### Audio packet length

In this section, I will explain what kind of packets are received according to the frequency and the number of channels.   

An audio packet is received each millisecond. So let's say that a frequency of 48 kHz has been chosen with two channels (stereo). Knowing that each sample of a packet is 16 bits long, 48 * 2 bytes will be received each millisecond for one channel. In total, for two channels, 48 * 2 * 2 bytes will be received.

<span class="tips">**Tip:** Compute the length packet </br>``AUDIO_LENGTH_PACKET = (FREQ / 500) * nb_channel``</span>

### How to interpret an audio packet

The ``read()`` function fills an ``uint8_t`` array. But this data has to be interpreted as 16 bits signed data (PCM). Then PCM values can be handled according to the number of channels.

### MONO: single channel

<span class="images">![](../../images/single_channel.png)</span>

### STEREO: two channels

When there are two channels, values for channel 1 and values for channel 2 will alternate as explained in the following diagram:

<span class="images">![](../../images/two_channels.png)</span>
