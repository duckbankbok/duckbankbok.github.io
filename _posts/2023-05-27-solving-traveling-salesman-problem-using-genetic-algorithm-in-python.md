---
title: Solving Traveling Salesman Problem (TSP) using Genetic Algorithm (GA) in Python
date: 2023-05-28 14:00:00 +0900
categories: [OR, TSP]
tags: [or, tsp, ga, python]     # TAG names should always be lowercase
math: true
---

**Genetic Algorithm** is a heuristic algorithm widely used in solving combinatorial optimization problems. One such problem is the **Traveling Salesman Problem** (TSP), which asks for the shortest route a salesman can take to visit a given set of cities and return to the starting point, without revisiting any city. TSP is known to be NP-hard, making it computationally hard to find an optimal solution.

However, with the help of GA, we can tackle TSP effectively. GA mimics the process of natural evolution by employing the concepts of selection, crossover, and mutation to generate new solutions iteratively. By gradually improving the solutions over generations, GA converges towards finding near-optimal or even optimal solution.

In this blog post, we will explore how to implement TSP using GA in Python. We will break down the steps involved, explain the key components of GA, and demonstrate its application in solving TSP.

## Genetic Algorithm

### Solution Representation

GA needs solution representation specially designed to the problem. We will use permutation representation comprised with a set of distinct nodes. To illustrate this, let's consider a TSP instance with five cities: A, B, C, D, and E. A possible permutation representation for this problem could be [A, C, E, B, D]. This permutation indicates that the salesman should visit city A first, then move to city C, followed by city E, B, and finally D, before returning to the starting city. We will solve TSP with 20 nodes, so the sample solution is as follows:

![solution representation](https://github.com/duckbankbok/duckbankbok.github.io/assets/64826387/ade44bdd-b7a7-457d-931e-51d64ecf66dc)

### Algorithm Flow Chart

GA model for the problem is structured with the following flow chart.

![ga flow chart](https://github.com/duckbankbok/duckbankbok.github.io/assets/64826387/d214ab06-d0c8-401b-9d87-fca24bd50025){: width="50%"}

#### Initialization

The initial population is constructed using random permutations.

#### Calculate Fitness

Fitness (the objective value, which is the Euclidean distance of the solution) of all solutions in the population is calculated.

#### Elitism

Based on their fitness, GA selects parents who will be directly carried over to the next generation.

#### Selection, Crossover, and Mutation Operators

Offsprings are created using selection, crossover, and mutation operators.

- **Binary Tournament Selection**

Binary Tournament Selection is the process randomly selecting two individuals from the population and comparing their fitness values. In GA, Binary Tournament Selection is used to choose the two parents who will pass on their genetic material to the offspring of the next generation.

- **One Point Crossover**

![one point crossover](https://github.com/duckbankbok/duckbankbok.github.io/assets/64826387/89cefec8-1967-4b35-8b4d-529f9dbd77b1)

In One Point Crossover, two parents are selected from the population. A random crossover point, typically a specific index position, is chosen along the solution sequence of the first parent. In general, the genetic material beyond this crossover point is exchanged between the parents. But in this problem, every node should appear only once in permutation. Therefore, the remaining part of the permutation is filled based on the permutation of the second parent.

- **Swap Mutation**

![swap mutation](https://github.com/duckbankbok/duckbankbok.github.io/assets/64826387/7fe6cb6d-b095-4e31-a4ca-e4a412da2cb3)

In Swap Mutation, two randomly selected nodes are swapped.

The crossover operator and mutation operator are applied when a randomly generated number between 0 and 1 is less than or equal to $p_{crossover}$ and $p_{mutation}$, respectively.

### Python Code

```python
import numpy as np
import random
import matplotlib.pyplot as plt
from tqdm import tqdm
```

```python
# node / [x coordinate, y coordinate]
node = np.random.randint(101, size=(20,2))

for i in node:
    plt.scatter(i[0], i[1], c="b")
plt.axis([-5, 105, -5, 105])
plt.show()
```

![ga sample](https://github.com/duckbankbok/duckbankbok.github.io/assets/64826387/4d07f9b7-a507-4a10-aa19-b89df27d2fbc)

```python
# build distance matrix
distance_matrix = np.zeros((len(node),len(node)))

for i in range(len(node)):
    for j in range(len(node)):
        distance_matrix[i][j] = np.linalg.norm(np.array((node[i][0],node[i][1]))-np.array((node[j][0],node[j][1])))
```

```python
class GeneticAlgorithm:
    def __init__(self):
        self.population = []
        self.parent = []
        self.best = None
        self.best_log = []
        self.best_score_log = []
    
    def initialize(self, n_population, n_nodes):
        self.population = [np.random.permutation(n_nodes) for _ in range(n_population)]

    def get_objective_value(self, solution): # calculate fitness
        objective = 0
        distance_list = [distance_matrix[solution[idx]][solution[idx+1]] for idx in range(len(solution)-1)]
        objective = sum(distance_list) + distance_matrix[solution[-1]][solution[0]]

        return objective

    def elitism(self, n_parent):
        self.parent = []
        fitness_population = {}
        for solution in self.population:
            fitness = self.get_objective_value(solution)
            fitness_population[fitness] = solution
        sorted_dict = sorted(fitness_population.items())

        self.best = sorted_dict[0][1]
        best_fitness = self.get_objective_value(self.best)
        self.best_log.append(self.best)
        self.best_score_log.append(best_fitness)
        
        for sol_tuple in sorted_dict[:n_parent]:
            self.parent.append(sol_tuple[1])

    def tournament_selection(self, n_tournament):
        participants = random.sample(self.population, n_tournament)
        fitness_population = {}
        for solution in participants:
            fitness = self.get_objective_value(solution)
            fitness_population[fitness] = solution
        sorted_dict = sorted(fitness_population.items())
        winner = sorted_dict[0][1]

        return winner

    def crossover(self, solution1, solution2):
        sol1 = np.copy(solution1)
        sol2 = np.copy(solution2)
        
        idx = np.random.randint(len(sol1))

        for node in sol1[:idx]:
            sol2 = sol2[sol2 != node]
        
        solution = np.concatenate((sol1[:idx],sol2))

        return solution
    
    def mutation(self, solution):
        mutated_solution = np.copy(solution)
        idx1 = np.random.randint(len(mutated_solution))
        idx2 = np.random.randint(len(mutated_solution))

        while idx1 == idx2:
            idx2 = np.random.randint(len(mutated_solution))
        
        mutated_solution[[idx1, idx2]] = mutated_solution[[idx2, idx1]]

        return mutated_solution
    
    def genetic_algorithm(self, n_population, n_parent, n_nodes, n_tournament, p_crossover, p_mutation, max_generation):
        self.initialize(n_population, n_nodes)
        for _ in tqdm(range(max_generation)):
            self.elitism(n_parent)
            population = []

            while len(population) != n_population - len(self.parent):
                parent1 = self.tournament_selection(n_tournament)
                parent2 = self.tournament_selection(n_tournament)

                if p_crossover >= np.random.rand(1)[0]:
                    offspring = self.crossover(parent1, parent2)
                else:
                    random_parent = [parent1, parent2]
                    offspring = random_parent[np.random.randint(len(random_parent))]
                
                if p_mutation >= np.random.rand(1)[0]:
                    offspring = self.mutation(offspring)

                population.append(offspring)
            
            population += self.parent
            self.population = population

        self.elitism(n_parent)
```

```python
ga = GeneticAlgorithm()
ga.genetic_algorithm(100, 10, 20, 2, 0.9, 0.1, 50)
```

#### Result

Following gif shows the best solution of each generation.

![ga result](https://github.com/duckbankbok/duckbankbok.github.io/assets/64826387/55b4ae63-da0b-41c4-8794-078a6f6045dd){: width="75%"}

#### Appendix: Draw Route of the Solution

```python
def draw_route(solution):
    for i in node:
        plt.scatter(i[0], i[1], c="b")
    for idx in range(len(solution)-1):
        plt.plot([node[solution[idx]][0],node[solution[idx+1]][0]], [node[solution[idx]][1],node[solution[idx+1]][1]], c="green")
    plt.plot([node[solution[-1]][0],node[solution[0]][0]], [node[solution[-1]][1],node[solution[0]][1]], c="green")
    plt.axis([-5, 105, -5, 105])
    plt.show()
```