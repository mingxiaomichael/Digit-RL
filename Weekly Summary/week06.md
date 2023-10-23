# Week06 Oct.18 - Oct.22

## 1. Adding MuJoCo termination:

During the training process, the training process will be terminated because of exceptions from the MuJoCo.

Torque limits exception: the torque will exceed the limit during the training process.

```python
torque = (command[j][0] + command[j][2] * (command[j][1] - observation.motor.velocity[j]))
if torque > self.LIMITS.torque_limit[j]:
    self.MuJoCo_termination = True
    break
else:
    d.ctrl[act_id] = torque / m.actuator_gear[act_id, 0]
```

## 2. Research about how to address continuous action space problem:

There are two main popular methods: **stochastic policy** and **deterministic policy**.

**Stochastic policy: (off-policy)** sampling action from a normal distribution. **Pros:** better exploration. **Cons:** requires huge amount of training data.

**Deterministic policy: (on-policy)** selecting action in a deterministic policy $\mu$, adding noise to guarantee the degree of exploration. **Pros:** less training data, stable. **Cons:** less exploration.

Paper: 
1. https://arxiv.org/pdf/1509.02971.pdf?source=post_page--------------------------- 
Deterministic policy. It introduced the use of DPG (deterministic policy gradients) and actor-critic architectures to address the challenges of continuous control tasks.
2. http://proceedings.mlr.press/v70/chou17a/chou17a.pdf
Stochastic policy. It introduced a novel distribution called Beta distribution to alternate normal distribution, and address the limitation of action space (if the action space is from -1 to 1, the probability is map to [-1, 1], but the normal distribution will map to (-$\infty$， +$\infty$)).
