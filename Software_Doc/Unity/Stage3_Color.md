# Brief Introduction
Author: Wish Kuo

The following scripts are related to the Color (3rd) Station.
Each function is fully implemented, and is described below. For detailed information please reference to the comments in the scripts.

# Dependency (Where should put the scripts)
- On the train (local): `Assets/Scripts/Train/TrainColorController.cs`
- In the main game scene (global):
    - Link Arduino to Unity: `Assets/Scripts/Train/RGBSliderController.cs`

# `TrainColorController.cs`
This script should be added to the prefab of the train.

It receives the input from either physical inputs (buttons, sliders, joy stick) or a keyboard, and then makes the action.

For the color mixing, currently we are using RGB system. However, RYB and CMYK systems are also implemented in the script.

## Variables 
### Needed to assign
- `private RGBSliderController rgb_slider`: will automatically assign `RGBSliderController.cs` in `Start()`
- `private List<GameObject> train_g1`: assign the parts of the train that you want it to be change at the same time (Group 1)
- `private List<GameObject> train_g2`: assign the parts of the train that you want it to be change at the same time (Group 2)
- `private List<GameObject> train_g3`: assign the parts of the train that you want it to be change at the same time (Group 3)
- `private List<GameObject> train_g4`: assign the parts of the train that you want it to be change at the same time (Group 4)

### Current state
- `public GameObject tmp_color`: indicate to the color that is modifying by the user
- `public GameObject cur_color`: indicate to the color that is currently on the parts of the train
- `private int current_selected_group_id`: indicate to the group ID that is currently selected
- `private List<GameObject> current_selected_group`: indicate to the actual GameObjects (parts of the train) that are currently selected 

### For Debug purpose
- `private bool keyboardInput`: check (set to true) on the inspector if no physical inputs are connected
- `private List<Slider> RGB`: assign the sliders on the UI, if you wish to test the rgb sliders directly through pc (no physical inputs required)

## Functions
``` C#
/// <param name="r">origin input: value of red, should be in the range from 0.0f ~ 1.0f</param>
/// <param name="y">origin input: value of yellow, should be in the range from 0.0f ~ 1.0f</param>
/// <param name="b">origin input: value of blue, should be in the range from 0.0f ~ 1.0f</param>
public List<float> RYBtoRGB(float r, float y, float b); // Convert RYB color to RGB color mixing
```
Reference: [Trilinear interpolation](https://en.wikipedia.org/wiki/Trilinear_interpolation)

``` C#
/// <param name="c">origin input: value of cyan, should be in the range from 0.0f ~ 1.0f</param>
/// <param name="m">origin input: value of magenta, should be in the range from 0.0f ~ 1.0f</param>
/// <param name="y">origin input: value of yellow, should be in the range from 0.0f ~ 1.0f</param>
/// <param name="k">origin input: value of black, should be in the range from 0.0f ~ 1.0f</param>
public List<float> CMYKtoRGB(float c, float m, float y, float k); // Convert CMYK color to RGB color mixing
```
Reference: [CMYK to RGB formula](https://www.101computing.net/cmyk-to-rgb-conversion-algorithm/)

# `RGBSliderController.cs`
This script should be added in the main game scene which has to be active at the very beginning of the game.
It will connect to the Arduino through [serial communication](https://create.arduino.cc/projecthub/raisingawesome/unity-game-engine-and-arduino-serial-communication-12fdd5).
Also, in order to avoid the lagging issue while plug in multiple Arduino boards, this code implements multi-threading.

This script is required to pass the physical input information to `TrainColorController.cpp`.

Note: DO NOT DELETE `void OnDisable()` and `void OnApplicationQuit()`, otherwise the program will get stuck and may crash the game.

## Variables 
### Configuration
- `SerialPort sp = new SerialPort("COM6", 9600);`: check which port is plug in using the Arduino IDE.

### For Debug purpose
- `thread_alive`: will be `TRUE` if the thread is still alive, otherwise, it will become `FALSE`. 

## Functions
``` C#
private void GetArduino(); // Parsing Arduino input to Unity
                           // In this case, we're parsing 9 strings into Unity,
                           // And then convert them into type int:
// Parsing Var:
// R, G, B (from sliders, range from 0~256) 
// up, down, left, right (from joy stick): 1 stands for actively tilting to that direction, otherwise will be 0
// done_button, apply_button: 1 stands for actively pressing the button, otherwise will be 0
```