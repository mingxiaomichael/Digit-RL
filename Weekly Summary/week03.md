# Week03 Sept.25 - Oct.1

## 1. Extracting "q_motors_des" and "qdot_motors_des":

**Our goal:** Call C++ attributes and functions in Python, includes "q_motors_des" and "qdot_motors_des".

**Why do I extract "q_motors_des" and "qdot_motors_des":** Use them as the inputs of RL framework.

**How to extract "q_motors_des" and "qdot_motors_des":** 

(1) Firstly, actual argument in Python, I use numpy array, here are three variables, plus a "stance_leg_output" which controls whether or not switch stance leg.

```Python
# Python actual arguments
q_motors_des = np.zeros(self.NUM_MOTORS)
qdot_motors_des = np.zeros(self.NUM_MOTORS)
stance_leg_output = np.zeros(1)
```

(2) Secondly, using Eigen in pybind11 to implement "Python <--> C++" transition. Eigen is C++ header-based library for dense and sparse linear algebra. Due to its popularity and widespread adoption, pybind11 provides transparent conversion and limited mapping support between Eigen and Scientific Python linear algebra data types. Here are some necessary "includes" in C++:

In pybind file:
```
#include <pybind11/pybind11.h>
#include <pybind11/numpy.h>
#include <pybind11/eigen.h>
```

In C++ file:
```
#include "eigen340/Eigen/Dense"
using namespace Eigen;
```

(3) Lastly, Adding those actual arguments into C++ functions, also need to upgrade head file.

```
void Digit_Controller::Update_(llapi_command_t &command_update,
                               Ref<Eigen::MatrixXd> q_motors_des,
                               Ref<Eigen::MatrixXd> qdot_motors_des,
                               Ref<Eigen::MatrixXd> stance_leg_output,
                               const llapi_observation_t &observation_update,
                               const llapi_limits_t *const limits_update)
```

## 2. Create "Controller" class:

**Our future goal:** Operating RL-Control framework.

**Why do I create "Controller" class:** It will be convenient to run code, if encapsulating code into "Reference" class, "Controller" class and "RL" class.

**How to encapsulate control code into "Controller class":** Transfering "PD Control" part of C++ code to Python "Controller" class. The two most important things: (1)making "stance_leg_" match in PD Control code; (2) transfering quaternion(w x y z) to euler (roll potch yaw).

Some details in PD Control: Stance compensation; q_motors (I use "observation.motor.position = d.qpos" instead to use "d.qpos", where d.qpos is get by the nameid in mujoco); gc.update() do not update observation.motor.position.




## 3. RL framework

**Method:** Actor-Critic with PPO optimization;

![image1](https://github.com/mingxiaomichael/Digit-RL/blob/main/Weekly%20Summary/_week02_step_controller.png)

**Input: (vanilla)** [12 + 12], consider only the legs, only the position. 12 q_motors and 12 q_motors_des from reference motion.

**Output: (vanilla)** [12 + 2], 12 delta q_motors_des from Actor-Critic network, 2 stance compensations, "u_knee_comp_" and "u_hiproll_comp_".