# Heap Go Simulations

A Python implementation of a simulation-based player for Heap Go, developed for CMPUT 455 at the University of Alberta.

The project uses repeated game simulations to evaluate possible moves and select one based on its estimated win rate. It supports both uniform-random and probabilistic move-selection policies.

## Overview

Heap Go is a two-player game in which players take turns selecting non-empty heaps and collecting tokens according to their colors. Each token has a point value, and the winner is determined by the final scores, including komi.

This project implements a simulation-based approach to playing Heap Go. Instead of searching the entire game tree, the player evaluates candidate moves by simulating possible continuations of the game and counting how often each move leads to a win.

## Features

- **Random simulations:** Plays out a game by selecting legal moves uniformly at random until the game ends.
- **Monte Carlo move selection:** Evaluates each legal move using a configurable number of simulations and selects the move with the highest estimated win count.
- **Probabilistic simulations:** Assigns higher selection probabilities to moves that collect more points immediately.
- **Configurable simulation policies:** Switches between random and probabilistic move selection.
- **Move probability reporting:** Displays the selection probabilities of legal moves under the current policy, rounded to three decimal places.
- **Heap Go command interface:** Supports game setup, move execution, score inspection, legal-move queries, and other game commands.

## Technologies

- Python 3
- `random` for randomized simulations and weighted move selection
- `ast` for parsing game-state input
- Standard Python data structures for representing heaps, scores, and simulation states

## How It Works

### 1. Random Simulations

The player repeatedly selects a legal move and simulates the game until all heaps are empty. The final score difference determines the winner.

### 2. Monte Carlo Move Selection

For each legal move, the player:

1. Evaluates the immediate effects of the move.
2. Runs a configurable number of simulations from the resulting position.
3. Counts the simulations won by the player choosing the move.
4. Compares the results across legal moves and selects the move with the highest win count.

The default is 10 simulations per move, configurable from 1 to 100. Moves with equal win counts are resolved by the order in which they are considered.

### 3. Probabilistic Move Selection

The random policy gives every legal move an equal probability of being selected. The probabilistic policy instead uses the points collected by a move as its weight.

A move's probability is calculated as:

\[
P(m)=\frac{w(m)}{\sum_{j\in M}w(j)}
\]

where \(w(m)\) is the immediate point gain of move \(m\), and \(M\) is the set of legal moves.

This policy favors moves that collect more points immediately while still allowing other legal moves to be selected.

### 4. Efficient Simulation

The implementation precomputes lookup tables for the points gained and number of tokens removed by moves at different heap sizes. Simulations then track heap sizes and the score difference instead of repeatedly modifying the full token lists.

This reduces the work needed to simulate game continuations and keeps the original game state unchanged during move evaluation.

## Project Structure

```text
cmput-455-assignment-3-heap-go-simulations/
└── a3.py    # Heap Go game logic, simulations, and player
```

## Running the Program

Run the player using Python 3:

```bash
python3 a3.py
```

The program accepts text commands through standard input. For example, the following commands configure a game, select the probabilistic policy, set the number of simulations, and generate a move:

```text
heapgo 0.5 [[('b', 3), ('w', 2)], [('w', 4)], [('b', 5)]]
policy probabilistic
numsimulations 20
genmove
```

Other supported commands include `simulate`, `policymoves`, `show`, `score`, `legal`, and `winner`.

## Testing

The implementation can be tested using the provided public test suite:

```bash
python3 a3test.py a3.py assignment3-public-tests.txt
```

The test suite checks the program's command behavior and expected outputs. The assignment also requires testing on a standard Linux undergraduate machine.

## What I Learned

- Implementing Monte Carlo methods for game-playing agents
- Evaluating candidate moves using repeated randomized simulations
- Applying probability and heuristic weighting to move selection
- Precomputing lookup tables to reduce repeated computation
- Tracking game states and score differences efficiently
- Designing and testing a command-driven Python application

---

Developed as part of CMPUT 455 — Search and Planning in Artificial Intelligence at the University of Alberta.
