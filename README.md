# Pacman Search Agents

Pacman agents navigates maze worlds using uninformed and informed search strategies — finding paths to goals, collecting food, and avoiding ghosts efficiently.

---

## Table of Contents

- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Algorithms Implemented](#algorithms-implemented)
- [Available Agents](#available-agents)
- [Questions / Tasks](#questions--tasks)
- [Key Concepts](#key-concepts)
- [Attribution](#attribution)

---

## Project Structure

```
pecman_search_agents/
├── search.py             # Core search algorithm implementations (DFS, BFS, UCS, A*)
├── searchAgents.py       # Pacman agents and search problem definitions
├── pacman.py             # Main game entry point
├── game.py               # Game logic and state management
├── util.py               # Data structures: Stack, Queue, PriorityQueue
├── eightpuzzle.py        # Eight-puzzle problem (tests generic search)
├── autograder.py         # Automated grading script
├── commands.txt          # All project commands for easy copy-paste
├── layouts/              # Maze layout files (tinyMaze, mediumMaze, bigMaze, etc.)
└── test_cases/           # Autograder test cases
```

---

## Getting Started

### Prerequisites

- Python 3.x (no external dependencies)

### Run a Basic Game

```bash
python pacman.py
```

### All Options

```bash
python pacman.py -h
```

Options support both long (`--layout`) and short (`-l`) forms. All commands in this project are also listed in `commands.txt`. On UNIX/Mac, run them all at once with:

```bash
bash commands.txt
```

---

## Algorithms Implemented

| Algorithm | Function | Data Structure | Optimal? |
|-----------|----------|----------------|----------|
| Depth-First Search | `depthFirstSearch` | `Stack` | No |
| Breadth-First Search | `breadthFirstSearch` | `Queue` | Yes (uniform cost) |
| Uniform Cost Search | `uniformCostSearch` | `PriorityQueue` | Yes |
| A* Search | `aStarSearch` | `PriorityQueue` | Yes (with admissible heuristic) |

All algorithms are implemented in `search.py` and return a **list of actions** (directions) that lead Pacman from start to goal.

### Abbreviations (for use with `-a fn=`)

| Short | Full |
|-------|------|
| `dfs` | `depthFirstSearch` |
| `bfs` | `breadthFirstSearch` |
| `ucs` | `uniformCostSearch` |
| `astar` | `aStarSearch` |

---

## Available Agents

| Agent | Description |
|-------|-------------|
| `GoWestAgent` | Always moves West — a trivial reflex agent |
| `SearchAgent` | Plans a full path using a specified search algorithm |
| `StayEastSearchAgent` | UCS agent that prefers staying East (low ghost areas) |
| `StayWestSearchAgent` | UCS agent that prefers staying West (high ghost areas) |
| `AStarCornersAgent` | A* agent that visits all four corners |
| `AStarFoodSearchAgent` | A* agent that collects all food |
| `ClosestDotSearchAgent` | Greedily eats the closest dot |

### Available Heuristics for aStar Search

| Heuristic | Description |
|-----------|-------------|
| `nullHeuristic` | Always returns 0 (trivial) |
| `manhattanHeuristic` | Manhattan distance to goal |
| `euclideanHeuristic` | Euclidean distance to goal |
| `cornersHeuristic` | Heuristic for visiting all four corners |
| `foodHeuristic` | Heuristic for collecting all food |

---

## Problems

### Depth First Search

Implement `depthFirstSearch` in `search.py`. Use `util.Stack`. The algorithm must avoid re-expanding visited states (graph search, not tree search).

```bash
# Verify SearchAgent works first
python pacman.py -l tinyMaze -p SearchAgent -a fn=tinyMazeSearch

# Test DFS on progressively larger mazes
python pacman.py -l tinyMaze -p SearchAgent
python pacman.py -l mediumMaze -p SearchAgent
python pacman.py -l bigMaze -z .5 -p SearchAgent
```

> The board shows an overlay of explored states (brighter red = explored earlier).
> Expected solution length for `mediumMaze`: **130** (pushing successors in the order returned by `getSuccessors`).

---

### Breadth First Search

Implement `breadthFirstSearch` in `search.py`. Use `util.Queue`. BFS finds the **fewest-actions** path.

```bash
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs
python pacman.py -l bigMaze -p SearchAgent -a fn=bfs -z .5
```

Also works on the eight-puzzle without any changes:

```bash
python eightpuzzle.py
```

> Tip: Use `--frameTime 0` if Pacman moves too slowly.

---

### Uniform Cost Search

Implement `uniformCostSearch` in `search.py`. Use `util.PriorityQueue`. UCS finds the **least-cost** path and supports custom cost functions.

```bash
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs
python pacman.py -l mediumDottedMaze -p StayEastSearchAgent
python pacman.py -l mediumScaryMaze -p StayWestSearchAgent
```

> `StayEastSearchAgent` and `StayWestSearchAgent` use exponential cost functions — expect very low and very high path costs respectively.

---

### A* Search *(to be implemented)*

Implement `aStarSearch` in `search.py`. A* combines UCS with a heuristic to guide search toward the goal more efficiently.

```bash
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic
```

---

## Key Concepts

### SearchProblem Interface

All search problems implement these methods (defined in `search.py`):

| Method | Returns |
|--------|---------|
| `getStartState()` | The initial state |
| `isGoalState(state)` | `True` if the state is the goal |
| `getSuccessors(state)` | List of `(successor, action, stepCost)` triples |
| `getCostOfActions(actions)` | Total cost of an action sequence |

### Important Notes

- Search functions must return a **list of legal actions** (directions) from start to goal.
- Use the `Stack`, `Queue`, and `PriorityQueue` from `util.py` — they have specific properties required by the autograder.
- Each search node must store both the **state** and the **path** to reach it.
- All algorithms should implement **graph search** (track visited nodes to avoid cycles).

### Running the Autograder

```bash
python autograder.py
```

---

## Attribution

Based on the Pacman AI Projects developed at UC Berkeley by John DeNero and Dan Klein.
Student autograding added by Brad Miller, Nick Hay, and Pieter Abbeel.
See [http://ai.berkeley.edu](http://ai.berkeley.edu).

> You are free to use or extend this project for educational purposes provided that you:
> (1) do not distribute or publish solutions,
> (2) retain this notice, and
> (3) provide clear attribution to UC Berkeley.
