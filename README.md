# **Lunar Lander Reinforcement Learning — Deep Q-Network (DQN)**

*Project built as part of the Machine Learning Specialization (Coursera)*

This repository contains my implementation of a **Deep Q-Network (DQN)** agent trained to solve the classical **Lunar Lander** environment provided by OpenAI Gym.
The goal: teach an AI agent to **land a rocket safely** using reinforcement learning.

<p align="center">
  <img src="https://raw.githubusercontent.com/fakemonk1/Reinforcement-Learning-Lunar_Lander/master/images/3.gif" width="420">
</p>

## **What This Project Is About**

Reinforcement Learning (RL) is a branch of Machine Learning where an *agent* learns to take optimal actions in an *environment* through **trial, error, and reward signals**.

This project demonstrates:

* How RL can control complex physics-based systems
* How Deep Q-Networks (DQN) stabilize learning
* How replay buffers, target networks, and ε-greedy exploration are used
* How neural networks approximate Q-values

This is one of the most popular benchmark tasks in RL and a great introduction to control-based learning.

# **The Lunar Lander Environment**

OpenAI Gym’s **LunarLander-v2** consists of landing a spacecraft on a designated pad.

### **State Space (8 continuous variables)**

* X & Y position
* X & Y velocity
* Angle & angular velocity
* Left leg contact
* Right leg contact

### **Action Space (4 discrete actions)**

| Action | Meaning           |
| ------ | ----------------- |
| 0      | Do nothing        |
| 1      | Fire left engine  |
| 2      | Fire main engine  |
| 3      | Fire right engine |

### **Reward Structure**

* +100 to +140 → successful landing
* -100 → crash
* Small negative rewards → fuel usage
* Shaped rewards → staying upright & centered

The agent must learn to land **smoothly and efficiently**.

# **Deep Q-Network (DQN) Approach**

The DQN algorithm was chosen because it:

* Handles large state spaces
* Uses deep neural networks for function approximation
* Stabilizes training through **experience replay** and **target networks**
* Supports ε-greedy exploration for balancing curiosity vs. exploitation

### **High-Level Algorithm**

```
Initialize replay buffer R
Initialize neural network Q with random weights
For each episode:
    Observe state s
    Choose action a using ε-greedy strategy
    Execute action → receive reward r and next state s'
    Save transition (s, a, r, s') into memory
    Sample random mini-batches from memory
    Compute Bellman target:
        If terminal: target = r
        Else: target = r + γ * max Q(s')
    Train Q-network using MSE loss
    Update ε (decay over time)
Repeat until convergence
```

---

# **Neural Network Architecture**

* **Input Layer:** 512 units (ReLU), takes 8-dimensional state
* **Hidden Layer:** 256 units (ReLU)
* **Output Layer:** 4 units representing Q-values for each action

This architecture balances **expressiveness** and **training stability**.

---

# **Hyperparameters**

| Parameter          | Value   |
| ------------------ | ------- |
| Learning Rate      | 0.001   |
| Gamma (γ)          | 0.99    |
| Replay Buffer Size | 500,000 |
| Batch Size         | 64      |
| Epsilon Decay      | 0.995   |
| Min Epsilon        | 0.01    |
| Max Episodes       | 2000    |

**Early stopping** was implemented once the moving average of rewards exceeded **200 over 100 episodes**.

---

# **Training Results**

### Learning Curve (Training Rewards)

<p align="center">
  <img src="https://raw.githubusercontent.com/fakemonk1/Reinforcement-Learning-Lunar_Lander/master/images/Figure_1_Reward%20for%20each%20training%20episode.png" width="600">
</p>

The agent:

* Starts with negative rewards
* Learns stable control after ~300 episodes
* Exceeds the success threshold around episode ~500
* Achieves consistent safe landings

### **Testing Performance**

<p align="center">
  <img src="https://raw.githubusercontent.com/fakemonk1/Reinforcement-Learning-Lunar_Lander/master/images/Figure_2_Reward%20for%20each%20testing%20episode.png" width="600">
</p>

Across **100 evaluation episodes**:

* Average reward: **205**
* All episodes achieved positive rewards
* Stable, reproducible policy

# **Training Progress Visualization**

### Untrained Agent

<p align="center">
  <img src="https://raw.githubusercontent.com/fakemonk1/Reinforcement-Learning-Lunar_Lander/master/images/1.gif" width="380">
</p>

### Learning Around 300 Episodes

<p align="center">
  <img src="https://raw.githubusercontent.com/fakemonk1/Reinforcement-Learning-Lunar_Lander/master/images/2.gif" width="380">
</p>

### Fully Trained Agent (~600 Episodes)

<p align="center">
  <img src="https://raw.githubusercontent.com/fakemonk1/Reinforcement-Learning-Lunar_Lander/master/images/3.gif" width="380">
</p>
