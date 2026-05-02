# Klotski BFS State Space Visualizer (Force Directed Graph)

> 🏆 **Winner of best technical complexity @ ConHacks 2026**

An interactive 3D visualization of the complete state space of the [Klotski sliding block puzzle](https://en.wikipedia.org/wiki/Klotski), built in **Godot 4**. Every reachable board arrangement is a node in a live force-directed graph lattice — and the shortest solution path is highlighted from the starting position. As you play, a blue highlight will be visible on the node / state you are currently in on the force directed graph.

First time using Godot, made some bad choices about how to structure the project because we were rapidly prototyping. What even is a file tree?

---

## What is Klotski?

Klotski is a classic sliding block puzzle. A set of rectangular pieces occupy a grid, and the goal is to slide the large central piece to the exit. The challenge is that pieces block each other — every move changes the entire configuration.

Due to the time constraints we simplified it to only accept 1x2 and 1x3 blocks.

## What does this project do?

Most Klotski solvers just tell you the answer. This one **shows you the entire problem**.

The solver performs a BFS from the starting board position, building a graph where:
- Each **node** is a unique board arrangement
- Each **edge** connects two arrangements reachable from each other by a single move ( only 1 unit sized moves)
- The **shortest solution path** is highlighted through the graph

This graph is then rendered in 3D using a **force-directed layout** — nodes repel each other and edges act as springs — so the graph naturally organizes itself into a readable structure. As you interact with the puzzle board, your current position is highlighted live in the graph.

## Features
### Colour Code
- ⚪ **Full state space graph** — BFS explores all reachable configurations and are displayed as white
- 🔵 **Live board tracking** — your current puzzle position is highlighted blue in the graph in real time
- 🟩 **Shortest path visualization (connections)** — the optimal solution route is highlighted through the graph connections in green
- 🟢 **Solution States** — Nodes highlighted in green are solution states for the puzzle
- 🧲/⚙️ **Force-directed Graph** — nodes self-organize into a lattice structure using repulsion and spring forces and as of this fork, frame-rate independent physics Lol. (mistake made previously as this was our first time using Godot)
- 🧩 **Mini board previews** — each node renders a thumbnail of the board state it represents

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Engine | Godot 4 (GDScript) |
| Solver | BFS in C# (`Board.cs`) |
| Graph layout | Custom force-directed simulation |
| Rendering | 3D instanced scene nodes + dynamic line meshes |

## How it works

### Solver (C#)
`Board.cs` runs a BFS from the initial puzzle state, serializing each board arrangement into a string hash (a flat encoding of the grid). It builds an adjacency list of all reachable states and emits the graph data and shortest winning path via a Godot signal to the Graph Visualizer.

### Visualizer (GDScript)
`GraphNodeTree.gd` receives the adjacency data and:
1. Spawns a 3D node for each board state
2. Draws connection lines between adjacent states
3. Runs a force simulation to spread the graph into a lattice 
4. Highlights the BFS shortest path and the player's current position

### Physics simulation
The layout uses a standard force-directed approach:
- **Repulsion** between all node pairs (inverse square law)
- **Spring attraction** along edges (Hooke's law)
- **Frame-rate independent damping** ensure consistent behavior across hardware as this was causing issue when the delta between frames was different across our hardware.

## Getting Started

1. Clone the repo
2. Open the project in **Godot 4.x** with C# support.
3. Run the scene — the puzzle board and graph will initialize together
4. Move pieces on the board and watch your position update in the graph
5. Change view of graph with Xbox Controller. oh yeah, plug one of those in.
6. The shortest path to the solution is shown automatically on load of a json solution.

## Team

Built at ConHacks 2026 in 36 hours.

Team consisted of: 
Conestoga College Student(s)

[Ivan](https://github.com/igranic8720)

[Ian](https://github.com/IanBlackmore)

[Ryan]{https://github.com/Yunehr)

Wilfred Laurier Student(s)

[Navid](https://github.com/Navidmznn)

## Inspiration:

2swap- I solved Klotski

https://www.youtube.com/watch?v=YGLNyHd2w10
