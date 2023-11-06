# Week08 Oct.30 - Nov.5

## 1. Using DDPG to learn how to walk forward:

**Task:** Learning to walking forward 0.2 meters when initial velocity `vx = 0.0`;

**Environment:** Reference motion of digit;

**Method:** DDPG;

**Details of reward function:** When torque limit termination, `qpos[2]` is too high or low, and `qpos[1]` is too left or right, give a penalty `reward = -100`; When `qpos[0]` reachs 0.2 meters, `reward = 500`; During walking, `reward = 0.1 * (np.exp(qpos[0] - 0.2) - 1)`, this reward is constantly negative before digit reachs 0.2 meters;

**Details of training:** Training 170 episodes. At `episode 0`, feeding a correct trajectory which `vx = 0.5` to digit. And then, training digit to walk forward 0.2 meters in `vx = 0.0` during 1~169 episodes (the action is with noise during 1~129 episodes, and without noise during 130~169 episodes).

**Problems:** 

![image1](https://github.com/mingxiaomichael/Digit-RL/blob/main/Weekly%20Summary/DDPG_170episodes.png)

From this score-episodes figure during training, there are two serious problems.
1. Oscillations of scores. At the end of training, about only 60% training episodes can walk forward 0.2 meters successfully, and the other training episodes will fall down or walk unstably;
2. Unstable average scores before 130 episodes. This problem equals to the oscillations of the noise of action (exploration noise, Ornstein-Uhlenbeck noise). The uneven oscillations in the Ornstein-Uhlenbeck process are determined by its stochastic nature, damping effect, and the influence of initial conditions.

**Solutions and optimization to problems above:** 
1. Get the initial `qpos` and `qvel` after stable walking, and use the stable initial to training;
2. Make the noise of action (exploration noise) smaller or try some other reasonable noise, such as Gaussian noise.

## 2. Stable initial:

Inputting the stable `qpos` and `qvel`, stable `qpos` and `qvel` are got after PD_control is stable. 

And digit can walk stably at the begining.