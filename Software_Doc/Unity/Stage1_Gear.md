# Brief Introduction
The following scripts are related to the Gear Station.
Full functions are implemented. 

# Dependency (Where should put the scripts)
- On the train (local): `Assets/Scripts/Train/TrainGearController.cs`
- In the main game scene (global):
    - Link Arduino to Unity: 
        - w/o RFID: `Assets/Scripts/Train/GearButtonController.cs`
        - w RFID: 

# `TrainGearController.cpp`
This script should be added to the prefab of the train.

## Variables 
### Configuration

- `length_total_max`: maximum length of the total gears
- `initial_trainSpeed`: default speed for the train
- `RotationSpeed`: the gear's rotation speed

### Needed to assign
- `ButtonInput`: assign `GearButtonController.cs` in `Start()`
- `train`: assign the train prefab you want to instantiate
- `default_gear_start`: assign the default gear prefab (it will automatically attach on the train prefab at the positions of `ref_points`)
- `gear_models`: assign 3 gear prefabs from small to large
- `ref_points`: assign the positions that you want the gears to appear
- `speedSlider`: a slider indicates the current speed of the train
- `torqueSlider`: a slider indicates the current torque of the train


### For Debug purpose
- `keyboardInput`: check (set to true) if no physical inputs are connected
- `indicator`: indicates current state of the ref_points
- `last_gear_length`: store the last gear length in order to implement the undo function

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
Also, in order to avoid the lagging issue while plug in multiple Arduino boards, the code implements multi-thread.




