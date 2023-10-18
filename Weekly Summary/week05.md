# Week04 Oct.2 - Oct.8

## 1. Encapsulating classes and functions:

**Reference class:** Load MuJoCo data and C++ digit controller.

    ReferenceMotion: digit-alip-controller/example/RL_Reference.py
    
    Functions:
    1. initialize()  # initialize the state of bipedal
    2. step_controller(gc, m, d, command, observation):
        """
        Update q_motors_des, qdot_motors_des and stance in C++ digit controller.
        parameters: gc: dc.Digit_Controller,
                    m: mujoco.MjModel,
                    d: mujoco.MjData,
                    command: dc.Command,
                    observation: dc.Observation,
        """
    3. command_update(observation)  # use controller class to implement PD control
    4. mujoco_update(m, d, command, observation)  # update MuJoCo data
    5. quaternion_to_euler(qw, qx, qy, qz)  # transfer quarternion to Euler
    

**Controller class:**

    PDController: digit-alip-controller/example/Controller.py

    Function:
    1. pd_control()

**RL class:** Actor-Critic network.

    Path: digit-alip-controller/example/RL_simple.py
    Classes:
    ValueNet(nn.Module)  # DQN for Critic network
    PolicyNet(nn.Module)  # Policy Gradient for Actor network
    ActorCriticAgent(object)  # Actor-Critic agent 
    
    # Initialize Critic and Actor network
    self.Critic = ValueNet(num_inputs, self.lr, 256, 128)
    self.Actor = PolicyNet(num_inputs, num_outputs, self.lr, 256, 128)

    Functions in ActorCriticAgent class:
    1. train(state, base_pre, base, q_motors_des, base_target, stance)
    2. reward_function(base, base_pre, base_target)  # compute reward of state
    3. update_target(steps_to_update)  # after "steps_to_update" steps, updating target Critic network

## 2. Episode training:

Since the training code of reinforcement learning needs MDP sequence which includes 4 important components, **state, action, reward, next-state**, so I finish encapsulate work above to easily achieve these 4 MDP components and implement episode training.

**In each episode:** At the beginning of each episode, use "initialize()" function to initialize or reset the bipedal. Then training for thousands of steps.

**In each step:** 

1. Get `q_motors`, `q_motors_des`. `q_motors[:12]` is used to be a **state**.
2. In `train()` function. Get **action** by Actor network.
3. Get **reward** by reward_function(base, base_pre, base_target);
4. `next_state = q_motors_des + action`, `next_state` is used to be **next-state**.
