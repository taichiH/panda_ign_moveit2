# panda_description

URDF and SDF description of Franka Emika Panda.

<p align="center" float="middle">
  <img width="100.0%" src="panda/thumbnails/2.png" alt="Visualisation of panda"/>
</p>

## Instructions

### URDF

For URDF, [panda.urdf.xacro](./urdf/panda.urdf.xacro) is the primary descriptor that includes all other xacros and creates a model based on the passed arguments. To generate URDF out of xacro, you can use the included [xacro2urdf.bash](./scripts/xacro2urdf.bash) script and modify its arguments as needed. Once executed, [panda.urdf](./urdf/panda.urdf) will automatically be replaced. Alternatively, `xacro panda.urdf.xacro name:="panda" <arg_i>:=<val_i> ...` can be executed directly, e.g. this is preferred within any launch script.

In order to visualise URDF with RViz2, included [view.launch.py](./launch/view.launch.py) script can be used.

```bash
ros2 launch panda_description view.launch.py
```

### SDF

For SDF, please use the included [xacro2sdf.bash](./scripts/xacro2sdf.bash) script with the desired arguments. This script makes sure that a correct relative path is used to locate all assets. In order to make the model discoverable within the context of Ignition Gazebo, please extend the `IGN_GAZEBO_RESOURCE_PATH` environment variable.

```bash
export IGN_GAZEBO_RESOURCE_PATH=${REPO_DIR}/panda_description${IGN_GAZEBO_RESOURCE_PATH:+:${IGN_GAZEBO_RESOURCE_PATH}}
```

To visualise SDF with Ignition Gazebo, included [view_ign.launch.py](./launch/view_ign.launch.py) script can be used.

```bash
ros2 launch panda_description view_ign.launch.py
```

#### Ignition Fuel

If you do not require URDF and other resources from this repository, the default model (without `ros2_control`) can also be included directly from [Ignition Fuel](https://app.ignitionrobotics.org/AndrejOrsula/fuel/models/panda) if you do not require the URDF description.

```xml
<include>
    <uri>https://fuel.ignitionrobotics.org/1.0/AndrejOrsula/models/panda</uri>
</include>
```

## Directory Structure

The following directory structure is utilised for this package because it provides compatibility with Ignition Gazebo, including [Ignition Fuel](https://app.ignitionrobotics.org).

```bash
.
├── config/initial_joint_positions.yaml # List of initial joint positions for fake and simulated ROS 2 control
├── launch/                             # [dir] ROS 2 launch scripts
    ├── view.launch.py                  # Launch script for visualising URDF with RViz2
    └── view_ign.launch.py              # Launch script for visualising SDF with Ignition Gazebo
├── panda/                              # [dir] Model directory compatible with Ignition Fuel
    ├── meshes/                         # [dir] Meshes for both URDF and SDF
        ├── **/collision/*.stl          # STL meshes for collision geometry
        └── **/visual/*.dae             # COLLADA meshes for visuals
    ├── thumbnails/                     # [dir] Thumbnails for Ignition Fuel
    ├── model.config                    # Model meta data
    └── model.sdf                       # SDF (generated from URDF)
├── rviz/view.rviz                      # RViz2 config for visualising URDF
├── scripts/                            # [dir] Additional useful scripts
├── urdf/                               # [dir] URDF description (xacros)
    ├── panda_arm.xacro                 # Xacro for Franka Emika Panda arm
    ├── panda_gripper.xacro             # Xacro for Franka Emika Panda gripper
    ├── panda_inertial.xacro            # Macro for inertial properties of Franka Emika Panda
    ├── panda_utils.Macro               # Macros for general boilerplate
    ├── panda.gazebo                    # Macros that add Ignition Gazebo plugins for Franka Emika Panda
    ├── panda.ros2_control              # Macros that add ros2 control for Franka Emika Panda
    ├── panda.urdf                      # URDF (generated from panda.urdf.xacro)
    └── panda.urdf.xacro                # High-level xacro for Franka Emika Panda
├── CMakeLists.txt                      # Colcon-enabled CMake recipe
└── package.xml                         # ROS 2 package metadata
```

## Disclaimer

URDF xacros are originally based on [frankaemika/franka_ros](https://github.com/frankaemika/franka_ros/tree/develop/franka_description). These files were modified to fit the purpose of this repository.
