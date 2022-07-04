# Matplotlib

Plot a simple diagram.

```py
plt.plot([1,2,3,4,5], [4, 2, 4, 2, 4])
plt.ylabel('funny numbers')
plt.show()

```

You can plot the diagram in other forms, like red circles.

```py
plt.plot([1,2,3,4,5], [4, 2, 4, 2, 4], 'ro')
plt.ylabel('funny numbers')
plt.show()
```

You can use NumPy to plot some equations.

```py
t = np.arange(0, 5, 0.2)
plt.plot(t, t**2, 'r--', t, t, 'bs')
```

## Scatter Plot

```py
plt.scatter(data['a'], data['b'])
```
