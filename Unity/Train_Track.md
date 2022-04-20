# Brief
@Author: Muru Chen

The camera controller controls cameras for the first, second and third station camera, as well as the cameras in the final simulation scenes. It uses several cameras which ouput to render texture.

# Dependency
- In the main game scene (global): below '=====Road======' `Assets\pathCreator\Scripts\PathFollower.cs`
- In the main game scene (global): on 'TrainMgr', below '=====Manager======' `Assets\Scripts\Train\TrainMgr.cs`

# `TrainGearController.cs`

-   `public bool bMoving { get; set; }`

    when `bMoving ` is false, the train is static

-   `public bool bReverse { get; set; } `

    when `bReverse ` is true, the train moves from the endpoint to startpoint

-   `public float speed = 5;`

    when `bReverse ` is true, the train's speed = -1 * `speed `

-   `public void OnPathChanged() `

    If the path changes during the game, update the distance travelled so that the follower's position on the new path
    
    is as close as possible to its position on the old path

    When the train change the track, run `OnPathChanged() `


# `TrainMgr.cs`

-   `public void SetNewPath(GameObject train, bool isReverse, int window)`

    Call this function when the train is switching tracks.

# Create New Track:
![Alt text](https://user-images.githubusercontent.com/49530505/163629604-67efb9c1-d059-411e-8baf-b946ae81d10b.png "track")

A new track contains startPoint, endPoint, midCheckPoint and Road Mesh Holder (optional)

# Path Creator Reference

https://github.com/SebLague/Path-Creator

# Scene Controls:
## Moving points:
![Alt text](https://user-images.githubusercontent.com/49530505/164095069-00d158da-e8c4-417d-b308-ee6a62ddf69a.png "move points")

![Alt text](https://user-images.githubusercontent.com/49530505/164096375-4c9d3ac2-2876-42f1-9624-da618d6f9bf0.png "move points")

Left-click and drag to move the points around. If you click on a point without
dragging, it will turn into a move tool with arrows so that you can move along a
single axis.

## Adding and Inserting points:
Shift-left-click to add new anchor points to the end of the path (hold the ​ctrl ​key
to add to the start of the path instead). To insert a point, shift-left-click over an
existing segment of the path.

## Deleting points:
Ctrl-click over an anchor point to delete it.

# Bézier Path Options:
## Control Mode:
Aligned:​ controls stay in a straight line around their anchor point.

Mirrored:​ controls stay in a straight, equidistant line around their anchor point.
Free:​ no constraints.

Automatic:​ controls are positioned automatically to try make the path smooth.

# Create train track mesh holder
![Alt text](https://user-images.githubusercontent.com/49530505/164102493-ca4b709a-9e5c-4195-a138-6462e9667d60.png "move points")

- Attach `RoadMeshCreator` on Road Creator

- Add Road Material and Underside Material

- Click Manual Update