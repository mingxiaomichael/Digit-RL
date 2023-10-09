# Week04 Oct.2 - Oct.8

## 1. Constructing Actor-Critic network:

**Actor-Critic:** Using stochastic prolicy gradient as Actor network, using DQN as Critic network.

**The most important problem:** The bipedal control is a **continuous** control, so it is a good solution to use **stochastic prolicy gradient** instead of policy gradient.

**Stochastic policy gradient:** The outputs of stochastic policy gradient include $\mu$ (the mean of normal distribution), and $\sigma^2$ (the standard variant of normal distribution), so we can sample an action from normal distribution $N(\mu, \sigma^2)$.

## 2. How to train Actor-Critic network?

**(1) Determine episode and step:** Reinforcement learning Agent learn from each episode, and try steps in each episode. The first try on bipedal control can start on keeping the position of the base, each episode is keeping the position of the base in 30 seconds, if falling down before 30 seconds, it will restart a new episode;

**(2) Reward function:** The reward function of keeping the base position is $reward = w_{base} * r_{base}$, where $r_{base} = exp(-||q - q_{old}||^2)$.

**(3) Update the parameters of Actor-Critic network:** 

```python
# Value network parameters update
q_target = reward_batch + self.gamma * T.max(q_next, dim=1)[0]
loss = self.Q_eval.loss(q_target, q_eval).to(self.Q_eval.device)
self.Q_eval.optimizer.zero_grad()
loss.backward()  # Compute the gradient of Loss with respect to the network parameters
self.Q_eval.optimizer.step()  # gradient descent, update parameters of Q_eval

# Policy network parameters update
# I am thinking about how to update parameters of policy network
# It will use Q_eval.
```
