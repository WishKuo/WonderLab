# Brief Introduction
Author: Wish Kuo

The following script needs to be uploaded to Arduino board. Currently we are using **Arduino Uno**.

File path: `Arduino/WhistleButton.ino` (tmp)

# Dependency
It communicates with `Assets/Scripts/Train/TrainWhistleRecord.cs`

# `GearButtons.ino`
## Variables
``` C++
// DO NOT change the Pin mode unless you're modifying the way it wires
const int DonePressSwitch = 2;
const int DoneLight = 3;
const int PlayPressSwitch = 4;
const int PlayLight = 5;
const int RecordPressSwitch = 6;
const int RecordLight = 7;
```

## Example pinMode usage
``` C++
pinMode(DonePressSwitch, INPUT);
```
Usage: define the pin `DonePressSwitch` as a "input" to read if the button is pressed

``` C++
pinMode(DoneLight, OUTPUT);
```
Usage: define the pin `DoneLight` as a "output", since it is a pin that connected to the LED light

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
-  `Serial.print(DoneButton);`: streaming `DoneButton` as a string to the serial port.