AVR Sine wave generator using the Direct Digital Synthesis method. The final goal is to make it generate standard PL tones (in the range from 67Hz to 254Hz) for ham-radio transceivers which lack this options themselves.

Theory of operation
------------
Timer/Counter1 is configured to run in CTC (Clear Timer on Compar) free running mode. It's TOP value is defined in the ICR1 register, and the Input Capture Interrupt is enabled. On every match of the 16-bit timer with the vlue in ICR1, the TIMER1_CAPT_vect service routine is executed.

This service routine (which is executed at a rate of fs - sampling frequency, defined by the value in ICR1) increments the Phase Accumulator variable - phaseRegister by the so called "tuning word" M.

The 8-MSB bits of the phaseRegister used as a index to a 2^8-bit sine wave table, and it's value is outputted to a 8-bit DAC on PORTD.

Formula for calculatin the output frequency of the generated sine wave :

fsine = (M * fs)/2^n

- M = The tuning word
- fs = The sampling frequency i.e. the frequency at which the ISR fires
- 2^n = Size of the phase acumulator, 32bits in this case so 2^32

DAC
------------
On the output of PORTD (8-bit) there is a 8-bit R-2R ladder, made up of resistors. You can read more about this type of DAC on wikipedia : https://en.wikipedia.org/wiki/Resistor_ladder

calculations.ods
------------
This LibreOffice Calc file contains a table with calculations of the value for the ICR1 register for different PL-tone frequencies
