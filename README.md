# SemP-NBV

## Project page for: SemP-NBV: Semantic Predictive Next-Best-View for Autonomous Plant 3D Reconstruction (IROS 2025)

[Paper](IROS2025_BenchBot.pdf)

### TODO: More to update

### Subprojects (see instructions)
* (EXE) For packaged UE5 simulation application, see [Globus download (~5GB)](https://app.globus.org/file-manager?origin_id=a8e2b46a-9f68-4752-ba44-812d5f310faa&origin_path=%2F)
* (Editor) For UE5 project ***without*** the asset models (for blueprint/code viewing), see [https://github.com/NCSU-BAE-ARLab/AgriRoboSimUE5/tree/dev/AgriRoboSim2](https://github.com/NCSU-BAE-ARLab/AgriRoboSimUE5/tree/dev/AgriRoboSim2)
* (ROS2) For ROS2-Humble project including the SemP-NBV planner (`benchbot_xarm6_cpp` and `pointr_predict` projects), see [https://github.com/XingjianL/UE5Sim_colcon_ws/tree/SemP-NBV](https://github.com/XingjianL/UE5Sim_colcon_ws/tree/SemP-NBV)
* AdaPoinTr checkpoint: [https://drive.google.com/drive/folders/1vt6djVtNlHejVu2klNQVRsKBoTQDHJMv?usp=sharing](https://drive.google.com/drive/folders/1vt6djVtNlHejVu2klNQVRsKBoTQDHJMv?usp=sharing)
### Instructions
* Requirements: GPU better than RTX 3060, Windows 11 with WSL Ubuntu 22.04 (Simulation works in Windows 11, ROS2 works in WSL)
#### (Simulator EXE) (recommended)
1. Download the `(EXE) packaged UE5 simulation application` in Windows 11 and unzip (log in with Google if asked)
2. Run `AgriRobotSim.exe` (full screen recommended for proper GUI) and select `xArm6` map in the top left dropdown
3. Press `Map Menu` after switching to the level. There are three tabs: `Robot Settings`, `Environment Settings`, and `TODO` (see simulation options)
4. Change the dropdown in `Plant Collection`, check the boxes for Plant and Variant, then click the `Randomize Plant` button.
5. The number box next to the button is the randomization seed.
6. Click the `Arm Joint Menu` in the top left, it will show 6 radial sliders and 3 row sliders for changing and visualizing the robot states. The `xArm6 Capture Image` at the bottom can be pressed with a file path to the saved images directory.
#### (Editor) (not recommended unless you want to see the blueprint logic)
1. Download Unreal Engine 5.3, clone the repo `(Editor)`, and try to open it with the `.uproject`
2. The project will NOT have any assets on GitHub due to licensing (mostly Quixel assets).
3. The blueprints should still work, but cannot compile due to this issue.
#### (ROS2) 
1. Clone the `(ROS2)` repo and follow the `environment_configuration.txt` instructions to set up the environment.
2. Change all the absolute filepaths (marked by `lxianglabxing`) in the project code to your own.
#### (Combined)
1. Run `ros2 launch tomato_xarm6 upside_down_xarm6_moveit.launch.py` to start the ROS and ROSBridge for Windows-WSL communication.
2. Start the (Simulator EXE) to confirm connections made.
3. Start `python pointr_predict.py` to spin the ROS node for AdaPoinTr.
4. Run `ros2 run benchbot_xarm6_cpp benchbot_xarm6_cpp` or `iros_test_auto.sh` (see `benchbot_xarm6_cpp/src/benchbot_xarm6_cpp.cpp` for args)

### Simulation Options
#### Map Menu
* `Robot Settings` - mostly for another project, not much to see for this one, you can try checking the box `Bind View to Arm` so the player view sticks to the robot camera.
* `Environment Settings` - adjusting the plants in the environment
    * `Plant Collection` - adjusting the set of plant models available for randomization
        * (Single/Multi) - changes the segmentation appearance for plant parts semantics or not
        * (Train/Test) - the set of plant models used for AdaPoinTr training or for testing the NBV framework.
    * `Plant` arrows - only used for one type of plant (can have multiple variants)
    * `Plant` checkbox - checked for randomizing the plant types
    * `Variant` checkbox - checked for randomizing the plant variants (both checked means both randomized)
    * `Segment` dropdown - not used in our experiments (only have options for hydrangea), for testing purposes
    * `Randomize Plant` button - click to randomize after checking the boxes, the number next to the buttom is the seed
    * `Raytrace Samples` slider - not used, was checking the image quality for hardware raytracing, does nothing here.
* `(TODO)` - was used for PixelStreaming plugin for remote visualizations of the simulation, none of the options will work here.
#### Stats Menu
* `FPS` - shows FPS top left
* `Unit` - more detailed stats for the simulation
* `Others` - only visualized in (Editor)
* `VSync` - limits the FPS to display
* `Maximum` - no FPS limit
* `60 FPS` - limits FPS to 60
* `30 FPS` - limits FPS to 30
#### Arm Joint Menu
* `xArm6` - 6 radial sliders ($-2 \pi$, $2\pi$) for controlling the joints in the robot arm, no collisions. Note: the name in the circle may change after running (ROS2)
* `BenchBot` - 3 row sliders for adjusting the BenchBot platform.
* `xArm Capture Image` - saves the image pairs (left, right RGB and depth, and left segmentation) to disk, preview can be seen in the environment.
