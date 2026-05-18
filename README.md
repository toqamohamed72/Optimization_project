Emergency Routing Optimization
===============================
A Python project that optimizes emergency vehicle routing on a real road network
(Ismailia, Egypt) using OpenStreetMap data and two optimization algorithms:
2-Opt Local Search and Simulated Annealing.


Overview
--------
When multiple emergencies occur simultaneously, efficient vehicle dispatch is critical.
This project models the problem on a real road network, assigns emergencies to the
nearest hospitals, then applies optimization algorithms to minimize total travel distance.

Study Area: Ismailia, Egypt
  - Road network nodes : 34,978
  - Road network edges : 91,197
  - Hospitals found    : 5
  - Emergency incidents: 10 (randomly generated)


How It Works
------------
1. Load Map & Extract Hospitals
   Fetches the road network from OpenStreetMap (osmnx) and locates hospital nodes.

2. Generate Emergency Incidents
   Randomly selects 10 road nodes to simulate emergencies.

3. Distance Precomputation
   Runs Dijkstra's algorithm from every hospital and emergency node, caching
   all shortest-path distances for fast lookup.

4. Greedy Baseline Assignment
   Each emergency is assigned to its closest hospital (benchmark).

5. 2-Opt Local Search
   Iteratively reverses route segments per hospital until no improvement is found.

6. Simulated Annealing (SA)
   Probabilistic optimizer that escapes local optima via three move types:
   swap_between, move_between, and two_opt. Accepts worse solutions early on
   (high temperature) and tightens criteria as it cools.


Results
-------
  Method                  Total Distance    Improvement    Time
  ----------------------  ----------------  -------------  ------
  Baseline (Greedy)       138,588 m         --             --
  2-Opt Local Search       76,561 m         44.76%         0.001s
  Simulated Annealing      69,451 m         49.89%         0.061s

SA achieves the best result by redistributing emergencies across hospitals,
which 2-Opt alone cannot do.


Cooling Rate Sensitivity
------------------------
  Cooling Rate    Total Distance
  ------------    --------------
  0.980           69,727 m
  0.990           71,658 m
  0.995           72,704 m
  0.999           69,006 m  <-- best

Slower cooling (closer to 1.0) allows more exploration and yields better results.


Installation
------------
  pip install osmnx networkx matplotlib numpy


Usage
-----
1. Clone or download the project files.
2. Install the required libraries (see above).
3. Open "Final Result.ipynb" in Jupyter Notebook.
4. Run all cells from top to bottom.

Note: The first run may take a few minutes to download the road network from OpenStreetMap.


Project Structure
-----------------
  emergency-routing-optimization/
  |-- Final Result.ipynb          # Main Jupyter notebook
  |-- emergency_routing_clean.py  # Clean Python script (no comments)
  |-- README.txt                  # This file


Key Parameters
--------------
  Parameter        Default    Description
  ---------------  ---------  ----------------------------------
  NUM_EMERGENCIES  10         Number of emergency incidents
  RANDOM_SEED      42         Seed for reproducibility
  T_init           6000.0     SA initial temperature
  cooling          0.995      SA cooling rate
  iterations       4000       SA number of iterations


Visualizations
--------------
  - Bar chart       : Per-hospital distances across all three methods
  - Baseline map    : Routes before optimization
  - 2-Opt map       : Optimized routes with color-coded hospitals
  - SA map          : Final routes after Simulated Annealing
  - Sensitivity chart: Cooling rate vs. solution quality


BY
------
Toqa Mohamed Fathy
Emergency Routing Optimization -- Ismailia, Egypt
Built with Python, OpenStreetMap, and NetworkX
