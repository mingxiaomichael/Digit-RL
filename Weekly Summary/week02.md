# Week02 Sept.18 - Sept.24

## 1. From "digit-alip-controller" code, how to update observations:

Mainly on "step_controller() functions", the observation of Digit_Controller is updated by MuJoCo. In the condition of reference motion, we use MuJoCo as a tool of physical simulation, and Digit_Controller as a controller of deciding the next observation.

![image1](https://github.com/mingxiaomichael/Digit-RL/blob/main/Weekly%20Summary/_week02_step_controller.png)

## 2. ReferenceMotion Class:

I encapsulated "mujoco_test.py" into a class called ReferenceMotion.

The reason I need a encapsulation is the input of the RL framework includes the data from reference motion, and the convergence of RL will be better and faster when we have the reference motion input.

## 3. Control framework (RL + PD control):

![image2](https://github.com/mingxiaomichael/Digit-RL/blob/main/Weekly%20Summary/_week02_control%20framework.png)

## 4. How to extract q_motors_des and qdot_motors_des:

Extract q_motors_des and qdot_motors_des in Walking_Update_ function in Digit_Controller.cpp as additional outputs.

More details: Firstly, create list array "array" in Python, call C++ Walking_Update_ function by using pybind. Secondly, assigning "array" to C++ matrix struct, then computing in C++. Thirdly, assigning the result after computing to "arrary", then back to Python.