# Genetic Programming

**M.Amin Ghasvari** `97521432`

# Overview

![It is the flow chart from [https://www.cs.ucdavis.edu/~vemuri/classes/ecs271/The GP Tutorial.htm](https://www.cs.ucdavis.edu/~vemuri/classes/ecs271/The%20GP%20Tutorial.htm). These notes are not for quizzes, but also for reviewing the course.](Genetic%20Programming%20dc605d5cdfc54540b8bc443b838b4940/Untitled.png)

It is the flow chart from [https://www.cs.ucdavis.edu/~vemuri/classes/ecs271/The GP Tutorial.htm](https://www.cs.ucdavis.edu/~vemuri/classes/ecs271/The%20GP%20Tutorial.htm). These notes are not for quizzes, but also for reviewing the course.

1. Initialize the $population$.
2. Determine what the $fitness$ is for each one of program.
3. Then reproduce the next generation by
    1. Crossover: select two programs based on fitness and then swap specific child of nodes
    2. Mutation: select one program based on fitness and generate a random tree and replace it with specific child of tree
    3. Replication or Reproduction: select one program based on fitness and then copy it to next generation
4. After reproduction and when we reach $production$ then we repeat step 3 to produce $generation + 1$.

![The selection of those three operation is a part of out process.](Genetic%20Programming%20dc605d5cdfc54540b8bc443b838b4940/Untitled%201.png)

The selection of those three operation is a part of out process.

---

# In-Class Quiz

![Genetic%20Programming%20dc605d5cdfc54540b8bc443b838b4940/Untitled%202.png](Genetic%20Programming%20dc605d5cdfc54540b8bc443b838b4940/Untitled%202.png)

We should consider both time and distance to reach the destination in our fitness function. We have 6 kinds of movements and let's assume that each one of them is such a function as $F_i(amount, currentTime)$ that gives us a tuple like $(position, time)$. $amount$ shows the amount of movement. For example, spinning with $amount = 30\%$ means to spin 30% of the movement.

Also we can add some leaves to put the current time and current position in them, but for now we do not write them.

## Initialize the programs

![Genetic%20Programming%20dc605d5cdfc54540b8bc443b838b4940/Untitled%203.png](Genetic%20Programming%20dc605d5cdfc54540b8bc443b838b4940/Untitled%203.png)

We initialize the tree like this. Since we have six kinds of movements we name them A, B, C, D, E, F. The post movement is the right hand of a tree node and it is a movement again, and the left hand is like this too. So the data structure of blue borders is something like $MovementNode$ and the amount of movement is just an integer number between 0 and 100.

 We consider the $population = 10000$ and generates these kinds of trees. 

> **Note: I did not add time and current position as leaves in this picture.**
> 

## Determine the fitness

So here we should specify the fitness function. If a program had a collision with something we drop that program. Since we should consider both time and distance $f= 1 / (currentTime \times distance)$ looks good. When the robot has a collision with something for the first time we calculate this $f$ to represent its fitness. $currentTime$ is the sum of all seconds until that moment. $distance = manhatan(currentPositions, destination)$.

Here we defined the fitness and now we need to calculate fitness for all of the programs.

  

## Produce next generation

We reproduce the new generation here. We make a decision which operation to choose. I choose $Crossover=70 \%$, $Mutation = 18 \%$, $Replication = 12 \%$. Then, we select one or two programs based on which operation is taken and the fitness and then produce the new program(s). We continue to fulfill the $population$ amount. Consider that the tree traversal is pre-procedural. It means that the current path is the path of the left child + this movement + the path of the right child.

So

- Replication: Some of them are just copied to next generation
- Mutation: A range of movements for some of them are completely changed.
- Crossover: A range of movements for two program will be swapped.

We should consider the depth of this tree. We write $maxDepth$ as the maximum depth. If this reproduction excess then we do not continue to add.

## End of the process

We should specify the final $generation$ to end this process. I think a solution is to check the count of the programs that reach the destination and check if they reached the $statisfiedAmount$ amount. For example, at the end of producing generations, we calculate the percentage of programs that reach the destination, and if the are $amount < staisfiedAmount$ then we continue the production and if the error is less than an appropriate amount then we terminate the process.