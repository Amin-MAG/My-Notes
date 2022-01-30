# HW04 Assignment

✍️ Mohammad Amin Ghasvari 97521432

# Question 1

It is going to be an LL1 if 

- It doesn't have any left recursions.
- It doesn't have any left prefix.
- It doesn't have any null.

Look at this conditional Expression part

```go
conditionalExpression
  :   '(' conditionalExpression ')'
  |   '!' conditionalExpression
  |   conditionalExpression '<' conditionalExpression
  |   conditionalExpression '&&' conditionalExpression
  |   expression
;
```

In here we have left recursion so that it isn't an LL1.