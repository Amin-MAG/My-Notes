# Python Snippets

A snippet to generate a dictionary based on another dictionary in one line:

```python
>>> a = [1,2,3,4,4,5]
>>> a = [1,2,3,4,5]
>>> b = {"a": 100, "b":200, "c": 300, "d": 400}
>>> a
[1, 2, 3, 4, 5]
>>> b
{'a': 100, 'b': 200, 'c': 300, 'd': 400}
>>> { my_key: [x + my_value for x in range(10)] for my_key, my_value in b.items() }
{'a': [100, 101, 102, 103, 104, 105, 106, 107, 108, 109], 'b': [200, 201, 202, 203, 204, 205, 206, 207, 208, 209], 'c': [300, 301, 302, 303, 304, 305, 306, 307, 308, 309], 'd': [400, 401, 402, 403, 404, 405, 406, 407, 408, 409]}
```