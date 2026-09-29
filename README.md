# 🐦 Flappy Bird Reinforcement Learning

An educational project that trains an AI agent to play Flappy Bird using Reinforcement Learning. The agent learns a policy with a Deep Q-Network (DQN), using observations from the game environment and rewards from its actions.

## 🎮 Game / Environment

The project uses [`flappy-bird-gymnasium`](https://pypi.org/project/flappy-bird-gymnasium/) to provide a Flappy Bird environment through the Gymnasium API. The environment ID configured by the project is `FlappyBird-v0`.

The agent receives a discrete action from the environment:

| Action | Meaning |
| --- | --- |
| `0` | Do nothing |
| `1` | Flap |

## 🏆 Rewards

The environment's reward behavior is described as follows:

| Event | Reward |
| --- | ---: |
| Bird stays alive for a frame | `+0.1` |
| Successfully passes a pipe | `+1.0` |
| Bird dies | `-1.0` |
| Bird touches the top of the screen | `-0.5` |

## 🎮 Gameplay Demo

<!-- TODO: Add your recorded gameplay video here: assets/gameplay.mp4 -->

[▶️ Watch Gameplay Video](assets/gameplay.mp4)

An optional GIF preview can be added at `assets/gameplay.gif`:

<!-- TODO: Add assets/gameplay.gif and display it with: ![Flappy Bird Gameplay](assets/gameplay.gif) -->

## 🧠 How It Works

The training loop follows this high-level flow:

```text
Environment
    ↓
State
    ↓
DQN
    ↓
Action
    ↓
Reward
    ↓
Replay Memory
    ↓
Training
    ↓
Updated DQN
```

- **State:** A numerical observation returned by the environment that describes the current game situation. The DQN uses it as its input.
- **Action:** The choice made by the agent: do nothing or flap.
- **Reward:** Feedback returned by the environment after an action. The agent aims to maximize the total reward over time.
- **Q-values:** The DQN estimates a value for each possible action in the current state. The agent can choose the action with the highest estimated value.
- **Neural network:** The DQN is a PyTorch feed-forward network with a hidden layer. Its output contains one estimated Q-value per action.
- **Exploration vs. exploitation:** During training, epsilon-greedy action selection sometimes samples a random action (exploration) and otherwise chooses the action with the highest estimated Q-value (exploitation). Epsilon decreases toward a configured minimum over time.
- **Replay memory:** The agent stores experiences containing the state, action, next state, reward, and termination flag. It later samples a mini-batch of these experiences so training can learn from varied past situations.
- **Training:** The policy network is updated from sampled experiences. A separate target network supplies target Q-values and is periodically synchronized with the policy network.

## 📁 Project Structure

```text
FLAPPY_BIRD/
├── agent.py
├── dqn.py
├── experience_replay.py
├── game_flappy_bird.py
├── parameters.yaml
├── README.md
├── .gitignore
└── assets/
    ├── gameplay.mp4   # Add your recorded gameplay video
    └── gameplay.gif   # Optional GIF preview
```

The gameplay files are optional and are not included until you add your own recordings.

| File | Purpose |
| --- | --- |
| `agent.py` | Creates the Gymnasium environment, runs the training or evaluation loop, selects actions, stores experiences, and updates the networks. |
| `dqn.py` | Defines the PyTorch neural network used to estimate Q-values for the available actions. |
| `experience_replay.py` | Implements a bounded replay memory and random experience sampling. |
| `game_flappy_bird.py` | Opens a human-rendered game for keyboard play; press Space to flap. |
| `parameters.yaml` | Holds the environment ID and training hyperparameters, including learning rate, discount factor, epsilon values, replay capacity, and batch size. |
| `README.md` | Project overview, setup instructions, and documentation. |
| `.gitignore` | Excludes generated Python cache files, local environments, logs, and trained model outputs from Git. |
| `assets/` | Optional location for recorded gameplay media. |

## 🧰 Technologies Used

These are used or directly imported by the project:

- Python
- PyTorch
- Gymnasium
- Flappy Bird Gymnasium (`flappy-bird-gymnasium`)
- Pygame
- PyYAML (`yaml`)
- Deep Q-Network (DQN)
- Experience Replay

## ⚙️ Installation

These instructions assume Python and Git are installed on Windows. Replace `<repository-url>` with the URL of your GitHub repository.

```bash
git clone <repository-url>
cd FLAPPY_BIRD

pip install torch
pip install gymnasium
pip install flappy-bird-gymnasium
pip install pygame
pip install pyyaml
```

## ▶️ Run

Start training with the parameter-set name defined in `parameters.yaml`:

```bash
python agent.py flappybirdv0 --train
```

To load the saved model and render the agent playing, run:

```bash
python agent.py flappybirdv0
```

For keyboard-controlled gameplay:

```bash
python game_flappy_bird.py
```

Training logs and the best saved model are written under `runs/`.