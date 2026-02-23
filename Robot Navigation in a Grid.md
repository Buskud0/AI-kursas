# AI-kursas
**Skaidrės:**

https://gamma.app/docs/AI-Search-Algorithms-with-the-AIMA-Architecture-9c9abkb9bm4bsg6

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

**Python kodas:**
```python
from search import Problem, breadth_first_graph_search, astar_search

class GridNavigationProblem(Problem):
    """
    Paieškos problema: roboto navigacija 2D tinklelyje išvengiant kliūčių (sienų).
    """
    
    def __init__(self, initial, goal, walls, grid_size):
        """
        initial: tuple (x, y) - pradinė roboto pozicija
        goal: tuple (x, y) - tikslo pozicija
        walls: set of tuples (x, y) - aibė su sienų/kliūčių koordinatėmis
        grid_size: tuple (plotis, aukštis) - tinklelio matmenys, pvz., (10, 10).
                   Koordinatės svyruoja nuo 0 iki plotis-1 ir nuo 0 iki aukštis-1.
        """
        super().__init__(initial, goal)
        self.walls = walls
        self.grid_size = grid_size
        
        # Galimi judesiai atitinkamomis kryptimis
        self.moves = {
            'UP': (0, 1),
            'DOWN': (0, -1),
            'LEFT': (-1, 0),
            'RIGHT': (1, 0)
        }

    def actions(self, state):
        """
        Grąžina galimus ir legalius judesius (actions) iš nurodytos būsenos (state).
        Atsižvelgiama į tinklelio ribas ir sienas.
        """
        possible_actions = []
        x, y = state
        width, height = self.grid_size
        
        for action, (dx, dy) in self.moves.items():
            new_x, new_y = x + dx, y + dy
            
            # Neleidžiame robotui išeiti už tinklelio ribų
            if 0 <= new_x < width and 0 <= new_y < height:
                # Patikriname, ar naujame langelyje nėra sienos
                if (new_x, new_y) not in self.walls:
                    possible_actions.append(action)
                    
        return possible_actions

    def result(self, state, action):
        """
        Grąžina naują būseną (x, y), kurią pasieksime atlikę nurodytą veiksmą (action).
        """
        x, y = state
        dx, dy = self.moves[action]
        return (x + dx, y + dy)

    def goal_test(self, state):
        """
        Patikrina, ar dabartinė būsena sutampa su tikslu.
        """
        return state == self.goal

    def path_cost(self, c, state1, action, state2):
        """
        Paskaičiuoja kelio kainą. Kiekvienas žingsnis kainuoja lygiai 1.
        'c' yra sukaupta kaina iki 'state1'.
        """
        return c + 1


def manhattan_heuristic(node, problem):
    """
    Manhatano atstumo euristika skirta A* algoritmui.
    Apskaičiuoja atstumą tarp esamo mazgo būsenos ir tikslo (juda tik horizontaliai ir vertikaliai).
    """
    current_state = node.state
    goal_state = problem.goal
    
    # |x1 - x2| + |y1 - y2|
    return abs(current_state[0] - goal_state[0]) + abs(current_state[1] - goal_state[1])


# ==========================================
# Execution Example (Vykdymo pavyzdys)
# ==========================================
if __name__ == '__main__':
    # 1. Problemos konfigūracija
    grid_size = (10, 10)     # 10x10 dydžio tinklelis
    initial_state = (0, 0)   # Pradedame nuo tinklelio kampo pakraščio
    goal_state = (9, 9)      # Norime pasiekti priešingą kampą
    
    # Apibrėžiame kliūtis (sienas), kad padarytume kelią įdomesnį
    walls = {
        (1, 0), (1, 1), (1, 2), 
        (3, 4), (4, 4), (5, 4),
        (7, 8), (8, 8), (9, 8)
    }
    
    # Inicializuojame problemos objektą
    problem = GridNavigationProblem(initial_state, goal_state, walls, grid_size)
    
    # 2. Vykdome paiešką naudodami Breadth-First Graph Search (BFS)
    print("Sprendžiama naudojant Breadth-First Graph Search (BFS)...")
    bfs_result = breadth_first_graph_search(problem)
    
    if bfs_result:
        print(f"Kelio ilgis (kaina): {bfs_result.path_cost}")
        print(f"Sekvencija (veiksmai): {bfs_result.solution()}")
    else:
        print("Tikslo pasiekti nepavyko (nėra kelio).")
        
    print("-" * 50)
    
    # 3. Vykdome paiešką naudodami A* Search su Manhatano atstumo euristika
    print("Sprendžiama naudojant A* Search (su Manhatano euristika)...")
    
    # AIMA `astar_search` tikisi funkcijos h(node), todėl naudojame 'lambda', 
    # kad prijungtume euristikos funkciją prie mūsų konkretaus problemos objekto.
    h = lambda node: manhattan_heuristic(node, problem)
    
    astar_result = astar_search(problem, h=h)
    
    if astar_result:
        print(f"Kelio ilgis (kaina): {astar_result.path_cost}")
        print(f"Sekvencija (veiksmai): {astar_result.solution()}")
    else:
        print("Tikslo pasiekti nepavyko (nėra kelio).")
```
