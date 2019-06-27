# Test & Verification

To ensure that the prototype is working as intended, a few tests were conducted at different stages to identify any faults in hardware and bugs in software.

## Hardware
### Test 1: Individual electronic components
* Joystick  
When the joystick knob was slowly moved towards the **up/ right** position, their corresponding output values increase until slightly below the **3V** applied. The output values work the other way round when the knob was moved towards the **down/ left** position, decreasing until close to **GND**.

* Touch Sensor  
When a finger was **placed** on the touch sensor pad, the output signal went **high**. When **removed**, the signal went **low**.

### Test 2: Multiple components on a breadboard
* LED, Switch & Touch Sensor  
When the touch pad was **touched**, the colour of the LED changed from **red** to **green**. When the finger was lifted off the pad, the colour changed back to **red**.

* Joystick & Touch Sensor Circuit  
When the joystick and the touch sensor circuit were used together, both outputs were still detectable since they worked **independently**.

### Test 3: Breadboard with micro:bit
* Joystick  
The following programme was used to test the joystick with the **analogue** input pins.

  When the joystick knob is moved in a specific direction then returned to the middle, the red dot on the LED screen moved a step in that direction as well. The threshold values were adjusted to set the sensitivity of the joystick.

``` javascript
let y = 0
let x = 0
let reading_y = 0
let reading_x = 0
let dot = game.createSprite(2, 2)
basic.forever(function(){
    reading_x = pins.analogReadPin(AnalogPin.P0)
    reading_y = pins.analogReadPin(AnalogPin.P1)
    if (reading_x > 800){
        x = x + 1
    } 
    else if (reading_x < 200){
        x = x - 1
    } 
    else{
        x = x
    }
    if (reading_y > 800){
        y = y - 1
    } 
    else if (reading_y < 200){
        y = y + 1
    } 
    else{
        y = y
    }
    dot.set(LedSpriteProperty.X, x)
    dot.set(LedSpriteProperty.Y, y)
    basic.pause(100)
})
```

* Touch Sensor  
The following programme was used to test the touch sensor with the **digital** input pins.

  When one of the numbered touch pads were touched, the corresponding number appeared on the LED screen.

``` javascript
basic.forever(function(){
    if (pins.digitalReadPin(DigitalPin.P13) == 1){
        basic.showNumber(1)
    }
    if (pins.digitalReadPin(DigitalPin.P14) == 1){
        basic.showNumber(2)
    }
    if (pins.digitalReadPin(DigitalPin.P15) == 1){
        basic.showNumber(3)
    }
    if (pins.digitalReadPin(DigitalPin.P16) == 1){
        basic.showNumber(4)
    }
    if (pins.digitalReadPin(DigitalPin.P8) == 1){
        basic.showNumber(8)
    }
})
```

* Both Inputs Simultaneously  
The two programmes used were combined to test whether the micro:bit was able to supply **enough current** for the implementation of the two inputs. Both results were able to be displayed, showing that the inputs can be used together.

### Test 4: Electrical conductivity
* Pencil (Graphite) Track  
Graphite conducts electricity but it could not be used to replace wires because it has a very **high resistance**, causing a huge voltage drop from one end to the other. It was used as a **potentiometer** instead since different voltage readings could be obtained at different points.

* Copper Tape  
Copper is a good conductor and there is no voltage drop across it. The **adhesive side** of the copper tape is **conductive** as well.

* Cable Clip  
The clip is made from **copper but plated with tin**. The clip is able to conduct electricity despite having the coating. allowing the clip to be used not just for **securing the paper canvas** but also as an electrical connection point.

## Software
### Test 1: Custom MakeCode extension
Custom blocks that provide different keys of a keyboard were created using TypeScript and they were tested on the [MakeCode Playground](https://makecode.com/playground) to visualise the format of the blocks.

### Test 2: Custom keyboard services
Instead of just sending "string" via Bluetooth, keyboard commands such as _enter_ and _backspace_ were tested using the microbit_presenter Mbed file.

## Hardware & Software Integration
### Test 1: Fixed inputs
* Joystick  
 **Thresholds** for the analogue inputs were adjusted via **trial and error** to give the best joystick sensitivity. When the joystick knob was moved, the **cursor** on a word document moved in the same direction as well.

* Touch Sensor  
Some keys were represented by the touch sensors and they were outputted on a **laptop screen** when the touch pads were touched. **Continuous** outputs were shown on the screen when the finger was **resting** on the touch pad.

### Test 2: Paper canvas
* Pencil (Graphite) Track  
Crocodile clips were used to connect one of the cable clips that corresponded to an **analogue** input pin to a particular position on the graphite track. Different analogue readings could be obtained at different positions on the graphite track.

* Copper Tape  
Copper tape was used on the paper and the cable clips were in contact with the tape. When the copper tape was powered with 3V, the cable clip corresponding to a **digital** input pin could be detected, leading to a key output on the laptop screen.

### Test 3: Demo examples
* Online Game (Crossy Road)  
Simple online games that only require basic keys such as the **arrow keys** and some other keys, like **WASD** and **spacebar**, can be played using the prototype. 

* Custom Game (Wire Loop Game)  
Copper tape was used to create **boundaries** of a track and connected to the 3V cable clip. The micro:bit will detect a **high signal** when the tip of the crocodile clip touches the tape. The game interface was created using **Python** to receive key presses from the micro:bit.

* Output Selector  
Different threshold points were used to set the boundaries on the graphite track for different output modes. 4 modes were made to illustrate **letters**, **numbers**, **symbols** and **commands**. The touch pads will then correspond to the different key outputs based on the position of the crocodile clip.
