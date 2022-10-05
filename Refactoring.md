# Refactorings

## Composing Methods

### Extract Method

**Problem**: You have a code fragment that can be grouped together.
**Solution**: Move the code fragment to a new method or function.

- More lines in a method make it harder to understand the method.
- Less code duplication
- Isolate independent parts of the code

### Inline Method

**Problem**: The method body is more obvious than the method.
**Solution**: Remove the method.

- The code will be more straightforward.

#### Drawbacks

> What about a case where the body code is used multiple times?

### Extract Variable

**Problem**: The expression is hard to understand.
**Solution**: Place the expression or its parts in separate variables.

- The complex expression becomes more understandable. Especially in `if` statements.
- It avoids long and multiple lines in the code.

> Extracting a variable may be the first step towards performing Extract Method if you see that the extracted expression is used in other places in your code.

#### Drawbacks

- It causes more variables.
- If these separate parts do heavyweight work, This can cause performance issue. We need to calculate all parts of the expression even when we know the result of the expression (e.g., in a `if (A || B)` statement)

### Inline Temp

**Problem**: A variable is assigned to an easy expression.
**Solution**: Remove the variable.

- Less variable usage.

#### Drawbacks

- Sometimes these variables cache the result of an expensive operation!

## Moving features between objects

### Move Method

**Problem**: A method is more used in another class than in its own class.
**Solution**: Move the method.

## Organizing Data

## Simplifying Conditional Expressions

### Consolidate Conditional Expression

**Problem**: Multiple condition expressions that lead to same result.
**Solution**: Move all of them into a single condition expression.

- It eliminates duplicated control flow code.
- The complex condition expression can be handled in a separate method.

### Consolidate Duplicate Conditional Fragments

**Problem**: Identical code lines between all branches of a condition.
**Solution**: Move the code lines outside the condition block.

- It brings about less code duplication.

### Decompose Conditional

**Problem**: Complex condition expression
**Solution**: Decompose the complex expression into a separate method.

- It leads to more readable and maintainable code.
- The condition expressions are much shorter.

## Simplifying Method Calls

### Add Parameter

### Remove Parameter

### Rename Method

## Dealing with Generalization

### Pull Up Field

**Problem**: Two classes have the same field that can be generalized.
**Solution**: Move the field to the super class.

- It eliminates duplication of fields.

### Pull Up Method

**Problem**: Two classes have the same method that can be generalized.
**Solution**: Move the method to the super class.

- It eliminates duplication of methods.

### Pull Up Constructor

**Problem**: Two subclasses have the code mostly like each other.
**Solution**: Move the same code to the superclass constructor.

- It eliminates duplication of methods.

### Push Down Field

**Problem**: A field in superclass is only used in few subclasses.
**Solution**: Move the field to the subclasses.

- Improves the internal class coherency.

### Push Down Method

**Problem**: A method in superclass is only used in few subclasses.
**Solution**: Move the method to the subclasses.

- Improves the internal class coherency.


# See More

- [Refactoring in Go](Golang/GoRefactorings.md)

# Resources

- [Refactoring gru](https://refactoring.guru/)