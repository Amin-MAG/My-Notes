# Ant Colony Quiz

Mohammad Amin Ghasvari 97521432 ✍️

---

# Job Scheduling problem

The job scheduling problem is an NP-hard problem. The problem consists of $n$ number of jobs and $m$ number of machines and we need to schedule them. The less time we spend to finished the job, the more score we gain.

In this problem "Make-span" is the time required to complete all of the jobs. Actually, make-span is the maximum amount of time between machines. For example, if machine 1 spends 200 seconds to complete the job, machine 2 spends 140 seconds, and the last one spends 50 seconds, then the make-span is the maximum of these amounts of time, 200 seconds.

## Ant Colony Optimization

ACO is one of the metaheuristics that is capable of solving this issue. Metaheuristic uses a standard approach but depends on random theory. The best answer spends less time to complete the job, so the objective is to find the minimum time.

The pheromone trail $T_{ij}$ on arc $(i, j)$ indicates the desirability to pick the job $j$ instantly after the job $i$. At each iteration $m$ ants are going to build the solutions concurrently. Also after each iteration pheromone evaporation will be applied.

The better the make-span time is for the solution constructed by a particular ant, the more pheromone there will be. We define a probability of taking job j after I that depends on the remaining pheromone and $\eta_j$. $\eta_j$ is the heuristic to the number of remaining jobs and their deadlines.

Probability of choosing j after i is $P=(\eta_j \times T_{ij})/(\sum_{l \in N} \eta_l \times T_{il})$

When the ants or the machines traverse then we can update pheromone in iterations. For choosing the next job, we should calculate the probability of choosing job $j$. I mean $argmax(T_{il})$ Where $l \in n$ or we can choose a random j for exploring.

We can save pheromone states in an $n \times n$ matrix. The summary would be 

- Generating $n$ number of ants for $n$ machines
- Iterate for each ant and do just like what i explained
- Apply the pheromone based on the condition of jobs and time. $\eta_j$ and $T_{ij}$
- After each iteration we should evaporate amount of pheromone