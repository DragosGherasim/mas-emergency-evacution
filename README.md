# Multiagent Systems — Topic 3: Emergency Evacuation Simulation

## Overview

Implement a multiagent system that simulates an emergency evacuation scenario.

* **Environment:** A 2D grid symbolizing the floorplan of the building that needs to be evacuated, where one or several cells serve as **exits**.
* **Agents:** Multiple **evacuating agents** navigate the grid, while the entire environment is managed by a **monitor agent**.
* **Field of View (FoV):** Each agent has a limited field of view—they cannot see the whole grid, only a small area in their immediate vicinity (e.g., a `9x9` grid area centered around the agent).

---

## Simulation Flow & Agent Behavior

1. **Normal State:**
   * Initially, the agents move randomly, ignoring the exits.
2. **Emergency Declaration:**
   * After a certain time limit expires, the **monitor agent** declares an emergency and broadcasts an emergency message to all other agents.
3. **Emergency Navigation:**
   * Upon being notified of the emergency, agents start moving in a random constant direction.
   * **Wall Collision:** If an agent hits a wall, it chooses another random direction, preferably away from the wall.
4. **When an Exit is in the Field of View:**
   * The agent prioritizes moving toward the visible exit (or the closest one if multiple exits are visible).
   * Two agents cannot occupy the same grid cell. If the direct path to the exit is blocked by other agents, the agent should wait until it clears—unless there is an available free cell closer to the exit, in which case it moves to that cell and continues attempting to reach the exit.
5. **When No Exit is in the Field of View:**
   * **Agent-to-Agent Communication:** If the agent sees other agents, it asks them whether they see an exit.
     * **Positive Response:** The asking agent follows the closest answering agent (while it remains in its field of view) until an exit becomes visible. If no exit becomes visible within a certain time limit, the agent resumes moving in a random direction.
     * **Deadlock Prevention:** To prevent deadlocks (e.g., two agents repeatedly asking each other), an agent cannot ask the same agent again for a certain cooldown period.
     * **Negative Response:** If the agent receives negative responses from the other agents, it moves on in a random direction.
6. **Exit Reached:**
   * Once an agent reaches an exit cell, it is removed from the grid.

---

## Objectives & Experiments

* **Metrics:** Measure the total time required for all agents to evacuate the building.
* **Optimization:** Experiment with different exit placements across the floorplan to determine the configuration that minimizes total evacuation time.