# Brief Introduction
Author: Wish Kuo

The following script needs to be uploaded to Arduino board. Currently we are using Arduino Uno.

File path: `Arduino/GearButtons.ino` (tmp)

# Dependency
It communicates with `Assets/Scripts/Train/GearButtonController.cs`

# `GearButtons.ino`
## Variables
``` C++
// DO NOT change the Pin mode unless you're modifying the way it wires
int SmallGearButtonSwitch = 45; 
int MedGearButtonSwitch = 43; 
int LargeGearButtonSwitch = 41; 
int DoneButtonSwitch = 39; 
int UndoButtonSwitch = 37; 
int ClearButtonSwitch = 35; 

int SmallGearButtonLight = 33; 
int MedGearButtonLight = 31; 
int LargeGearButtonLight = 29; 
int DoneButtonLight = 27; 
int UndoButtonLight = 25; 
int ClearButtonLight = 23; 
```

## Example pinMode usage
- `pinMode(SmallGearButtonSwitch, INPUT);`: define the pin `SmallGearButtonSwitch` as a  "input" to sense if the button is pressed
- `pinMode(SmallGearButtonLight, OUTPUT);`: define the pin `SmallGearButtonLight` as a  "output", since it is a pin that connected to the LED light
- For more information, reference to [Arduino's Doc about digital I/O](https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/)

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