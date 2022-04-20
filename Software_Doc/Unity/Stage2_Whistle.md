# Brief Introduction
Author: Wish Kuo

The following scripts are related to the Whistle (2nd) Station.
Each function is fully implemented, and is described below. For detailed information please reference to the comments in the scripts.

Regarding to sound, this Unity project is using [FMOD](https://www.fmod.com/unity).
Official document can be found [here](https://www.fmod.com/resources/documentation-api?version=2.02&page=welcome.html).

# Dependency (Where should put the scripts)
- On the train (local): `Assets/Scripts/Train/TrainWhistleRecord.cs`
- In the main game scene (global):
    - Link Midi keyboard to Unity: `Assets/Scripts/Train/WhistleController.cs`
    - Link Arduino to Unity: `Assets/Scripts/Train/Stage3Mgr.cs` (will rename after cleaning up the code)

# `TrainWhistleRecord.cs`
This script should be added to the prefab of the train.

It receives the input from either physical inputs or a keyboard, and then makes the action.

## Variables 
### Needed to assign
- `private WhistleController WhistleInput`: assign `WhistleController.cs` in `Start()`
- `private Stage3Mgr stage3mgr`: assign `Stage3Mgr.cs` in `Start()`
- `private List<GameObject> Instrument_model`: assign the 4 different whistles prefab you want to instantiate

### Current state
- `private List<string> whistle_path_list`: the saved whistle path will store here

### For Debug purpose
- `private bool keyboardInput`: check (set to true) on the inspector if no physical inputs are connected

## Functions
``` C#
public void SaveWhistleList(); // Copy the whistle path from WhistleController.cs
```

``` C#
public void ClearWhistleModel(); // Call it each time to avoid multiple
                                 // whistle models appear at the same time  
```

# `WhistleController.cs`
This script should be added in the main game scene which has to be active at the very beginning of the game.

It gets the input from the midi keyboard. We are using the package [MidiJack: MIDI input plugin for Unity](https://github.com/keijiro/MidiJack). Current project has already imported the package.

## Variables 
### Needed to assign
- `private List<string> DogAudioSourcePath`: assign FMOD event path related to Dog here
- `private List<string> PianoAudioSourcePath`: assign FMOD event path related to Piano here
- `private List<string> WhistleAudioSourcePath`: assign FMOD event path related to Whistle here
- `private List<string> BellAudioSourcePath`: assign FMOD event path related to Bell here

### Current state
- `public int currentInstrument`: indicate which instrument is on the train now
- `public bool startRecord`: indicate if it is recording the user's input
- `public bool doneRecord`: indicate if it is done recording the user's input
- `public List<string> PlayerAudioSourcePath`: the list stores the player's input while recording

### FMOD setting
- `private FMOD.Studio.EventInstance playEvent`: initialize FMOD event instance

### For Debug purpose
- `private bool keyboardInput`: check (set to true) on the inspector if no physical inputs are connected

## Get midi keyboard input method usage example
``` C#
bool keyPressed = false;
for (int key = 36; key <= 72; key++) // set the index to the range you want to include
{
    // Avoid repetitively trigger the action in the Updated loop
    keyPressed = keyPressed || MidiMaster.GetKeyDown(MidiChannel.All, key); 
    
    if (MidiMaster.GetKeyDown(MidiChannel.All, key))
    {
        Debug.LogError("Key Pressed with ID: " + key); // You can test the key ID with this line
    }
}
if (keyPressed)
{
    Debug.Log("Piano Key Pressed!");
}
```

## Functions
``` C#
/// <param name="path">a string indicates to the FMOD event path</param>
public void PlayKeySound(string path); // Play the sound at the path
```

``` C#
public void StopPlayingKeySound(); // Stop playing all the sound in the scene
```

``` C#
public void StartPlayingAudio(); // Start playing the user's record
```

``` C#
IEnumerator PlayPlayerAudio() // Define IEnumerator for playing the user's record
{
    foreach (string path in PlayerAudioSourcePath)
    {
        // Stop for 0.5 second if a rest is detected
        if (path == "REST") yield return new WaitForSeconds(0.5f);
        else
        {
            // Settings for playing a FMOD event
            playEvent = FMODUnity.RuntimeManager.CreateInstance(path);
            playEvent.set3DAttributes(FMODUnity.RuntimeUtils.To3DAttributes(this.gameObject));
            playEvent.start();
        }
        yield return new WaitForSeconds(0.5f); // Stop for 0.5 after each sound clip
    }
}
```

``` C#
/// <param name="s">a string indicates to the FMOD event path</param>
public void AddToRecordList(string s); // Store s into the list(PlayerAudioSourcePath)
```

# `Stage3Mgr.cs`
This script should be added in the main game scene which has to be active at the very beginning of the game.
It will connect to the Arduino through [serial communication](https://create.arduino.cc/projecthub/raisingawesome/unity-game-engine-and-arduino-serial-communication-12fdd5).
Also, in order to avoid the lagging issue while plug in multiple Arduino boards, this code implements multi-threading.

This script is required to pass the physical input information to `TrainWhistleRecord.cpp`.

Note: DO NOT DELETE `void OnDisable()` and `void OnApplicationQuit()`, otherwise the program will get stuck and may crash the game.

## Variables 
### Configuration
- `SerialPort sp = new SerialPort("COM5", 9600);`: check which port is plug in using the Arduino IDE.

### For Debug purpose
- `thread_alive`: will be `TRUE` if the thread is still alive, otherwise, it will become `FALSE`. 

## Functions
``` C#
private void GetArduino(); // Parsing Arduino input to Unity
                           // In this case, we're parsing 3 strings into Unity,
                           // And then convert them into type int:
// Parsing Var: 
// done_button, play_button, record_button
// 1 stands for actively pressing the button, otherwise will be 0
```