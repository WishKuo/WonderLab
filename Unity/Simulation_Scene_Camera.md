# Brief
@Author: Muru Chen

The camera controller controls cameras for the first, second and third station camera, as well as the cameras in the final simulation scenes. It uses several cameras which ouput to render texture.

# Dependency
- In the main game scene (local): 'SampleScene\FinalSimulation\ChangeCameraBlock' `Assets\Scripts\CameraSwitchCheckPointCollision.cs`
- In the main game scene (global): on 'LastStationCameraController', below '=====Manager======' `Assets\Scripts\LastStationCameraController.cs`


# `LastStationCameraController.cs`

-   `public GameObject lastStation;`
![Alt text](https://user-images.githubusercontent.com/49530505/163618768-d080372c-9fe8-40ab-9f3e-e2fcc8de5e19.png "the last window")

    The RawImage that display the rendertexture of the simulation camera.
    <br></br>

-   `public List<RenderTexture> windowTextures;`

    Store the renderer texture for the simulation scene cameras.

-   `public void SetTextureByIndex(int index)`

    Set renderer texture on the camera for last station.

# `CameraSwitchCheckPointCollision.cs`

-   `public int cameraIndex;`

    The index of camera for `windowTextures` in `LastStationCameraController`.

-   `void OnTriggerEnter(Collider other)`

    when the camera checkpoint block collides the train, the camera in the simulation scene will switch to `LastStationCameraController.Instance.SetTextureByIndex(cameraIndex)`.


# Set Camera for different windows:
![Alt text](https://user-images.githubusercontent.com/49530505/163623741-7e518fad-196c-40df-982a-6f8bc9141c3a.png "the last window")

Create a new camera and new render texture. Set the render texture in camera's output.
<br></br>

![Alt text](https://user-images.githubusercontent.com/49530505/163622137-a74b015f-4364-45ef-a554-2f87312a4e63.png "the last window")

Set the render texture to the Raw Image in Canvas

# Switch camera in simulation scene:
![Alt text](https://user-images.githubusercontent.com/49530505/164083517-40e00c44-1a57-46c7-90da-0f36e4a0ad35.png "camera block")
- Add switch camera check point blocks in 'SampleScene\FinalSimulation\ChangeCameraBlock' 
- Attach `CameraSwitchCheckPointCollision` on the block.   
<br></br>
![Alt text](https://user-images.githubusercontent.com/49530505/164086398-b172f4f1-e433-46b9-91e7-af1f742ab732.png "public list")
- Add the rendertexture in the public list `windowTextures`
- Set the index of render texture in the public list to public int `cameraIndex` in `CameraSwitchCheckPointCollision`
