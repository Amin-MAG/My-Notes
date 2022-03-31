# Genetic Algorithm

1. Design the chromosome.
2. Initialize the parents until it fulfills the $PopulationSize$.
3. Calculate the fitness for each one of these parents.
4. Sort the parents based on their fitness.
5. Choose 2 random parent with a $SelectEquation$.
6. Creating 2 new children.
    1. Generate new random crossover point, let's call this $x$.
    2. The first child will have the first $x$ elements of parent one combines with other elements of parent two.
    3. The second child will have the first $x$ elements of parent two combines with other elements of parent one.
    4. These children now have $MutationChance$ percent chance of being muted.
7. Reproduction until fulfill the their original $PopulationSize$.

# Design the chromosome

There is a knight piece and we can start from each one of 8 * 8 places on the chessboard. The condition is to take a tour and visit each one of these places once and only once. The knight tour problem is a famous problem and it could have many solutions to satisfy the condition.

We use a List $A$ with the size of at most 64, which $A_i$ represents the movement in time $i$.

At most eight kind of movements are possible in each situation. Let's name them:

$e_1 = \begin{pmatrix}
-1 & 2
\end{pmatrix}$

$e_2 = \begin{pmatrix}
1 & 2
\end{pmatrix}$

$e_3 = \begin{pmatrix}
2 & 1
\end{pmatrix}$

$e_4 = \begin{pmatrix}
2 & -1
\end{pmatrix}$

$e_5 = ...$

$e_6 = ...$

$e_7 = ...$

$e_8 = \begin{pmatrix}
-2 & 1
\end{pmatrix}$

For example if we were in (0, 0) in chessboard, some movements are impossible.

So $A$ is something like $A = \begin{pmatrix}
4 & 5 & 7 & 5 & ... & 8
\end{pmatrix}$.

# Initialize the parents

We should initialize $PopulationSize$ = 400 valid sequences.

# Calculate the fitness

We should provide a function to calculate the fitness. At the moment, with this kind of chromosome, we satisfy the movement condition of the knight. We should calculate the biggest number of different places that the knight can visit and no place visited twice. 

# Sort the parents

So after calculating, we should sort the parent based on their fitness. We should choose an equation for our selection and I think this kind of function is good.

![Genetic%20Al%20738b8/Untitled.png](Computational%20Intelligence/Genetic%20Al/Untitled.png)

# Choose 2 random parent

Randomly choose two of these parents using $SelectEquation = 60x^5$ so appropriate parents would be selected.

# Creating 2 new children

Generate random number $k$ between zero and minimum size of theirs parents.  Then the first child would be $parent_1[:k] + parent_2[k+1:]$ and the second child would be $parent_2[:k] + parent_1[k+1:]$.

Each one of these children has the chance of $MutationChance=0.5$ to mutate.

# Reproduction

We continue to produce children until they fulfill the $PopulationSize$. 

This Should be happened for the next generations.