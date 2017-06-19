# readme-svg-test
How does it work
---------------------
The WS2812B protocol needs 24 bits of data to set the color of one LED. Each bit is defined by a short high voltage pulse which is followed by a low voltage pulse: 
* A 1 bit is defined by a high voltage pulse with a duration of 850 ns which is followed by a low voltage pulse of 400 ns
* A 0 bit is defined by a high voltage pulse with a duration of 400 ns which is followed by a low voltage pulse of 850 ns
* Each pulse can have a deviation of +/- 150 ns 

![Alt text](https://rawgit.com/Ic-ks/readme-svg-test/master/ws2812b-timings.svg "Timings")

At the moment there is no direct solution to send pulses with different time durations by the API of Android Things. There is however the [Serial Peripheral Interface (SPI)](https://developer.android.com/things/sdk/pio/spi.html) which is able to send a 1 bit by one pulse of high voltage or a 0 bit by one pulse of low voltage. Both pulses have the exact same time duration because they are defined by the frequency of the SPI.
The solution to control the WS2812B LEDs by the SPI is to find a bit pattern and a frequency so that a couple of SPI bits represent one WS2812B bit:
