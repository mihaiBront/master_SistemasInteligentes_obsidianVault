---
Subject: Machine Learning
Teacher: "@RamónAlbertoMollinedaCárdenas @JoséSalvadorSánchezGarreta"
Topic: 
date: 2024-12-17
tags:
  - SJK003-MachineLearning
---
# Formal definition

"*When it is not in our power to determine what is true, we ought to follow what is most probable*" (Descartes)

>[!example] Mobile robot:
>A mobile robot decides whether it should enter a new room to collect more trash or try to find its way back to its battery recharging station. It makes its decision based on the current charge level of its battery and how quickly it was able to find the recharger in the past
>
>Goal: Cleaning the house the most efficiently possible
>
>Agent: The robot
>
>State:
>- Battery percentage
>- Position
>- Distance from the charger
>- Percentage of task completeness
>
>*We will also need a cycle frequency for processing all this...*
>
>Actions: 
>- Nvaigate
>- Back to charger
>
>Environment:
>- The hpuse
>
>Sort-term rewards:
>- Trash recollection
>- Penalty for low battery
>
>Long-term rewards:
>- Perform the task efficiently

## Exploit vs Explore dilema

![[Pasted image 20241218154328.png]]
## Tabular solutions

-  **Q-learning**: 
	- *off policy* (updating $Q$ depends on the values of $\pi$ when learning)
	- *model-free* RL (no mathematical representation or definition of space)
- ***State-Action-Reward-State-Action*** (**SARSA**): 
	- *on policy* (updating $Q$ does not depend on the values of $\pi$ when learning)
	- *model-free RL*

# Key concepts
![[Pasted image 20241218153413.png]]

# Q-Learning and SARSA


- 𝑄-learning approximates the optimal action-value function 𝑄∗ that maximizes the expected value over all successive steps** 
$$𝑄: 𝑆 × 𝐴 → ℝ$$
- 𝑄-learning does not require an environment model (model-free) 
- 𝜋 (policy) depends on 𝑄 
	- Function that generates information of the status from the sensoric input about the environment
- 𝑄 depends on 𝜋 (𝜋 determines which state-action pair should be updated). 
- 𝑄’s update (learning) does not depend on 𝜋 (off policy). $\pi$ determines just the position of the array $Q$ which is going to be updated, but does not intervene in its new value_

## Bellman equation

![[Pasted image 20241218161051.png]]
Note:
- $\alpha$ should be small, because this is an estimation determined by values we don't yet know are true
- As you can see, this method is, indeed, *off-policy* 
- SARSA calculates the second part as:
  $$
  ...\alpha · (r_{t+1} + \gamma · Q(S_{t+1}, \Pi(S_{t+1})))
  $$
  Which is an on-policy method
## Meaning of the Q(s,a) table

The table will return the short term expected accumulated reward that that action will report from the state to the end.

## Meaning of the V(s) vector

The best action to take from the state S


# Limitations of tabular solutions

![[Pasted image 20241218163632.png]]

Comment:
- On generalization issues, having a big table makes some states rarely reached so the weights for them are not frequently updated

>[!example]
>![[Pasted image 20241218163928.png]]
>
>State space (500 rows):
>- (taxi_x, taxi_y) -> x25
>- passenger_pos [1, 2, 3, 4, INSIDE] -> x5
>- passenger_destination [1,2,3,4] -> x4
>
>Action space (6 columns) (some actions are impossible due to walls or space constraints, wall hits are penalized with a penalty of $-1$ and the taxi not moving anymore -> the system does not reach nowhere, option is discarded by training)
>- Directions [L, R, U, D]
>- Load
>- Unload

# Approximate solutions

## Motivation:

![[Pasted image 20241218172606.png]]

Comment:
- Bootstrap: Accumulating past variables from the environment;
  $$[S_t, a_t, S_{t+1}, a_{t+1}, done]$$
  Process of bootstraping from the actions stored -> Supervised learning

![[Pasted image 20241218173134.png]]