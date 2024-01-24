# PolicyandValueFunctionofMaze
This project explores the application of Reinforcement Learning (RL) to solve a maze problem. The goal is to find an optimal policy that guides an agent from a start point to a goal point in a maze, while minimizing the cost associated with the path.

**Key Components**
Environment: The maze, represented as a grid where each cell can be a start point, goal point, an open path, or a wall.

Agent: The entity that navigates through the maze, learning from its actions and the received rewards.

Policy: The strategy that the agent uses to decide its actions at each state.

Value Function: A measure of the expected long-term return of a state, considering the current policy.

**Methodology** 
We use a model-free RL algorithm to estimate the value function and derive the policy. The agent interacts with the environment, receiving feedback in the form of rewards. Through iterative updates, the agent improves its policy.


**FUNCTION DEFINITIONS**

self.state_values : A NumPy array of size self.number_of_tiles x self.number_of_tiles filled with zeros. This array will be used to store the estimated values of each state during the reinforcement learning process.
self.policy_probs : A NumPy array of size self.number_of_tiles x self.number_of_tiles x 4, where 4 represents the number of possible actions (up, down, left, right). Each entry in this array is initialized with 0.25, which means initially, each action is equally likely in each state. This array will be used to store the action probabilities for each state during the reinforcement learning process.

**CREATION OF MAZE:**
create_maze(self, level): The level parameter for this method is a list of strings that represents the layout of the maze. It uses the characters "" and "X" to iteratively determine the locations of walls and maze tiles in each level's cell. It appends the wall positions to the walls list and the maze tile positions as (row, col) tuples to the maze list. Ultimately, it yields a tuple with two lists: the walls list, which contains the positions of walls, and the maze list, which contains the positions of maze tiles.

get_init_state(self, level): The level parameter for which denotes the maze layout, is also required by this method. It goes through every cell in the level, looking for the letter "P" each time. As soon as it locates "P" in the maze, it returns "P's" position (row, col) as the starting point for solving the maze. Once "P" is found, the method returns the location as a tuple (row, col) and ends its search.

**TAKING STEPS AND REWARD COMPUTATION**
compute_reward(self, state: Tuple[int, int], action: int): Using the current state of the maze as a starting point, this approach determines the reward for each action. If the state is not equal to the goal_pos, the reward is a negative number (-1.0); otherwise, it is zero. While navigating the maze, the objective is to minimise the overall prize.

step(self, action): This function does the specified action by taking a step in the maze environment. The next_state, the reward from the compute_reward method for the current state and action, and a done flag designating whether or not the agent has reached the goal state are all contained in the tuple that is returned.

simulate_step(self, state, action): Similar to the step method, this method simulates a step in the maze environment for a given state and action, returning the next_state, reward, and done flag without updating the actual environment state.

_get_next_state(self, state: Tuple[int, int], action: int):Using its current state and the selected action as inputs, this internal private method determines the next_state. Analysing the direction of the action (0: left, 1: up, 2: right, 3: down), it determines the next state and determines if it is a wall (not listed among the walls). The agent remains in the same location if the subsequent state is a wall, which returns to the initial state.

**STIMULATING THE ENVIRONMENT**
**Import libraries and create constants:
**  The code imports the required libraries, including Pygame, NumPy, and a custom Maze class.
Several constants are defined to set up the maze environment, screen dimensions, and tile sizes.
**Create the maze environment:
**  The maze layout is defined using a list of strings called level.
An instance of the Maze class is created, passing the level and goal position as parameters.
The env object is then used to reset the maze and find the optimal policy using the solve method.
**Initialize Pygame:
**  Pygame is initialized, and a game window is created with a specified caption.
The main game loop:
  The program enters a loop that runs until the running variable becomes False.
  The reset_goal function is called in a separate thread to check if the player has reached the goal and reset the goal if needed.
**Drawing the maze:**
  The maze is drawn on the screen by iterating through the level list and drawing tiles based on wall characters ("X").
  The player's position is drawn as a rectangle filled with a specific color (red) and centered in the maze cell.
  The goal position is drawn as a green rectangle with rounded corners.
**Update the game state and player movement:**
  The player's action is determined based on the current policy (env.policy_probs) at the player's position. The action with the highest probability is chosen using np.argmax.
  Based on the chosen action, the player's position is updated if it is a valid move (not hitting walls).
  The player's position in the env object is updated as well.
**Frame rate control:**
  The game loop is controlled with a frame rate of 60 frames per second using clock.tick(60).
**Exiting the game:**
  The game loop exits when the running variable becomes False, typically triggered by the user closing the game window.
  Pygame is closed with pygame.quit().

**SOLVING THE MAZE**
The solve method represents the implementation of the Value Iteration algorithm to find an optimal policy for the maze environment.

Let's go through the method step by step:

Initialization:

  gamma: The discount factor for future rewards (default value: 0.99).
  theta: The threshold for convergence (default value: 1e-6).
  delta: A variable initialized to infinity, which will be used to measure the change in state values during the iteration.

Iterative Value Update:
  The method uses a while loop that continues until the change in state values (delta) becomes smaller than the specified convergence threshold (theta).Within the loop, the delta variable is reset to 0 before each iteration.

Value Iteration:
The approach determines the Q-value (q_max) for every single cell in the maze (barring walls) for each potential action (up, down, left, and right).The instantaneous reward from the simulate_step method and the discounted value of the value of the next state (gamma * self.state_values[next_state]) are used to calculate the Q-value. Following the choice of the action with the maximum Q-value (q_max), the related policy probabilities (action_probs) are adjusted to reflect the action's higher probability of being picked.

Updating State Values:
 The state value (self.state_values[row, col]) for a given state is updated to the maximum Q-value (q_max) among all actions after the Q-values for all actions in that state have been determined.The activities that led to the largest Q-value have a high probability (1.0) in the policy probabilities (self.policy_probs[row, col]) for that state, while all other actions have a probability of 0.

Convergence Check:
 The approach determines whether the biggest change in state values (delta) during this iteration is less than the convergence threshold (theta) after updating the state values and policy probabilities for each state in the maze.
The loop ends and the maze-solving process is deemed to have converged if the condition is satisfied.
Value iteration is carried out repeatedly until the state values converge. At that point, the best course of action is identified by maximising the expected cumulative reward for figuring out how to get from the starting state to the desired state.

