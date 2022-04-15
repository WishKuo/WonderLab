# Dependency
- In the main game scene (global): below '=====Road======' `Assets\pathCreator\Scripts\PathFollower.cs`
- In the main game scene (global): below '=====Manager======' `Assets\pathCreator\Scripts\PathFollower.cs`

# `TrainGearController.cpp`
    ```
    // when bMoving is false, the train is static
    public bool bMoving { get; set; } 

    // when bReverse is true, the train moves from the endpoint to startpoint
    public bool bReverse { get; set; } 

    // when bReverse is true, the train's speed = -1 * speed
    public float speed = 5;

    // If the path changes during the game, update the distance travelled so that the follower's position on the new path
    // is as close as possible to its position on the old path
    public void OnPathChanged() 
    ```
