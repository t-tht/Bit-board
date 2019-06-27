# Circuit Diagram

A stripboard was used to connect the fixed set of inputs (joystick and touch sensors) to the BBC micro:bit.

The following circuit diagram illustrates how the inputs are interconnected and shows the necessary components required to complement the inputs.

[![Image](https://github.com/t-tht/bitboard-docs/raw/master/Images/Circuit_Diagram.jpg "Circuit Diagram")](https://github.com/t-tht/bitboard-docs/raw/master/Images/Circuit_Diagram.jpg)

Based on the circuit diagram above, the components required for the prototype were soldered onto a stripboard. The following layout was designed to keep all the components compact in order to reduce the area required.

[![Image1](https://github.com/t-tht/bitboard-docs/raw/master/Images/strip_board_layout.jpg "Stripboard Layout")](https://github.com/t-tht/bitboard-docs/raw/master/Images/strip_board_layout.jpg)

### Components Required:
* [Joystick](#joystick)
* [Capacitive Touch Sensor](#capacitive-touch-sensor)
* [Bi-colour LED](#bi-colour-led)
* [Switch](#switch)
* [Resistor](#resistor)
* [micro:bit Breakout Board](#microbit-breakout-board)

## Joystick
[<img src="https://github.com/t-tht/bitboard-docs/raw/master/Images/Joystick.jpg" alt="Image2" title="2-Axis Joystick" width="150" height="150"/>](https://github.com/t-tht/bitboard-docs/raw/master/Images/Joystick.jpg)

The joystick used has two axes and it is commonly used to represent the arrow keys on a keyboard. Each axis is a potentiometer and they are independent of each other. By dividing the voltage applied to the joystick, analogue inputs can be sent to the analogue pins of the micro:bit. These analogue readings will then be used to set thresholds when determining the sensitivity of the joystick.

## Capacitive Touch Sensor
[<img src="https://github.com/t-tht/bitboard-docs/raw/master/Images/Touch_Sensor.jpg" alt="Image3" title="Capacitive Touch Sensor" width="150" height="150"/>](https://github.com/t-tht/bitboard-docs/raw/master/Images/Touch_Sensor.jpg)

This sensor can detect touch by measuring the change in capacitance. It is a breakout board that utilises the TTP223 IC, which has touch detection. The IC is in direct mode and has an active high output. This means that the output will go high whenever the touch pad is touched and will go back to low when touch is not detected. A digital pin from the micro:bit can be used to read the output of this sensor.

## Bi-colour LED
[<img src="https://github.com/t-tht/bitboard-docs/raw/master/Images/Bi-colour_LED.jpg" alt="Image4" title="Bi-colour LED" width="100" height="150"/>](https://github.com/t-tht/bitboard-docs/raw/master/Images/Bi-colour_LED.jpg)

Instead of a standard LED that has a single colour when it lights up, two colours were used to indicate different states: touched and not touched. This LED is either green or red when it lights up. It is essentially two LEDs that share a common cathode, causing it to have three leads.

## Switch
[<img src="https://github.com/t-tht/bitboard-docs/raw/master/Images/Switch.jpg" alt="Image5" title="SPDT Switch" width="150" height="150"/>](https://github.com/t-tht/bitboard-docs/raw/master/Images/Switch.jpg)

This is a MAX4544 switch, which is a single-pole, double-throw (SPDT) analogue switch. It can be used to complete either one of the two available circuits depending on a single digital input. When the input is driven high, the switch will close the circuit that is connected via the normally open (NO) terminal. If a low input is applied instead, the switch will then toggle back to the normally closed (NC) terminal.

## Resistor
[<img src="https://github.com/t-tht/bitboard-docs/raw/master/Images/Resistor.jpg" alt="Image6" title="Through Hole Resistor" width="150" height="150"/>](https://github.com/t-tht/bitboard-docs/raw/master/Images/Resistor.jpg)

The use of resistors in the prototype stripboard is to control the amount of current in the circuit, in order to prevent any damages to the LEDs. By allowing more current to flow, the LEDs will glow brighter. For a significant brightness, a 220 Ω resistor is used for green light and a 750 Ω resistor is used for red light.

## micro:bit Breakout Board
[<img src="https://github.com/t-tht/bitboard-docs/raw/master/Images/Breakout_Board.jpg" alt="Image7" title="Breakout Board" width="150" height="150"/>](https://github.com/t-tht/bitboard-docs/raw/master/Images/Breakout_Board.jpg)

An edge connector is used to allow direct access to the pins of the micro:bit. However, a breakout board is required for the prototype stripboard because the pitch of the edge connector is too narrow for the stripboard. The breakout board used also reduced the number of pins from the edge connector to the actual number of distinct pins available from the micro:bit.
