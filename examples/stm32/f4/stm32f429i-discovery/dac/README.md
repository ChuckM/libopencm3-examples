# README

An exploration of the STM32F4 DAC

This provides a collection of all the wave generating capabilities of
the DAC that don't use dual modes. Press the user button to switch
modes. The mode is output to TX1 at 115200 baud when you switch.

Example waves include two sawtooth waves, a square wave, white noise,
a triangle and a sine wave with added noise.

Some terms used in the descriptions:

- *Software-loaded* - The value is loaded to the DAC by the CPU.
- *Software-triggered* - The next value of a generated wave is 
  triggered by the CPU setting a control bit.
- *Timer-triggered* - The next value of a generated wave is triggered
  by a timer.
- *DMA transferred* - Values are loaded from memory via DMA.
