# Brief
The train movement controls train to switch window, stop at checkpoint, leave the checkpoint, move reverse.

# Dependency
- In the train prefab (local):Assets\Prefabs
- In the main game scene (global): on 'TrainMgr', below '=====Manager======' `Assets\Scripts\Train\TrainMgr.cs`
- In the main game scene: `Assets\Scripts\CheckPoints\BranchCheckPoint.cs`
# `Assets\Scripts\Train\TrainMgr.cs\Train`

-   ``` c#
    public class Train
    {
        public GameObject train { get; set; }
        public int index;
        public string id;
        PathFollower pathFollower = null;
        PathCreator pathCreator = null;
        public int curWindow = 0;
        public bool canMove = true;
        private DicFSM StateDic = new DicFSM();
        public StateMachine stateMachine { get; set; }

        public void ResetWindow();
        void InitializeStateMachine();
        public void StartPath();
        public void StopPathFollower();
        public void GetReachCheckPointEvent(int checkpoint);
        public void ClickDoneButton();
        public void ChooseWindow(int window);
    }
    ```

# `Assets\Scripts\Train\TrainMgr.cs`

    when `bMoving ` is false, the train is static

-   `[SerializeField] private List<GameObject> paths; `

    when `bReverse ` is true, the train moves from the endpoint to startpoint

-   `[SerializeField] private PathCreator pathCreator;`

    when `bReverse ` is true, the train's speed = -1 * `speed `

-   `[SerializeField] private GameObject trainPrefab;`

    If the path changes during the game, update the distance travelled so that the follower's position on the new path
    
    is as close as possible to its position on the old path

    When the train change the track, run `OnPathChanged() `

- `public void CreateNewTrain(int window, string id, int arrayIndex);`

- `public void CreateNewTrain(int window)`


# Checkpoints:
## checkpoints in 1, 2, 3 stations
![Alt text](https://user-images.githubusercontent.com/49530505/163628842-383d7643-0972-424b-817d-357734dce522.png "the last window")

Create transparent cube and attach `BranchCheckPoint.cs`

## checkpoints in simulation scenes
- Bridge

- Reset