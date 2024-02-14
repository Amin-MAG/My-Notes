# Python

## Why to use Python

**Good sides of using Python:**
1. **Readability**: Python's syntax is  easy to understand, making it accessible to beginners and experienced programmers alike.
2. **Versatility**: Python is a multipurpose language with a vast ecosystem of libraries and frameworks that support a wide range of applications. Whether it's web development with frameworks like Django or Flask, data analysis with libraries like Pandas and NumPy, or machine learning with TensorFlow or PyTorch, Python has tools for almost any task.
3. **Large Standard Library**: Python comes with a comprehensive standard library that provides support for many common programming tasks, reducing the need for external dependencies.
4. **Community Support**
5. **Cross-platform Compatibility**
6. **Easy to Learn and Use**

**Bad sides of using Python:**

1. **Performance**: Python is often criticized for its performance compared to languages like C or C++. It is an interpreted language, which can lead to slower execution speeds, especially for CPU-intensive tasks. However, performance-critical parts of a program can often be optimized using libraries written in C or by utilizing just-in-time (JIT) compilers.
2. **Global Interpreter Lock (GIL)**: In multi-threaded applications, Python's Global Interpreter Lock can be a bottleneck, as it prevents multiple native threads from executing Python bytecode simultaneously. This can limit the scalability of certain types of applications, although there are workarounds such as multiprocessing or using asynchronous programming techniques.
3. **Mobile Development**
4. **Deployment Size**: Python applications typically require the Python interpreter to be installed on the target system, which can increase deployment size compared to compiled languages.

## Differenced Between Python 2 & 3

- **Print Statement vs. Function**
- **Unicode Support**
- **Division Operator**
- **Range vs. xrange**
- **Iterating Over Dictionaries**
- **`input()` vs. `raw_input()`**

## Data Types

1. int
2. float
3. complex
4. str
5. list
6. tuple
7. dict
8. set
9. frozenset
10. bool
11. NoneType
12. bytes
13. bytearray

## Init files in packages

All imports in `__init__.py` are made available when you import the package (directory) that contains it.

the code in `__init__.py`runs the first time you import any module from that directory. So it's normally a good place to put any package-level initialization code.

## Method order in a class

- **Interfaces first:** Public methods and Python magic functions define the interface of the class. Most of the time, you and other developers want to use a class rather than change it. Thus they will be interested in the interface of that class. Putting it first in the source code avoids scrolling through implementation details you don't care about.
- **Properties, [magic methods](https://rszalski.github.io/magicmethods/), public methods:** It's hard to define the best order between those three, which are all part of the interface of the class. As @EthanFurman says, it's most important to stick with one system for the whole project. Generally, people expect `__init__()` to be the best first function in the class so I follow up with the other magic methods right below.
- **Reading order:** Basically, there are two ways to tell a story: Bottom-up or top-down. By putting high-level functions first, a developer can get a rough understanding of the class by reading the first couple of lines. Otherwise, one would have to read the whole class in order to get an understanding of the class and most developers don't have the time for that. As a rule of thumb, put methods above all methods called from their body.
- **Class methods and static methods:** Usually, this is implied by the *reading order* explained above. Normal methods can call all methods times and thus come first. Class methods can only call class methods and static methods and come next. Static methods cannot call other methods of the class and come last.

## Testing

```python
import unittest

class TestSum(unittest.TestCase):

    def test_sum(self):
        self.assertEqual(sum([1, 2, 3]), 6, "Should be 6")

    def test_sum_tuple(self):
        self.assertEqual(sum((1, 2, 2)), 6, "Should be 6")

if __name__ == '__main__':
    unittest.main()
```

### Writing first test

```
project/
│
├── my_sum/
│   └── __init__.py
|
└── tests
```

Open up `my_sum/__init__.py` and create a new function called `sum()`, which takes an iterable (a list, tuple, or set) and adds the values together:

```python
def sum(arg):
    total = 0
    for val in arg:
        total += val
    return total
```

## Linter

To apply this linter you need to install the black linter.

```bash
pip3 install black
```

Then you can add this linter to your git hook.

```bash
# Enable the pre commit hook
cp ./git/hooks/pre-commit.sample ./git/hooks/pre-commit
```

Add these lines to the top of `pre-commit`:

```bash
# Linter
if command -v black
then
	black .
fi
```

## Itertools

With this library, you can for instance calculate the production of two list or get all of subset combinations of a list.

```python
import itertools

# Instead of 2 loops
for a, b in itertools.product(list_a, list_b):
	pass

# Consider the combinations with repeat, which means product
two_digit_numbers = list(itertools.product(list(range(10)), repeat=2))

# Calculate all combinations
for subset in itertools.combinations(list_a, 2):
	pass

```

It can perform more operations and these two are just some examples.

## Enumerate

You can write more cleaner code using `enumerate`. In case you need the index of an iterm in a list

```python
for i, item in enumerate(items):
	pass
```

## Breakpoint

You can use `breackpoint()` and then you kind of enter to the shell mode.

```python
n # for next
pp v # to print a variable ?
c # continue
```

## Write a decorator

```python
def do_twice(func) {
	def wrapper_do_twice():
		func()
		func()
	return wrapper_do_twice()
}
```

# Socket

```python
import socket

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect(("challs.dvc.tf", 6666))
    # Now read from this connection
    data = s.receive(1024)
    # Or send something
    s.sendall(bytes("1 1", "utf-8"))
```


# Snippets

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

To unzip a file in the python with the password

```python
from zipfile import ZipFile

name = "37366"
while True:
    with ZipFile(f'{name}.zip') as zf:
        name = zf.namelist()[0][:-4]
        zf.extractall(pwd=name.encode())
```

## Create instant web server

```bash
python3 -m http.server 7600
```

## Byte to string and hex

```python
os = ''.join(map(chr, o))
oo = bytearray.fromhex(os)
```

# See more

- [Django](Django.md)
- [ClickHouse](ClickHouse.md)
- [Pandas](Pandas.md)
- [Telnetlib](Telnetlib.md)
- [Hashlib](Hashlib.md)
- [FastAPI](Python/FastAPI.md)
- [Matplotlib](Python/Matplotlib.md)
- [Scapy](Scapy.md)
- [Bcrypt](Python/Bcrypt.md)
