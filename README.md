# Arduino-keyboard-for-TRS-80-M2


The TRS-80 Model II keyboard is rather bulky and occupies a lot of desk space. Sometime you even do not have a proper working keyboard.
This project describes a solution where you use Putty running on a Windows or Linux computer connected to an Arduino as the keyboard replacement for a TRS-80 Model II.
The model 16 uses the same keyboard hardware (keyboard without cable).

<img width="300"  src="https://github.com/user-attachments/assets/38e45ba9-abd1-4610-a2d1-7861c1418159" />


This project can also be used for the Models 12, 16B & 6000. These models use a keyboard with cable.
Please note that the pins on the keyboard connector of these computers are different from the Model II !!


## Functions

* All standard keys are transferred to the Model II.
* Each key typed is echoed to the Putty terminal as well. Also the built-in led on the arduino will light up.
* Arrow keys on the computer keyboard are translated into the Model II keyboard code.
* Backspace is translated to the correct code for the Model II.
* Function keys F1 ~ F8 are translated into the proper codes for function keys of the Model II and Model 12.
* Function key F9 is translated into the BREAK code (03)
* Function key F10 is translated into the HOLD code (00). When pressed, the message [HOLD] will be shown of the Putty screen.

* The line is monitored anyway to detect the presence of a powered on Model II. When the Model II is not connected, or powered down, the led will blink fast.

The BUSY* line coming from the Model II is not used to wait until the Model II is ready to receive a key. The Model II is much faster than the typist. 

When starting up (or after pressing reset on the Arduino) a welcome text will be printed on the Putty screen. This will also show the status of the Model II.
This message can also be triggered by typing Cntrl-t.

During normal operation, the build-in led blinks briefly every 8 second.
When the Model II is not connected or powered off the built-in led blinks fast at 4 Hertz to indicate an abnormal situation.
In this situation the Arduino will still transmit characters using the DIN connection. It does not check the BUSY* line.
This allows checking of the workings with a Tandy computer connected. 

## Hardware

The smallest Arduino I could find was good enough (without additional circuitry). You only essentially need to add a 5 pin 180 degrees DIN female connector.
Arduine used: Arduino Nano
Processor: ATmega 168
These boards are available with different USB connectors. I have used mini USB for my unit.

<img width="300" alt="click to enlarge" src="https://github.com/user-attachments/assets/7eec2c83-32c4-4d01-a011-8550477db702" />
<BR><BR>

>[!CAUTION]
> **As the Arduino receives power via USB from the Windows or Linux PC, the 5 Volt from the Tandy computer must NOT be connected to the Arduino. Doing this anyway might damage the PC, the Arduino and/or the Tandy computer!**


When building the Arduino into a small enclosure you might want to add and extra push button for the reset (temporarely connect to ground), and an additional led with resistor (e.g. 220 ohms) connected from D13 to ground. 

<img width="300"  src="https://github.com/user-attachments/assets/b4b0e0d9-66c4-4043-8b78-8d2a7726f5ef" />
<img width="300"  src="https://github.com/user-attachments/assets/026540a4-2391-4acd-8c78-d64c4d319a53" />

I used a momentary on switch with embedded led on top of the little enclosure.



The pins of the connector are linked to the Arduino in the following manner:


| Model II DIN connector | Model 12 DIN connector | Arduino Nano |
|--|--|--|
| 1  DATA | 1 DATA | D4 |
| 2  BUSY* | 3 BUSY* | D2 |
| 3  Gnd | 5 Gnd | Gnd |
| 4  CLOCK | 2 CLOCK | D3 |
| 5  +5V | 4 +5V | NOT USED |

Additional signals:

| Function | Arduino Nano |
| -- | -- |
| Same as built-in led | D13 |
| Reset line | RST |

## Putty settings

To work correctly, Putty emulation must be set to VT100+ mode. Otherwise the function keys do not work correctly.
Local echo should be off.
On a Windows PC, the OS might assign a differt serial port to the USB connection to the Arduino. On my system this is normaly port 6 or 7. The system manager will show which port is used.

## Background information

Detailed information on the keyboard interface can be found in the Model II technical reference manual. Essential is the timing relationship between the DATA and CLOCK signals.

<img width="900"  alt="Keyboard timing diagram" src="https://github.com/user-attachments/assets/6c5b35c3-feb5-4467-a511-fb046971d69c" />

The exact frequency of the CLOCK signal is not critical. The timing is determined by the value of the QuarterPulse.
The signals are created in 4 parts where the signal level for the DataBit and ClockBit are set high or low. After sending the data the stop bit is transmitted.
The current code gives pulse width of about 40 micro seconds.

HERE SCREENSHOT 

## Notes

During startup, the Tandy computer might detect one or two spurious keystrokes. Possibly caused by the fact that the Arduino is initialising. I have not been able to fix this.


