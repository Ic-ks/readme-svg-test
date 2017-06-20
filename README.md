# readme-svg-test
How does it work
---------------------
The WS2812B protocol needs 24 bits of data (8 bit per color channel) to set the color of one LED. Every further LED of a strip needs another 24 bit long data block. The transmission of these bits is done by sending a chain of high and low voltage pulses to the data line of the LED. 

In detail: Eah separate bit is defined by a high voltage pulse which is followed by a low voltage pulse. Both pulses must have an exact specified duration, so that they are recognized as 1 or 0 bit: 
![Alt text](https://rawgit.com/Ic-ks/readme-svg-test/master/ws2812b-timings.svg "Timings")
* A 1 bit is defined by a high voltage pulse with a duration of 850 ns which is followed by a low voltage pulse of 400 ns
* A 0 bit is defined by a high voltage pulse with a duration of 400 ns which is followed by a low voltage pulse of 850 ns
* Each pulse can have a deviation of +/- 150 ns 

At the moment there is no direct solution to send such short timed pulses with **different durations** by the API of Android Things. There is however the [Serial Peripheral Interface (SPI)](https://developer.android.com/things/sdk/pio/spi.html) which is able to send short timed pulses with the **exact same** duration: 
* The duration in nanoseconds of the pulses is defined by the frequency of the SPI. 
* A transmitted 1 bit sends a high voltage pulse to the MOSI (Master Out Slave In) pinout 
* A transmitted 0 bit sends a low voltage the MOSI pinout
The solution to control the WS2812B LEDs by the SPI is to find a bit pattern and a frequency so that an assembly of SPI bits represent one WS2812B bit.
This approach is using 3 bits to represent 1 WS2812B bit:
