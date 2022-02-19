# Compiler

# First

Let's assume we want to calculate the first of $X$. So 

## $X$  → $x$

If it is a terminal like x then the first of $X$ is going to be {"$x$"}.

## $X$ → $\lambda$

If $X$ → $\lambda$ then we should have lambda in our list {"$\lambda$"}

## $X$ → $Y_1Y_2Y_3...Y_n$

If $x$ → $Y_1Y_2Y_3...Y_n$ then we should calculate the first of $Y_1$. If the first of $Y_1$ doesn't contain $\lambda$ then the first of the $X$ is the first of $Y_1$. 

What if it contains $\lambda$? 
Because $Y_1$ might be $\lambda$, then we should consider that possibility too. Then, we need to calculate the first of $Y_2$ too. So,

$First(X) = \{ First(Y_1) - \lambda \} \cup First(Y_2)$ 

And again here is a possibility that $\lambda$ would be in first of $Y_2$, then we continue as we did. If $\lambda$ exists in each of them from $Y_1$ to $Y_n$ , you should add the $\lambda$ to the first set of $X$.

# Follow

It is the set of terminals that comes immediately right of a non-terminal $X$.

## $S$ as the starting non-terminal

Follow of $S$ (the starting non-terminal) is {"$\$$"}.

## $A$ → $pB$

Follow $B$ when $A$ → $pB$ is the follow of the $A$. Because there is nothing after $B$, so everything in the $A$ follow set is at the right of $B$ . This means that $Follow(A)=Follow(B)$.

## $A$ → $pBq$

Follow $B$ when $A$ → $pBq$  is the first of the $q$ if it doesn't contain $\lambda$.

$Follow(B)=\{First(q) - \lambda \}$

What about the situation that it contains $\lambda$?

If it is $\lambda$ so we got to the second situation that we didn't have any q's so then we should calculate the Follow of the non-terminal. ($A$ → $pB$)

$Follow(B)=\{First(q) - \lambda \} \cup Follow(A)$

<aside>
💡 The Follow set should not include $\lambda$.

</aside>

# LL1

Grammars should keep to these rules

1. Shouldn't have left prefix. 
2. Shouldn't have left recursions. The First shouldn't have anything in common.
3. Shouldn't have null productions, if they cause problems

To transfer a grammar to LL1

## Left Factoring

Just by factoring and adding new non-terminal we can avoid in common items in the first sets.

## Null production removal

We will remove all of the $\lambda$'s by expanding the grammar rules.

## Left recursion elimination

For example, $X$ → $X\alpha$ | $\beta$ has left recursion and it is not LL1. To convert it to LL1 we need to add a non-terminal just like this

$X$ → $\beta X'$

$X'$ → $\alpha X'$ | $\lambda$

We kind of change the left recursion to the right recursion that resolves the issue.

# Top-Down Parsing

![Compiler%20aae9c/Untitled.png](Compiler%20aae9c/Untitled.png)

[Results](Compiler%20aae9c/Results%208364e.md)

[HW04 Assignment](Compiler%20aae9c/HW04%20Assig%20f2f42.md)

[Compiler Extra Project](Compiler%20aae9c/Compiler%20E%2088001.md)