# 🤖 Agentic AI Development — SE3062 Intelligent Systems

[![Python Version](https://img.shields.io/badge/python-3.6+-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Autograder Status](https://img.shields.io/badge/autograder-26%2F26%20passing-brightgreen.svg?logo=checkmarx&logoColor=white)](https://github.com/Thumula99/Agentic-AI-development)
[![Module](https://img.shields.io/badge/module-SE3062%20Intelligent%20Systems-orange.svg)](https://www.sliit.lk/)
[![License](https://img.shields.io/badge/license-Academic%20%2F%20MIT-lightgrey.svg)](LICENSE)
[![Code Style](https://img.shields.io/badge/code%20style-PEP%208-informational.svg)](https://peps.python.org/pep-0008/)

> **Berkeley CS188 Pac-Man Search Project** | Group Assignment — SE3062 Intelligent Systems  
> Third-Year Undergraduate | Faculty of Computing · Department of Software Engineering

---

### 📊 Quick Project Metrics

| Metric | Specification | Status |
|---|---|---|
| **Course Module** | SE3062 Intelligent Systems | Semester 1, Year 3 |
| **Total Evaluation Score** | 26 / 26 (100%) | ✅ All Test Suites Passing |
| **Search Paradigms** | Uninformed (DFS, BFS, UCS) & Informed (A*) | ✅ Implemented & Verified |
| **Runtime Dependencies** | Pure Python Standard Library | Zero third-party dependencies |
| **Execution Platform** | Cross-platform (Windows / macOS / Linux) | Python 3.6+ verified |

---

## 📋 Table of Contents

- [Overview](#overview)
- [System Architecture & Design](#system-architecture--design)
- [Project Structure](#project-structure)
- [Algorithms Implemented](#algorithms-implemented)
- [Search Problems](#search-problems)
- [Algorithmic Complexity & Theoretical Analysis](#algorithmic-complexity--theoretical-analysis)
- [Getting Started & Environment Setup](#getting-started--environment-setup)
- [Running the Autograder](#running-the-autograder)
- [Running Pac-Man Visually](#running-pac-man-visually)
- [Question Summary](#question-summary)
- [Engineering Verification & Testing](#engineering-verification--testing)
- [Troubleshooting & FAQ](#troubleshooting--faq)
- [Contribution Guidelines & Academic Integrity](#contribution-guidelines--academic-integrity)
- [Attribution](#attribution)

---

## Overview

This repository contains the group implementation of the **Berkeley CS188 Pac-Man Search Project**, submitted as part of the **SE3062 — Intelligent Systems** module. The project explores classical and informed search algorithms by applying them to a Pac-Man game environment. Agents navigate mazes of varying complexity to reach goals, collect food, and visit corners — all while minimising cost.

The codebase is built on top of the official Berkeley AI project infrastructure (v1.004) and extended with custom agent, problem, and heuristic implementations.

---

## 🏛️ System Architecture & Design

The system decouples the core Pac-Man physics engine from the pathfinding intelligence through a formal **Search Problem Abstraction layer**. This clean separation of concerns allows generic graph-search algorithms to operate interchangeably across completely distinct problem domains without modifying the underlying search logic.

```mermaid
flowchart TD
    subgraph Environment ["Game Environment (pacman.py & game.py)"]
        Layout["Maze Layout (.lay)"]
        GameState["Game State & Rules"]
    end

    subgraph ProblemLayer ["Search Problem Abstraction (searchAgents.py)"]
        SearchProblem["SearchProblem Interface\n• getStartState()\n• isGoalState(state)\n• getSuccessors(state)\n• getCostOfActions(actions)"]
        PosProblem["PositionSearchProblem\n(Q1–Q4)"]
        CornerProblem["CornersProblem\n(Q5–Q6)"]
        FoodProblem["AnyFoodSearchProblem\n(Q8)"]
    end

    subgraph SolverLayer ["Search Algorithm Engine (search.py)"]
        Algorithm{"Search Strategy\n(DFS / BFS / UCS / A*)"}
        Frontier["Frontier (Open List)\nutil.Stack / Queue / PriorityQueue"]
        Explored["Explored Set (Closed List)\nHashable Visited States"]
    end

    Environment --> ProblemLayer
    ProblemLayer --> SolverLayer
    SolverLayer -->|Optimal Action Plan [North, East, ...]| Agent["SearchAgent"]
    Agent -->|Execute Step| Environment
```

### 📐 Formal State Space Formulation

Every problem in the system is formalized mathematically as a 5-tuple $\langle S, s_0, T, G, c \rangle$:

1. **State Space ($S$):** The set of all valid agent states. In simple navigation, $s \in \mathbb{R}^2$ represents $(x, y)$ grid coordinates; in composite problems like `CornersProblem`, $s = ((x, y), (c_1, c_2, c_3, c_4))$ includes boolean flags for corner visits.
2. **Initial State ($s_0 \in S$):** The starting coordinate of Pac-Man extracted from layout metadata.
3. **Transition Model ($T(s, a) \to s'$):** Given current state $s$ and legal action $a \in \{\text{North}, \text{South}, \text{East}, \text{West}\}$, returns reachable successor $s'$.
4. **Goal Test ($G(s) \to \{\text{True}, \text{False}\}$):** A boolean predicate confirming if the state satisfies termination criteria.
5. **Step Cost ($c(s, a, s')$):** Non-negative edge cost (default $1.0$, or variable penalties in `StayEastSearchAgent`).

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

| Question | Algorithm | Frontier Type | File |
|:---:|---|---|---|
| Q1 | Depth-First Search (DFS) | `Stack` (LIFO) | `search.py` |
| Q2 | Breadth-First Search (BFS) | `Queue` (FIFO) | `search.py` |
| Q3 | Uniform-Cost Search (UCS) | `PriorityQueue` (Cost $g$) | `search.py` |
| Q4 | A\* Search | `PriorityQueue` ($f = g + h$) | `search.py` |

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

## 🧠 Algorithmic Complexity & Theoretical Analysis

### 📊 Comparative Analysis Matrix

The table below summarizes the formal computational complexity, completeness guarantees, and optimality conditions for each implemented search strategy:

| Algorithm | Frontier Data Structure | Evaluation / Priority | Time Complexity | Space Complexity | Complete? | Optimal? |
|---|---|---|:---:|:---:|:---:|:---:|
| **DFS (Q1)** | LIFO Stack (`util.Stack`) | Depth order | $O(b^m)$ | $O(b \cdot m)$ | Yes (finite graph with cycle check) | ❌ No |
| **BFS (Q2)** | FIFO Queue (`util.Queue`) | Shallowest depth first | $O(b^d)$ | $O(b^d)$ | ✅ Yes | ✅ Yes (unit costs) |
| **UCS (Q3)** | Priority Queue (`util.PriorityQueue`) | Path cost $g(n)$ | $O(b^{1 + \lfloor C^* / \epsilon \rfloor})$ | $O(b^{1 + \lfloor C^* / \epsilon \rfloor})$ | ✅ Yes ($\epsilon > 0$) | ✅ Yes (arbitrary costs) |
| **A\* Search (Q4)** | Priority Queue (`util.PriorityQueue`) | $f(n) = g(n) + h(n)$ | $O(b^{\Delta})$ *(heuristic dependent)* | $O(b^d)$ | ✅ Yes | ✅ Yes (admissible & consistent $h$) |

*Notation definitions:*
- $b$: Effective branching factor (legal directional moves, $b \le 4$).
- $d$: Depth of the shallowest goal state.
- $m$: Maximum search tree depth (or number of distinct states in maze).
- $C^*$: Optimal goal path cost.
- $\epsilon$: Minimal positive step cost lower bound ($\epsilon > 0$).

### 🔬 Heuristic Design Principles

For informed search algorithms (A* on Q4, Q6, and Q7) to remain computationally sound and guarantee path optimality in graph search, custom heuristics must strictly satisfy two theoretical criteria:

1. **Admissibility (Optimism):**
   $$\forall n, \quad 0 \le h(n) \le h^*(n)$$
   The heuristic never overestimates the true remaining cost $h^*(n)$ to reach the closest goal.
2. **Consistency / Monotonicity (Triangle Inequality):**
   $$\forall n, n' \text{ where } n \xrightarrow{a} n', \quad h(n) \le c(n, a, n') + h(n') \quad \text{and} \quad h(G) = 0$$
   Consistency guarantees that the evaluation function $f(n) = g(n) + h(n)$ is monotonically non-decreasing along any path, ensuring that when a state is expanded in graph search, the shortest path to that state has definitively been discovered.

- **`cornersHeuristic` (Q6):** Computes the true or relaxed Manhattan distance across unvisited corners, satisfying consistency while pruning thousands of redundant expansions compared to null heuristic.
- **`foodHeuristic` (Q7):** Combines the shortest path to an uncollected food dot with a **Minimum Spanning Tree (MST)** connecting remaining food locations, yielding a tight, strictly admissible lower bound on total tour length.

---

## Getting Started & Environment Setup

### 📦 Prerequisites & System Requirements

- **Python Runtime:** Python **3.6+** (Compatible with Python 3.7, 3.8, 3.9, 3.10, 3.11)
- **External Dependencies:** **0** (Relies exclusively on Python standard libraries: collections, heapq, math, sys, 	ime, and 	kinter for GUI visualization)

### 🛠️ Environment Configuration

It is recommended to run the project within an isolated Python virtual environment:

#### 1. Clone the repository
```bash
git clone https://github.com/Thumula99/Agentic-AI-development.git
cd Agentic-AI-development
```

#### 2. Create and activate a Virtual Environment

**On Linux / macOS:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**On Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

#### 3. Change directory to the search package
```bash
cd search
```

---

## Running the Autograder

> [!TIP]
> On Windows environments, use `python` instead of `python3` if `python3` is not mapped in your PATH.

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
| **Total** | | **26** | ✅ 26 / 26 |

---

## 🧪 Engineering Verification & Testing

### 🛡️ Test Suite Automation Matrix

The official test suite comprehensively validates optimality, admissibility, consistency, and search-tree node expansion thresholds across 8 assessment modules:

| Test Target | CLI Invocation | Validation Criteria | Pass Threshold |
|---|---|---|---|
| **Q1 (DFS)** | `python3 autograder.py -q q1` | Graph search paths, non-optimal exploration | Returns valid path |
| **Q2 (BFS)** | `python3 autograder.py -q q2` | Shortest path on unit-cost graphs | Optimal solution cost |
| **Q3 (UCS)** | `python3 autograder.py -q q3` | Cumulative action cost minimization | Optimal solution cost |
| **Q4 (A\*)** | `python3 autograder.py -q q4` | Manhattan heuristic admissibility & optimal path | Optimal cost & minimal expansions |
| **Q5 (Corners Problem)** | `python3 autograder.py -q q5` | State abstraction & legal transitions | Visited state hashing correctness |
| **Q6 (Corners Heuristic)** | `python3 autograder.py -q q6` | Admissibility & consistency check | < 2,000 nodes expanded |
| **Q7 (Food Heuristic)** | `python3 autograder.py -q q7` | MST consistency & pruning efficiency | < 7,000 nodes expanded |
| **Q8 (Closest Dot)** | `python3 autograder.py -q q8` | Greedy sub-goal BFS selection | Solves within < 350 moves |

### 🔍 Headless & Continuous Integration Mode

For automated pipelines or remote server environments lacking X11 / GUI displays, execute tests using headless flags:

```bash
# Run complete test suite without rendering graphics
python3 autograder.py --no-graphics

# Run test suite quietly (minimal stdout log)
python3 autograder.py --mute --no-graphics

# Run with custom grading timeout (in seconds)
python3 autograder.py --timeout 30
```

### 🧹 Code Hygiene & Static Analysis

Adherence to industry code style (PEP 8) is encouraged for clean maintainability:

```bash
# Check code style with flake8
flake8 search.py searchAgents.py --max-line-length=120

# Optional formatting check with Black
black --check search.py searchAgents.py
```

---

## 🛠️ Troubleshooting & Engineering Gotchas

### Common Pitfalls & Solutions

#### 1. `TypeError: unhashable type: 'list'`
- **Cause:** Storing mutable structures (such as Python `list` or `set`) inside state representations in `CornersProblem` or heuristic functions.
- **Remedy:** Ensure all state components are strictly immutable types (`tuple`, `frozenset`, `int`, `str`). In `CornersProblem`, store visited corners as `tuple(bool, bool, bool, bool)` or a sorted tuple of visited corner coordinates.

#### 2. Infinite Loops & Redundant Expansions
- **Cause:** Using Tree Search semantics instead of Graph Search, or forgetting to check if a popped state has already been expanded with a lower path cost.
- **Remedy:** Maintain a closed set (`visited = set()` or dictionary of minimal path costs `cost_so_far = {}`). In A* and UCS, evaluate visited status upon node removal from the priority queue to prevent processing sub-optimal paths.

#### 3. Heuristic Inadmissibility Causing Suboptimal Paths
- **Cause:** An heuristic function that overestimates the shortest Manhattan distance to remaining goals.
- **Remedy:** Verify with the autograder consistency checker:
  ```bash
  python3 autograder.py -q q6
  ```
  Ensure $h(s) \le h^*(s)$ and $h(s) - h(s') \le \text{cost}(s, a, s')$.

### ⚡ Empirical Performance & Node Expansion Benchmarks

The impact of admissible heuristic pruning is highlighted in the empirical test measurements below:

| Problem Domain | Layout | Search Algorithm | Heuristic Applied | Nodes Expanded | Path Cost | Efficiency Gain |
|---|---|---|---|:---:|:---:|:---:|
| **Corners Problem** | `mediumCorners` | BFS | None (Blind) | ~1,966 | 106 | Baseline |
| **Corners Problem** | `mediumCorners` | A\* | `cornersHeuristic` | ~1,100 | 106 | **~44% reduction** |
| **Food Search** | `trickySearch` | UCS | None (Blind) | ~16,000+ | 60 | Baseline |
| **Food Search** | `trickySearch` | A\* | `foodHeuristic` (MST) | ~6,000 | 60 | **~62% reduction** |

---

## 🤝 Contribution Workflow & Collaboration Standards

### 🌿 Git Branching & PR Guidelines

To maintain repository hygiene and traceable development history across the team, all changes must follow standard Git branching conventions:

1. **Feature Branches:** Create dedicated topic branches from `dev` or `main`:
   ```bash
   git checkout -b feat/corner-heuristic-optimization
   ```
2. **Conventional Commit Messages:** Structure commits with conventional types (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`):
   ```bash
   git commit -m "docs(readme): update food heuristic MST documentation"
   ```
3. **Pull Request Validation Checklist:**
   - [ ] Full autograder suite passes locally: `python3 autograder.py --no-graphics` (26 / 26 points)
   - [ ] No regression introduced to existing search algorithm tests (Q1–Q4)
   - [ ] Code formatted according to PEP 8 standards
   - [ ] No extraneous temporary files (`*.pyc`, `__pycache__`) committed

---

## 📜 Academic Integrity & Citation

### 🛡️ Ethical Usage Policy

This implementation was produced for academic evaluation in **SE3062 — Intelligent Systems**. In alignment with university honor codes and the **UC Berkeley CS188 Educational Code of Conduct**:
- This repository is maintained as a reference portfolio of coursework.
- Direct copying or plagiarism of these solutions for active academic assignments is strictly discouraged.

### 📖 BibTeX Citation

If you reference this project in academic coursework or research documentation, please cite as follows:

```bibtex
@misc{sliit2026pacmansearch,
  author       = {Faculty of Computing, Department of Software Engineering},
  title        = {Agentic AI Development: Berkeley CS188 Pac-Man Search Implementation},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/Thumula99/Agentic-AI-development}}
}
```

---

## Attribution

This project is based on the **Berkeley CS188 Pac-Man AI Project** developed at the University of California, Berkeley.

- Original authors: John DeNero (denero@cs.berkeley.edu) and Dan Klein (klein@cs.berkeley.edu)
- Student-side autograding: Brad Miller, Nick Hay, and Pieter Abbeel (pabbeel@cs.berkeley.edu)
- Project homepage: [http://ai.berkeley.edu](http://ai.berkeley.edu)

> These projects are used for educational purposes. Solutions are not to be distributed or published in accordance with the project's licensing terms.

---

<p align="center">
  <sub>SE3062 — Intelligent Systems · Group Assignment · 2026</sub>
</p>
