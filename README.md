Reinforcement Learning in a Custom Maze Environment

Introduction

This project implements a Reinforcement Learning (RL) agent trained to navigate a custom maze environment using Deep Deterministic Policy Gradient (DDPG) with Hindsight Goal Ranking (HGR). The maze increases in difficulty over time by adding more obstacles, and the agent is trained using a Convolutional Neural Network (CNN)-based policy.

Additionally, a Soft Actor-Critic (SAC) agent is implemented to compare performance against DDPG. SAC incorporates entropy-based exploration and utilizes two Q-value estimators for stability.

Environment: CustomMazeEnv

The CustomMazeEnv class defines a custom maze where the agent learns to navigate from a random start position to a goal while avoiding obstacles.

Key Features:

State Representation: A 3D matrix with three channels:

Channel 0: Agent position

Channel 1: Goal position

Channel 2: Obstacles

Action Space: Four discrete actions: 0 = up, 1 = down, 2 = left, 3 = right.

Obstacle Evolution: The number of obstacles increases over time to introduce curriculum learning.

Reset Function: Randomly initializes the agent and goal while ensuring there is a valid path between them.

Reward Function:

+1 if the agent reaches the goal.

Small negative reward proportional to the distance from the goal.

Additional reward for moving closer to the goal.

Agents: DDPG vs SAC

DDPG Agent

The DDPGAgent class implements an RL agent using a policy-based approach with a CNN-based actor network.

Network Architecture

ActorCNN: Three convolutional layers (64, 128, 256 filters) followed by two fully connected layers.

Training Mechanism:

Uses Temporal Difference (TD) Error to prioritize learning from important transitions.

Replay Buffer (HGRReplayBuffer) for experience replay and importance sampling.

Soft updates for stability (tau = 0.005).

SAC Agent

The SACAgent implements Soft Actor-Critic for comparison.

Network Architecture

ActorSAC: Similar to DDPG but outputs a probability distribution over actions.

CriticSAC: Uses two Q-value estimators to improve stability.

Entropy Regularization: Encourages exploration by maximizing entropy.

Training Mechanism

Uses Replay Buffer for experience storage.

Critic Updates: Learns Q-values using two critics.

Policy Updates: Learns policy by maximizing expected reward with entropy regularization.

Target Networks: Soft updates (tau = 0.005).

Hindsight Goal Ranking (HGR) Replay Buffer

The HGRReplayBuffer enhances learning by prioritizing transitions with high TD error.

Stores transitions (state, action, reward, next_state, done)

Computes TD Error and prioritizes sampling transitions that contribute the most to learning.

Importance Sampling: Uses probability-based sampling for more effective learning.

Training Procedure

The training loop executes for a given number of episodes:

Reset Environment: Initialize agent position and obstacles.

Interact with Environment: The agent selects an action based on the policy and receives a new state and reward.

Store in Replay Buffer: The transition is stored with its TD error.

Train the Agent: Once the buffer has sufficient samples, the agent updates its policy using minibatch training.

Increase Difficulty: Every 20 episodes, an additional obstacle is added.

Save Progress: The model and performance plots are saved periodically.

Performance Monitoring

The script logs and saves:

Rewards per episode: Indicates agent improvement over time.

Success rate: Measures how often the agent reaches the goal.

Steps per episode: Tracks efficiency in reaching the goal.

Obstacle count: Monitors increasing difficulty.

Benchmarking and Additional Tests

To compare the effectiveness of DDPG and SAC, multiple benchmarking tests were conducted:

Multi-Run Testing

The trained model was tested on multiple runs with different starting and goal positions.

Performance plots were generated to compare cumulative rewards and goal-reaching efficiency.

Results were saved and visualized at different time steps.

Multi-Level Maze Testing

The agent was evaluated in a multi-level maze where it progresses through increasing difficulty levels.

Performance was tracked in terms of rewards, levels completed, and success rates.

Multi-Objective Maze Testing

The agent was tested on an environment with multiple goals.

Performance was measured based on how many goals were reached and total rewards earned per episode.
