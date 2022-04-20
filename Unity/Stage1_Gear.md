# Brief Introduction
Author: Wish Kuo

The following scripts are related to the Gear (1st) Station.
Each function is fully implemented, and is described below. For detailed information please reference to the comments in the scripts.

# Dependency (Where should put the scripts)
- On the train (local): `Assets/Scripts/Train/TrainGearController.cs`
- In the main game scene (global):
    - Link Arduino to Unity: `Assets/Scripts/Train/GearButtonController.cs`

# `TrainGearController.cs`
This script should be added to the prefab of the train.

It receives the input from either physical inputs (Buttons) or a keyboard, and then makes the action.

## Variables 
### Configuration
- `private int length_total_max`: maximum length of the total gears
- `private float initial_trainSpeed`: default speed for the train
- `private float RotationSpeed`: the gear's rotation speed

### Needed to assign
- `private GearButtonController ButtonInput`: will automatically assign `GearButtonController.cs` in `Start()`
- `private GameObject train`: assign the train prefab you want to instantiate
- `private GameObject default_gear_start`: assign the default gear prefab (it will automatically attach on the train prefab at the positions of `ref_points`)
- `private List<GameObject> gear_models`: assign 3 gear prefabs from small to large
- `private List<GameObject> ref_points`: assign the positions that you want the gears to appear
- `public Slider speedSlider`: a slider indicates the current speed of the train
- `public Slider torqueSlider`: a slider indicates the current torque of the train


### For Debug purpose
- `private bool keyboardInput`: check (set to true) on the inspector if no physical inputs are connected
- `private int indicator`: indicates current state of the ref_points
- `private int last_gear_length`: store the last gear length in order to implement the undo function

## Functions
``` C#
/// <param name="gear_index">the size of the gear to be added</param>
public void AddGear(int gear_index); // Add a gear to the train
```

``` C#
public void ClearCurrentState(); // Clear all gears on the train
```

``` C#
public void UndoGear(); // Back to the last state
```

``` C#
/// <param name="init_speed">the initial speed of the train</param>
/// <param name="gearLengthOnTrain">a stack that stores all the current length of the gears</param>
public float CalculateTrainSpeed(float init_speed, Stack<int> gearLengthOnTrain); // Calculate the current speed of the train according to the gears installed
```

# `GearButtonController.cs`
This script should be added in the main game scene which has to be active at the very beginning of the game.
It will connect to the Arduino through [serial communication](https://create.arduino.cc/projecthub/raisingawesome/unity-game-engine-and-arduino-serial-communication-12fdd5).
Also, in order to avoid the lagging issue while plug in multiple Arduino boards, this code implements multi-threading.

This script is required to pass the physical input information to `TrainGearController.cpp`.

Note: DO NOT DELETE `void OnDisable()` and `void OnApplicationQuit()`, otherwise the program will get stuck and may crash the game.

## Variables 
### Configuration
- `SerialPort sp = new SerialPort("COM7", 9600);`: check which port is plug in using the Arduino IDE.

### For Debug purpose
- `thread_alive`: will be `TRUE` if the thread is still alive, otherwise, it will become `FALSE`. 

## Functions
``` C#
private void GetArduino(); // Parsing Arduino input to Unity
                           // In this case, we're parsing 6 strings into Unity,
                           // And then convert them into type int:
// Parsing Var: 
// small_gear_button, mid_gear_button, large_gear_button
// done_button, undo_button, clear_button
// 1 stands for actively pressing the button, otherwise will be 0
```




