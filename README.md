# Arduino-keyboard-for-TRS-80-M2
The TRS-80 Model II keyboard is rather bulky and occupies a lot of desk space. Sometime you even do not have a proper working keyboard.
This project describes a solution where you use Putty running on a Windows or Linux computer connected to an Arduino as the keyboard replacement for a TRS-80 Model II.


**Functions**

All standard keys are transferred to the Model II.
Arrow keys on the computer keyboard are translated into the Model II keyboard code.
Backspace is translated to the correct code for the Model II.
Function keys F1 ~ F8 are translated into the proper codes for function keys of the Model II and Model 12.
Function key F9 is translated into the BREAK code
Function key F10 is translated into the HOLD code (00). When pressed, the messahe [HOLD] will be shown of the Putty screen.

Each key typed is echoed to the Putty terminal as well. Also the led on the arduino will light up.
The BUSY* line coming from the Model II is not used to wait until the Model II is ready to receive a key. The Model II is much faster than the typist. 
The line is monitored anyway to detect the presence of a powered on Model II. When the Model II is not connected, or powered down, the led will blink fast.

When starting up (or after pressing reset on the Arduino) a welcome text will be printed on the Putty screen. This will also show the status of the Model II.
This message can also be triggered by typing Cntrl-t.

When in normal operation, the build-in led blinks briefly every 8 second.
When the Model II is not connected or powered off the build-in led blinks fast at 4 Hertz to indicate an abnormal situation.

**Hardware**

The smallest Arduino I could find does this job. without additional circuitry. You only need to add a 5 pin 180 degrees DIN female connector.
Arduine used: Arduino Nano
Processor: ATmega 168

When building the Arduino into a small enclosure you might want to add and extra push button for the reset, and an addition led 


The pins of the connector are linked to the Arduino in the following manner:


| Model II DIN connector | Model 12 DIN connector | Arduino Nano |
|--|--|--|
| 1  DATA | 1 DATA | D4 |
| 2  BUSY* | 3 BUSY* | D2 |
| 3  Gnd | 5 Gnd | Gnd |
| 4  CLOCK | 2 CLOCK | D3 |
| 5  +5V | NOT USED | NOT USED |

Additional signals:

| function | Arduino Nano |
| -- | -- |
| Same as build-in led | D13 |
| Reset line | RST |

This project can also be used for the Model 12 & 6000.
Please note that the pins on the keyboard connector of these computers are different from the Model II !!

**Background information**

Detailed nformation on the keyboard interface can be found in the Model II technical reference manual. Essential is the timing relationship between the DATA and CLOCK signals.
The exact frequency of the CLOCK signal is not critical. The tinming is determined by the value of the QuarterPulse.
The signals are created in 4 parts where the signal level for the DataBit and ClockBit are set high or low. After sending the data the stop bit is transmitted

