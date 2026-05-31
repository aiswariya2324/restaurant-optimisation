# 🍽️ Restaurant Schedule Optimisation with Simulated Annealing

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://python.org)
[![Algorithm](https://img.shields.io/badge/Algorithm-Simulated%20Annealing-orange.svg)]()
[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A combinatorial optimisation project that builds an optimal 4-week restaurant dining schedule using **Simulated Annealing**, benchmarked against a greedy baseline. The dataset was personally collected — 20 real restaurants near the University of Stirling, with real costs, travel times, and personal ratings.

> **Key result:** SA produces a fully feasible schedule within the £150 budget while the greedy approach catastrophically overspends, demonstrating the practical value of metaheuristic search over naive heuristics.

---

## 📋 Table of Contents
- [Problem](#-problem)
- [Dataset](#-dataset)
- [Results](#-results)
- [Methodology](#-methodology)
- [How to Run](#-how-to-run)
- [Key Findings](#-key-findings)
- [Technologies](#-technologies)

---

## 🎯 Problem

**Decision variable:** A schedule of 12 restaurant visits over 4 weeks  
**Objective:** Maximise total dining satisfaction  
**Constraints:**
- Total spend ≤ £150
- No restaurant repeated within 3 consecutive visits
- Travel time ≤ 30 minutes from University of Stirling campus

**Why this is hard:** With 20 restaurants and 12 occasions, the search space is enormous. A greedy approach (always pick the highest-rated affordable option) repeatedly visits the same top restaurants, triggering a satisfaction decay penalty and eventually blowing the budget.

---

## 🗂 Dataset

**20 restaurants near University of Stirling** — personally collected data over 30–60 days.

| Field | Description |
|-------|-------------|
| Name | Restaurant name |
| Cuisine | Food type |
| Cost (£) | Average spend per visit |
| Travel (min) | Walking/bus time from campus |
| Base Rating | Personal enjoyment score (1–10) |

Restaurants include Chilos Burgers, Navarasa, Kazoza, SUMO, Hoi An Quan, Cosmo Buffet, Marble Global Buffet, Umami, and more — covering British, Indian, Japanese, Vietnamese, Korean, and International cuisines.

---

## 📊 Results

| Metric | SA Optimal | Greedy Baseline |
|--------|-----------|-----------------|
| **Total Satisfaction** | Higher | Lower (decay losses) |
| **Total Cost** | ✅ Within £150 | ❌ Overspends budget |
| **Feasible** | ✅ Yes | ❌ No |
| **Unique Cuisines** | 9 | Fewer (repetition) |
| **Feasible runs (15 seeds)** | 15/15 | N/A |

SA converges to the same optimal solution across all 15 independent random seeds — strong evidence of global optimality.

---

## ⚙️ Methodology

### Satisfaction Model
For visiting restaurant *i* on occasion *t* with visit history *H*:

```
S(i, t) = R_i × δ^|H_i| − α × C_i − β × T_i
```

- `R_i` = base rating
- `δ = 0.8` = satisfaction decay per repeat visit
- `α = 0.05` = cost penalty coefficient
- `β = 0.03` = travel penalty coefficient

### Simulated Annealing
- **Neighbour moves:** random swap of two occasions, random restaurant replacement, or swap for a cheap option
- **Acceptance:** Metropolis criterion — worse solutions accepted with probability `exp(Δf/T)`
- **Cooling:** geometric schedule, `T × 0.995` per iteration
- **Warm start:** random feasible schedule
- **Robustness:** 15 independent runs with different random seeds

### Greedy Baseline
At each occasion, picks the highest base-rated restaurant that fits the no-repeat constraint and remaining budget — ignoring decay, penalties, and future budget implications.

---

## 🚀 How to Run

**Google Colab (no setup needed):**
1. Open `part3_restaurant_optimisation.ipynb` in Colab
2. Runtime → Run all
3. All outputs generated inline — no external data files required (dataset is embedded in the notebook)

**Local:**
```bash
pip install numpy pandas matplotlib
jupyter notebook part3_restaurant_optimisation.ipynb
```

---

## 🔍 Key Findings

1. **Greedy fails on budget** — greedily picking top-rated restaurants (Kazoza £45, SUMO £32.50) exhausts the £150 budget in fewer than 12 visits. SA plans ahead.

2. **Decay makes diversity essential** — visiting the same restaurant even twice reduces satisfaction by 20% (δ=0.8). SA naturally discovers a diverse schedule to keep decay low.

3. **SA is robust** — all 15 seeds converge to the same feasible solution, suggesting the objective landscape is well-conditioned and the global optimum is reliably reachable.

4. **9 different cuisines** in the optimal schedule — British, Indian, Vietnamese, Japanese, International and more, reflecting realistic variety in a student's dining life.

5. **Limitations:** Ratings are subjective and personal; the decay factor is hand-tuned; the model doesn't account for meal deals, seasonal menus, or time-of-day pricing.

---

## 🛠 Technologies

`Python` · `NumPy` · `Pandas` · `Matplotlib` · `Simulated Annealing` · `Google Colab`

---

*University of Stirling — MATPMD4 Stochastic Processes & Optimisation, Assignment 2 Part 3*
