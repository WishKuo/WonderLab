# Brief Introduction
Author: Wish Kuo

The following script needs to be uploaded to Arduino board. Currently we are using **Arduino Mega**.

File path: `Arduino/ColorSlidersButton.ino` (tmp)

# Dependency
It communicates with `Assets/Scripts/Train/TrainColorController.cs`

# `ColorSlidersButton.ino`
## Variables
``` C++
// DO NOT change the Pin mode unless you're modifying the way it wires
const int UpPin = 4;
const int DownPin = 5;
const int LeftPin = 6;
const int RightPin = 7;

const int DonePressSwitch = 2;
const int DoneLight = 3;
const int ApplySwitch = 8;
const int ApplyLight = 11;

// Define analog input pin
#define RED_SLIDER A0
#define GREEN_SLIDER A1
#define BLUE_SLIDER A2
```

## Example pinMode usage
``` C++
pinMode(RED_SLIDER, INPUT_PULLUP);
```
Usage: define the pin `RED_SLIDER` as a "input" to read the value of the slider

``` C++
pinMode(UpPin, INPUT_PULLUP);
```
Usage: define the pin `UpPin` as a "input" to read the input from the joy stick

``` C++
pinMode(DonePressSwitch, INPUT);
```
Usage: define the pin `DonePressSwitch` as a "input" to read if the button is pressed

``` C++
pinMode(DoneLight, OUTPUT);
```
Usage: define the pin `DoneLight` as a  "output", since it is a pin that connected to the LED light

For more information, reference to [Arduino's Doc about digital I/O](https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/)

## Example code sensing the button and light up the LED
``` C++
if (digitalRead(DonePressSwitch) == LOW){ // if the button is pressed
    digitalWrite(DoneLight, HIGH); // Light up the LED
} else{
    digitalWrite(DoneLight, LOW); // turn off the LED
}
```

## Example Serial port communication usage
-  `Serial.print(analogRead(RED_SLIDER))`: streaming the value of the analog input `RED_SLIDER` as a string to the serial port.
-  `Serial.print(up);`: streaming `up` as a string to the serial port.