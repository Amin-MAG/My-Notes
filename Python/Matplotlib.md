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

## Create 3D plots

```python
from mpl_toolkits import mplot3d
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt

ax = plt.axes(projection='3d')

# Data for a three-dimensional line
zline = np.linspace(0, 15, 1000)
xline = np.sin(zline)
yline = np.cos(zline)
ax.plot3D(xline, yline, zline, 'gray')

# Data for three-dimensional scattered points
zdata = 15 * np.random.random(100)
xdata = np.sin(zdata) + 0.1 * np.random.randn(100)
ydata = np.cos(zdata) + 0.1 * np.random.randn(100)
ax.scatter3D(xdata, ydata, zdata, c=zdata, cmap='Greens');
```

# References
- [3D plots in matplotlib](https://jakevdp.github.io/PythonDataScienceHandbook/04.12-three-dimensional-plotting.html)