# 🤖 Agentic AI Development — SE3062 Intelligent Systems

> **Berkeley CS188 Pac-Man Search Project** | Group Assignment — SE3062 Intelligent Systems  
> Third-Year Undergraduate | Faculty of Computing · SE Department

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Algorithms Implemented](#algorithms-implemented)
- [Search Problems](#search-problems)
- [Getting Started](#getting-started)
- [Running the Autograder](#running-the-autograder)
- [Running Pac-Man Visually](#running-pac-man-visually)
- [Question Summary](#question-summary)
- [Attribution](#attribution)

---

## Overview

This repository contains the group implementation of the **Berkeley CS188 Pac-Man Search Project**, submitted as part of the **SE3062 — Intelligent Systems** module. The project explores classical and informed search algorithms by applying them to a Pac-Man game environment. Agents navigate mazes of varying complexity to reach goals, collect food, and visit corners — all while minimising cost.

The codebase is built on top of the official Berkeley AI project infrastructure (v1.004) and extended with custom agent, problem, and heuristic implementations.

---

## Project Structure

```
Agentic-AI-development/
├── search/
│   ├── pacman.py            # Pac-Man game engine
│   ├── game.py              # Core game abstractions (Directions, Actions, Grid)
│   ├── search.py            # Search algorithm implementations (Q1–Q4)
│   ├── searchAgents.py      # Search agents and problem definitions (Q5–Q8)
│   ├── util.py              # Data structures: Stack, Queue, PriorityQueue
│   ├── autograder.py        # Official autograder
│   ├── layouts/             # 37 maze layout files (.lay)
│   └── test_cases/          # Autograder test cases (q1–q8)
├── LICENSE
└── README.md
```

## Algorithms Implemented

| Question | Algorithm | File |
|:---:|---|---|
| Q1 | Depth-First Search (DFS) | `search.py` |
| Q2 | Breadth-First Search (BFS) | `search.py` |
| Q3 | Uniform-Cost Search (UCS) | `search.py` |
| Q4 | A\* Search | `search.py` |

All algorithms are implemented as **graph search** (with visited-state tracking) and operate on a generic `SearchProblem` interface — fully agnostic to the specific problem domain.

---

## Search Problems

| Question | Problem | Description | File |
|:---:|---|---|---|
| Q5 | `CornersProblem` | Find a path that visits all four maze corners | `searchAgents.py` |
| Q6 | `cornersHeuristic` | Admissible & consistent heuristic for A\* on `CornersProblem` | `searchAgents.py` |
| Q7 | `foodHeuristic` | MST-based heuristic for the full food-collection problem | `searchAgents.py` |
| Q8 | `AnyFoodSearchProblem` | Find the closest food dot using BFS | `searchAgents.py` |

### Q5 — Finding All the Corners (Individual Contribution)

The `CornersProblem` represents the task of visiting all four corner cells of a maze. The key design is a compact, **hashable state**:

```python
state = (position, visitedCorners)
# position       = (x, y)                    — Pac-Man's current grid cell
# visitedCorners = (bool, bool, bool, bool)  — one flag per corner
```

- **`getStartState()`** — returns Pac-Man's starting position paired with a boolean tuple that pre-marks any corner Pac-Man starts on as visited.
- **`isGoalState(state)`** — returns `True` only when all four corner flags are `True`.
- **`getSuccessors(state)`** — generates legal moves (wall-checked), builds a **new** immutable `visitedCorners` tuple for each move, and returns triples of `(nextState, action, cost=1)`.

**Autograder result:** ✅ **3 / 3**

---

## Getting Started

### Prerequisites

- Python **3.6+**
- No external dependencies — all code uses the Python standard library.

### Clone the repository

```bash
git clone https://github.com/Thumula99/Agentic-AI-development.git
cd Agentic-AI-development/search
```

---

## Running the Autograder

Run all questions:

```bash
python3 autograder.py
```

Run a specific question (e.g., Q5):

```bash
python3 autograder.py -q q5
```

Run without graphics:

```bash
python3 autograder.py -q q5 --no-graphics
```

---

## Running Pac-Man Visually

### DFS on a medium maze
```bash
python3 pacman.py -p SearchAgent -a fn=depthFirstSearch -l mediumMaze
```

### BFS on a medium maze
```bash
python3 pacman.py -p SearchAgent -a fn=breadthFirstSearch -l mediumMaze
```

### UCS with a cost-penalised agent
```bash
python3 pacman.py -p StayEastSearchAgent -l mediumDottedMaze
```

### A\* with Manhattan heuristic
```bash
python3 pacman.py -p SearchAgent -a fn=aStarSearch,heuristic=manhattanHeuristic -l bigMaze
```

### Q5 — Corners problem with BFS
```bash
python3 pacman.py -p SearchAgent -a fn=bfs,prob=CornersProblem -l tinyCorners
python3 pacman.py -p SearchAgent -a fn=bfs,prob=CornersProblem -l mediumCorners
```

### Q6 — Corners problem with A\* + corners heuristic
```bash
python3 pacman.py -p AStarCornersAgent -l mediumCorners
```

### Q7 — Food search with A\* + food heuristic
```bash
python3 pacman.py -p AStarFoodSearchAgent -l trickySearch
```

### Q8 — Closest dot search
```bash
python3 pacman.py -p ClosestDotSearchAgent -l bigSearch
```

---

## Question Summary

| Q | Topic | Max Marks | Status |
|:---:|---|:---:|:---:|
| Q1 | Depth-First Search | 3 | ✅ |
| Q2 | Breadth-First Search | 3 | ✅ |
| Q3 | Uniform-Cost Search | 3 | ✅ |
| Q4 | A\* Search | 3 | ✅ |
| Q5 | Corners Problem (state representation) | 3 | ✅ |
| Q6 | Corners Heuristic | 3 | ✅ |
| Q7 | Food Heuristic | 5 | ✅ |
| Q8 | Closest Dot Search | 3 | ✅ |
| **Total** | | **26** | |

---

