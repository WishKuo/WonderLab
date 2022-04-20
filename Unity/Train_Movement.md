# Brief
@Author: Muru Chen

The train movement controls train to switch window, stop at checkpoint, leave the checkpoint, move reverse.

# Dependency
- In the train prefab (local):Assets\Prefabs
- In the main game scene (global): on 'TrainMgr', below '=====Manager======' `Assets\Scripts\Train\TrainMgr.cs`
- In the main game scene: `Assets\Scripts\CheckPoints\BranchCheckPoint.cs`
# `Assets\Scripts\Train\TrainMgr.cs\Train`

-   ``` c#
    public class Train
    {
        // The train game object with scripts attached
        public GameObject train { get; set; }

        // The index in the train dictionary in TrainMgr
        public int index;

        // The id that is given by RFID card
        public string id;

        // The script on corresponding path
        PathFollower pathFollower = null;

        // The script on corresponding path
        PathCreator pathCreator = null;

        // current station: 1, 2, 3, 4
        public int curWindow = 0;
        
        // when the player hasn't pressed done, the train can't move.
        public bool canMove = true;

        // The fsm dictionary, containing four states
        private DicFSM StateDic = new DicFSM();

        // FSM
        public StateMachine stateMachine { get; set; }

        // Back to the first station
        public void ResetWindow();

        // Create four states for each station
        void InitializeStateMachine();

        // Start moving along the track
        public void StartPath();

        // Stop the train movement
        public void StopPathFollower();

        // When the train reach the middle of the railway, send message to state and stop the train
        public void GetReachCheckPointEvent(int checkpoint);

        // Train leave the station.
        public void ClickDoneButton();

        // Check whether there is a train already at that station. Scan RFID card on the station, and the train will move to that station.
        public void ChooseWindow(int window);
    }
    ```

# `Assets\Scripts\Train\TrainMgr.cs`


-   `[SerializeField] private List<GameObject> paths; `

    Stores the track in 1, 2, 3 stations.


-   `[SerializeField] private GameObject trainPrefab;`

    Used for instantiating new trains in the scene

- `public void CreateNewTrain(int window, string id, int arrayIndex);`

- `public void CreateNewTrain(int window)`

    When the RFID card scan the window but there is no train for this card, the system will generate a new train showing up at the station.

# Checkpoints:
## checkpoints in 1, 2, 3 stations
![Alt text](https://user-images.githubusercontent.com/49530505/163628842-383d7643-0972-424b-817d-357734dce522.png "the last window")

Create transparent cube and attach `BranchCheckPoint.cs`

## checkpoints in simulation scenes

![Alt text](https://user-images.githubusercontent.com/49530505/164113406-2550cb70-448f-4ca8-9054-8207601c34a2.png "bridge")

- Bridge: When the train passes the bridge, the bridge will play animation. Add the bridge model to the script.
<br></br>

![Alt text](https://user-images.githubusercontent.com/49530505/164112938-4d1782d9-d169-40b3-b892-c9bc827fc1b2.png "reset check point")

- Reset: When the train passes the reset checkpoint, the camera in the simulation scene will reset to the first one. Attach `ResetCheckPoint`