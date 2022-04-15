# Dependency
- In the main game scene (local): 'SampleScene\FinalSimulation\ChangeCameraBlock' `Assets\Scripts\CameraSwitchCheckPointCollision.cs`
- In the main game scene (global): on 'LastStationCameraController', below '=====Manager======' `Assets\Scripts\LastStationCameraController.cs`

# `CameraSwitchCheckPointCollision.cs`

-   `public bool bMoving { get; set; }`

    when `bMoving` is false, the train is static

-   `public bool bReverse { get; set; } `

    when `bReverse ` is true, the train moves from the endpoint to startpoint

-   `public float speed = 5;`

    when `bReverse ` is true, the train's speed = -1 * `speed `

-   `public void OnPathChanged()`

    If the path changes during the game, update the distance travelled so that the follower's position on the new path
    
    is as close as possible to its position on the old path

    When the train change the track, run `OnPathChanged() `


# `LastStationCameraController.cs`

-   `public GameObject lastStation;`
![Alt text](https://user-images.githubusercontent.com/49530505/163618768-d080372c-9fe8-40ab-9f3e-e2fcc8de5e19.png "the last window")

    The RawImage that display the rendertexture of the simulation camera.

-   `public List<RenderTexture> windowTextures;`

-   `public void SetTextureByIndex(int index)`

    when `bMoving ` is false, the train is static


# Set Camera for different windows:
![Alt text](https://user-images.githubusercontent.com/49530505/163623741-7e518fad-196c-40df-982a-6f8bc9141c3a.png "the last window")

Create a new camera and new render texture. Set the render texture in camera's output.

![Alt text](https://user-images.githubusercontent.com/49530505/163622137-a74b015f-4364-45ef-a554-2f87312a4e63.png "the last window")

Set the render texture to the Raw Image in Canvas

# Switch camera in simulation scene:
- Add switch camera check point blocks in 'SampleScene\FinalSimulation\ChangeCameraBlock' 
- Attach `CameraSwitchCheckPointCollision` on the block.
- Add the rendertexture in the public list
- Set the index of render texture in the public list to public int `cameraIndex` in `CameraSwitchCheckPointCollision`
