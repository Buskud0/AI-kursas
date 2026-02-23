# AI-kursas
**Promptas kurį siuntėme į Antigravity AI, kad sukurtų python paieškos uždavinį:**
```Tema: Robot Navigation with Obstacles
Aprašymas: Robotas turi nukeliauti iš taško A $(0,0)$ į tašką B $(N,M)$ tinklelyje, kuriame yra sienos (nepraeinami langeliai).
Pilnas promptas (anglų kalba)
Nukopijuok ir įklijuok šį tekstą į AI įrankį (pvz., ChatGPT ar Claude):
Role: You are a Senior Python Developer specializing in Artificial Intelligence.
Task: Create a new search problem class using the AIMA (Artificial Intelligence: A Modern Approach) Python framework architecture.
Problem Description: > Implement a GridNavigationProblem class that inherits from aima.search.Problem. The goal is to find the shortest path for a robot in a 2D grid from a starting coordinate to a target coordinate, avoiding obstacles (walls).
Technical Requirements:
State Representation: A state should be a tuple of (x, y) coordinates.
Obstacles: The class should take a set of "wall" coordinates as an input during initialization.
Actions: The robot can move in 4 directions: 'UP', 'DOWN', 'LEFT', 'RIGHT'. Ensure the robot cannot move into a wall or outside the grid boundaries (e.g., 10x10).
Methods to Implement:
__init__(self, initial, goal, walls, grid_size)
actions(self, state): Return valid moves only.
result(self, state, action): Return the new (x, y) tuple.
goal_test(self, state): Check if the robot reached the target.
path_cost(self, c, state1, action, state2): Each move costs 1.
Heuristic: Provide a Manhattan distance heuristic function h(node) for A* search.
Execution Example: Show how to solve this problem using breadth_first_graph_search and astar_search with the provided heuristic.
```
