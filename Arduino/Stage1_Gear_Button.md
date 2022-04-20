# Brief Introduction
Author: Wish Kuo

The following script needs to be uploaded to Arduino board. Currently we are using **Arduino Uno**.

File path: `Arduino/GearButton.ino` (tmp)

# Dependency
It communicates with `Assets/Scripts/Train/GearButtonController.cs`

# `GearButtons.ino`
## Variables
``` C++
// DO NOT change the Pin mode unless you're modifying the way it wires
const int SmallGearButtonSwitch = 45; 
const int MedGearButtonSwitch = 43; 
const int LargeGearButtonSwitch = 41; 
const int DoneButtonSwitch = 39; 
const int UndoButtonSwitch = 37; 
const int ClearButtonSwitch = 35; 

const int SmallGearButtonLight = 33; 
const int MedGearButtonLight = 31; 
const int LargeGearButtonLight = 29; 
const int DoneButtonLight = 27; 
const int UndoButtonLight = 25; 
const int ClearButtonLight = 23; 
```

## Example pinMode usage
``` C++
pinMode(SmallGearButtonSwitch, INPUT);
```
Usage: define the pin `SmallGearButtonSwitch` as a "input" to read if the button is pressed

``` C++
pinMode(SmallGearButtonLight, OUTPUT);
```
Usage: define the pin `SmallGearButtonLight` as a "output", since it is a pin that connected to the LED light

For more information, reference to [Arduino's Doc about digital I/O](https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/)

## Example code sensing the button and light up the LED
``` C++
if (digitalRead(SmallGearButtonSwitch) == LOW){ // if the button is pressed
    digitalWrite(SmallGearButtonLight, HIGH); // Light up the LED
} else{
    digitalWrite(SmallGearButtonLight, LOW); // turn off the LED
}
```

## Example Serial port communication usage
-  `Serial.print(SmallGearButton);`: streaming `SmallGearButton` as a string to the serial port.