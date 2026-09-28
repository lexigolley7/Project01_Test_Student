# Project 1 Report: Intelligent Search Visualizer

> [!IMPORTANT]
> **AUTOGRADER COMPLIANCE INSTRUCTIONS:**
> This report is parsed automatically by the autograder. To ensure you receive full credit for your work:
> 1. **Do not modify** the section headers (`## ...`) or bold field keys (e.g., `**Name:**`, `**Selected Region:**`, `**Live Deployment URL:**`, etc.).
> 2. **Write your answers directly after** the colon `:` of each field, replacing the placeholder text completely (including the outer brackets `[` and `]`).
> 3. **Maintain the file structure**. Changing headers, bold titles, or deleting lines can cause the autograder to miss your responses and award 0 marks.

---

## Student Information

- **Name:** Alexandra Golley
- **UID (netID):** agoll8
- **UIN:** 654729361

---

## Section 1: Selected City Region

- **Selected Region:** Illinois, USA

---

## Section 2: Map Graph Configuration

- **Total Cities Configured:** 22
- **Total Connection Edges:** 35
- **Graph Fully Connected:** Yes

---

## Section 3: Local Verification & Search Algorithms

*Check the algorithms you successfully ran and verified on your local development server by placing an `x` in the brackets (e.g., `[x]`):*

- [x] Breadth-First Search (BFS)
- [x] Depth-First Search (DFS)
- [x] Uniform Cost Search (UCS)
- [x] Iterative Deepening Search (IDS)
- [x] Greedy Best-First Search (Greedy)
- [x] A* Search (A*)

---

## Section 4: Deployed and Presentation Information

- **Deployment Platform:** Render
- **Live Deployment URL:** https://project01-test-student-1-y8bz.onrender.com/
- **Video Presentation Link:** https://drive.google.com/file/d/1hPNJYGfO9yScvOoAUGVIMxN2ZwNUaaRz/view?usp=sharing

---

## Section 5: Discussion
- **Which search algorithm is best for this route finding problem?** 
    UCS and A* both found the lowest-cost route in the local Chicago, IL to Springfield, IL test. A* used heuristic information to guide the search, while UCS relied only on accumulated path cost. A* is useful because its heuristic can guide the search toward the goal while still considering path cost.

- **Search Efficiency (Nodes expanded/time taken comparison):** 
    In the Chicago, IL to Springfield, IL test, Greedy Best-First Search expanded 4 nodes, A* expanded 11 nodes, BFS expanded 20 nodes, DFS expanded 15 nodes, UCS expanded 22 nodes, and IDS expanded 51 nodes. BFS, UCS, IDS, Greedy Best-First Search, and A* all found a route with a distance of 205.43 miles in this test, while DFS found a 336.41-mile route. These results show that the number of nodes expanded can vary depending on the search strategy. IDS expanded the most nodes in this particular test because it repeatedly searched with increasing depth limits.

- **Link the idea of search algorithm to today Generative AI.** 
    Search algorithms are related to Generative AI because both involve exploring possible choices to reach a goal. Traditional search algorithms use explicit rules for deciding which node to expand, such as path cost or a heuristic. Generative AI systems can also search through possible outputs or actions while using learned models to guide which possibilities are more promising.
