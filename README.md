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

