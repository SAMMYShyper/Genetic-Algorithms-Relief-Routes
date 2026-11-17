# Relief Route Scheduling (Local Beam Search vs Genetic Algorithm)

## Project Overview
This project studies a **route optimization problem** inspired by emergency logistics after a major storm.  
A truck must:

1. Start at a depot  
2. Visit all delivery locations exactly once  
3. Return to the depot  

This is a **Traveling Salesperson Problem (TSP)-like** permutation optimization task, where the objective is to **minimize total travel distance**.

The project is implemented in **Jupyter Notebook (Python)** and compares:

- **Phase 1:** Local Beam Search  
- **Phase 2:** Genetic Algorithm (GA)

## Problem Context
The scenario models relief delivery to shelters/clinics/distribution points under urgency constraints.  
Locations are represented as 2D points, and travel cost is modeled with **Euclidean distance**.

### Key modeling assumption
The map is treated as a **fully connected graph**:
- Direct travel is possible between any two nodes
- Cost between nodes is calculated from coordinates

## Algorithms Covered

### 1) Local Beam Search
- Starts with `k` random routes
- Generates neighbors by swapping positions
- Keeps the best `k` routes each iteration
- Tracks diversity via average pairwise Hamming distance

**Observed issue:**  
Beam search can suffer from **beam collapse** (loss of diversity), causing plateau behavior and local optimum trapping.

---

### 2) Genetic Algorithm (GA)
The GA maintains a population and iteratively applies:

- **Selection** (roulette + tournament)
- **Crossover** (OX and PBX)
- **Mutation** (swap, inversion, scramble, plus custom mutation)
- **Elitism** and early stopping with patience

This phase evaluates whether GA can maintain diversity better and improve solution quality.

---

### Required baseline
Beam Search tested with:
- `k ∈ {4, 8, 16}`
- Multiple random seeds
- Metrics: best cost, runtime, evaluated candidates

### GA variant study
Grid search over:
- **Selection:** roulette, tournament
- **Crossover:** OX, PBX
- **Mutation:** swap, inversion, scramble, custom
- **Mutation rates:** `pm = 0.02`, `pm = 0.08`

Each configuration is run across multiple seeds and summarized with:
- Mean best cost
- Standard deviation of best cost
- Mean runtime (ms)
- Mean evaluations

## Main Findings (from current runs)
From the reported results in the notebook:

- Best GA variant by mean cost:  
  **GA[tournament](OX, inversion, pm=0.08)** (~27.46)

- Best beam baseline:  
  **Beam(k=16)** (~28.75)

- GA achieved better quality with far fewer evaluations and lower runtime in the shown runs, while also showing stronger stability in top variants.

---

## Tech Stack 

- **Language/Environment:** Python in Jupyter Notebook
- **Libraries:** `random`, `time`, `math`, `numpy`, `pandas`, `matplotlib`, `dataclasses`, `typing`
- **Techniques:** local search, evolutionary optimization, experiment design, reproducibility via seeded runs, metric-based algorithm comparison

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/SAMMYShyper/Genetic-Algorithms-Relief-Routes.git
   cd Genetic-Algorithms-Relief-Routes
   ```

2. Open Jupyter Notebook or other editor.

3. Run all notebook cells in order.

4. For full report-quality evaluation, use at least:
   - `algo_seeds = range(10)`
   - one fixed clustered instance seed
