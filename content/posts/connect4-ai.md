---
title: "Connect4AI: Minimax Agent with Pygame GUI"
date: 2026-03-03
draft: false
tags: ["Artificial Intelligence", "Python", "NumPy", "Minimax Algorithm", "Pygame"]
summary: "An interactive Connect 4 application featuring an AI agent that uses Minimax with Alpha-Beta pruning to evaluate thousands of board states in real-time."
showToc: true
TocOpen: false
---

## Project Overview
As part of my AI specialization at Durham College, I developed a complete, interactive Connect 4 game from scratch. Rather than just writing a console script, I built a full visual application using **Pygame** where human players can test their skills against an autonomous AI agent.

The underlying engine relies on a custom implementation of the **Minimax algorithm**, heavily optimized using **NumPy** for rapid matrix evaluations.

## 1. Mathematical Board Representation
To ensure the AI could calculate thousands of potential future moves without lagging the game interface, standard Python lists were too slow. 

I engineered the 6x7 game board as a 2D **NumPy zero-matrix**. As pieces are dropped, the matrix updates with `1`s (Human) and `2`s (AI). This allowed me to use advanced array slicing to instantly extract horizontal, vertical, and diagonal windows for the AI to evaluate.

## 2. The AI Engine: Minimax & Alpha-Beta Pruning
The brain of the agent evaluates the board by looking **5 moves ahead** into the future. 

It uses the Minimax algorithm to simulate all possible outcomes, assuming the human will play perfectly to minimize the AI's score, while the AI seeks to maximize it. 

![Decision Tree illustrating Minimax with Alpha-Beta Pruning](/images/connect4/minimax.png)
*Caption: A conceptual diagram of the Minimax tree. Alpha-Beta pruning cuts off redundant search branches to keep the Pygame loop running smoothly.*


To prevent the game from freezing during these massive calculations, I implemented **Alpha-Beta Pruning** within the recursive loop. This mathematical optimization aggressively ignores branches of the game tree that are proven to be worse than previously evaluated paths, exponentially speeding up the AI's response time.

## 3. Custom Heuristic Design
Because the AI cannot search all the way to the end of the game on its first turn, I designed a custom heuristic scoring function `score_position()` to evaluate non-terminal board states.

The AI dynamically scores the matrix based on several strategic rules:
* **Center Control:** It applies a 3x point multiplier for pieces secured in the center column, as this position offers the highest mathematical probability of creating a 4-in-a-row.
* **Offensive Traps:** It heavily rewards board states where it has 3 pieces and an empty slot (+5 points), actively setting up winning traps.
* **Defensive Blocking:** If the array detects the human opponent has 3 pieces in a row with an open slot, it applies a heavy penalty (-4 points). This forces the Minimax algorithm to prioritize blocking the opponent over advancing its own secondary strategies.

## 4. The Pygame Interface
The mathematical engine is tied directly into a Pygame event loop. 

![Connect 4 Pygame Interface](/images/connect4/pygame-interface.png)
*Caption: The interactive Pygame GUI. The system tracks mouse motion to hover pieces over the columns before dropping them into the NumPy matrix.*


The interface tracks mouse events, maps the physical pixel coordinates to the exact column in the NumPy array, and dynamically re-renders the board graphics after every turn. Once a terminal state is reached (a win or a draw), the game cleanly halts and displays the victor.