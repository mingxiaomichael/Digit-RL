# Week01 Sept.11 - Sept.17

## 1. MoJoCo tutorial

**mjModel**: contains the model description, i.e., all quantities which do not change over time.

**mjData**: contains the state and quantities that depend on it. The state is made up of time, generalized positions and generalized velocities.

**Important code and function**: https://github.com/mingxiaomichael/Reinforcement-Learning/tree/main/MuJoCo/Tutorial

```python
# Create model and data for MuJoCo simulation
model = mujoco.MjModel.from_xml_string(xml)
data = mujoco.MjData(model)

# Make renderer, render and show the pixels
renderer = mujoco.Renderer(model)

# Start simulation (need loop)
mujoco.mj_step(model, data)
renderer.update_scene(data)
renderer.render()

# enable joint visualization option:
scene_option = mujoco.MjvOption()
scene_option.flags[mujoco.mjtVisFlag.mjVIS_JOINT] = True
```

## 2. Introduction to "digit-alip-controller" code:

**How to control bipedal robot (simulation, not real)**: 

Using "mjData" (in MuJoCo) to simulate and control bipedal, then finishing planning (making decision to next state) by "Digit_controller" (in C++ code).

mjData (or observation) --> action --> next mjData (or observation)

**Why using pybind**: 

There are differences of data structures and types between MuJoCo state and robot company state.
Robot company: C++ structure, observation.
MuJoCo: Python MuJoCo, mjData.


**Components of robot**: 

30 components, among these 30 components, there are 20 motors: 
Upper body (8 motors): left-shoulder-roll, left-shoulder-pitch, left-shoulder-yaw, left-elbow, right-shoulder-roll, right-shoulder-pitch, right-shoulder-yaw, right-elbow.
Lower body (12 motors + 10 joints): left-hip-roll, left-hip-yaw, left-hip-pitch, left-knee, left-toe-A, left-toe-B, right-hip-roll, right-hip-yaw, right-hip-pitch, right-knee, right-toe-A, right-toe-B, left-shin, left-tarsus, left-toe-pitch, left-toe-roll, left-heel-spring, right-shin, right-tarsus, right-toe-pitch, right-toe-roll, right-heel-spring.

**Keywords in code**: 

(1) Digit_controller:

observation: base, imu, motor, joint.
base: Includes position, orientation, linear_velocity, angular_velocity.
imu: be used to compute angular_velocity, linear_acceleration, magnetic_field.

command

(2) mjData:

d_qpos: Initializing by "init_pos" with len = 61.
d_qvel

(3) step_controller(gc, m, d, command, observation):

```python
def step_controller(
    gc: dc.Digit_Controller,
    m: mujoco.MjModel,
    d: mujoco.MjData,
    command: dc.Command,
    observation: dc.Observation,
):
```