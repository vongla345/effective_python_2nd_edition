# Chapter 1: Pythonic Thinking

## Item 1: Know Which Version of Python You're using

Use Python 3
```bash
python --version
```
```python
import sys
print(sys.version_info)
print(sys.version)
```

## Item 2: Follow the PEP 8 Style Guide

### Whitespace
* 4 spaces for indent
* max 79 characters in 1 line
* 2 blank lines between ``class`` and ``def`` (top, bottom)
* methods in ``class`` shouble be seperated by 1 blank line
* no space before colon in dict: ``{key: value,}``
* space before and after '=': ``var = something``
* no space before colon and use a space before type: ``name: str``

### Naming
* Functions, variables and attributes in ``lowercase_underscore``
* Protected instance attributes in ``_leading_underscore``
* Private instance attributes in ``__double_leading_underscore``
* Class (including exceptions like ``ValueError, ..``) in ``CaptitalizeWord``
* Module-level constants in ``ALL_CAPS``
* instance methos in class use ``self``, which refers to the object, as the name of the 1st parameter: ``def name_function(self)``
* class methos use ``cls``, which refers to the class, as the name of the 1st parameter: ``def class_method(cls)``

### Expresions and Statements
* use ``if a is not b`` for negative comparision, not ``if not a is b``
* use ``if not somelist`` for checking if list is empty or not (not ``if (lensomelist) == 0``)
* use ``if somelist`` for checking if list is non-empty or not
* avoid 1 line for ``if``, ``for`` and ``while``, spread these over multiple line for clarity

### Imports
* Always put import statements (includung ``from x import y``) at the top of the file
* Always declare explicitly name of modules: ``from bar import foo`` not ``import foo``
* Import order:
    - Standard library modules (``sys``, ``os``, ...)
    - Third-party modules (``pandas``, ``numpy``, ...)
    - Your own modules

## Item 3: Know the Differences Between bytes and str
Distinguish between ``str`` and ``bytes`` in Python
* ``str``: contain Unicode code points that represent textual characters from human languages
* ``bytes``: raw, unsigend 8-bit value (often displayed in ACSII), use for computer and system

You can't operate between ``str`` and ``bytes``, these operation must be convert formerly (operators like ``>``, ``==``, ``+`` and ``%``)
```bash
>>> b'hello' + 'world'
TypeError: can't concat bytes to str
```
Convert first using encode() (for ``str`` to bytes) and decode() (reverse)
```bash
>>> b'hello' + 'world'.encode()
b'helloworld'
```
```bash
>>> b'hello'.decode() + 'world'
'helloworld'
```

When working with file, declare explitcily read/write text mode or read/write bianry mode to make sure there is no failure
* Reading text file
```python
with open('data.txt','r', encoding='utf-8') as f:
    content = f.read()
```
* Reading binary file
```python
with open('data.bin','rb') as f:
    content = f.read()
```

When encoding, make sure using the right UnicodeEncoder
```bash
>>> 'ñòóôõ'.encode('acsii')
UnicodeEncodeError: 'ascii' codec can't encode characters
```

## Item 4: Prefer Interpolated F-Strings Over C-style Format and ``str.format()``

Prefer using F-string than C-style and str.format() for:
* Readable
* Shorter
* More efficient

C-style
```python
name = "Nhan"
age = 22
print("My name is %s and I'm %d years old." % (name, age))
```
-> Oudated, many type which is not supported (``list``, ``tuple``, object, ...)

``str.format()``
```python
print("My name is {} and I'm {} years old.".format(name, age))
```
-> Easily getting confuse if there are many variables, hard to debug

F-string
```python
print(f"My name is {name} and I'm {age} years old.")
```

##  Item 5 – Write Helper Functions Instead of Complex Expressions
Instead of put all logic in 1 line, seperate it into specific functiono to:
* Readable
* Reuse
* Easily testing

Although Python's syntax can help us easily write single-line expresson, it can be overly complicated and difficult to read.
```python
value = int(user_input.strip().split(',')[1]) if ',' in user_input else 0
```
-> short, but hard to understand
-> user helper functions
```python
def parse_second_field(user_input: str) -> int:
    if ',' not in user_input:
        return 0
    parts = user_input.strip().split(',')
    return int(parts[1])

value = parse_second_field(user_input)
```
-> more readable

when coding, it is important to implement helper functions with:
* explicit name
* write more lines of code if it's needed to be more easily understand
* you can split into smaller funtions if it repeat the code in helper functions

## Item 6: Prefer Multiple Assignment Unpacking Over Indexing

When using tuple, it's easier to ``unpacking`` value in it by using Python's syntax rather than traditional way
* Normal way 
```python
user = ("Nhan", 22)
name = user[0]
age = user[1]
```
* Use ``unpacking`` in tuple
```python
user = ("Nhan", 22)
name, age = user
```
We can use ``unpacking`` for swap values
```python
#normal way
def bubble_sort(data):
    for _ in range(len(data)):
        for i in range(len(data)):
            if data[i] < data[i-1]:
                temp = data[i]
                data[i] = data[i-1]
                data[i-1] = temp
```
```python
def bubble_sort(data):
    for _ in range(len(data)):
        for i in range(len(data)):
            if data[i] < data[i-1]:
                data[i], data[i-1] = data[i-1], data[i] #unpacking
```
we can use _unpacking_ for iterate more readable
```python
items = [("Snacks", 100), ("Cola", 120)]
for index, (name, price) in enumerate(items): #easier to read than index-based loop
    print(f"The price of {name} is {price}")
```

## Item 7: Prefer enumerate Over range
Prefer using ``enemerate()`` when looping to get both index and item of given data (``list``, ``array``, ...)
``enumerate()`` will return a tuple containing both index and item, then using _unpacking_ to get these variables

```python
flavor_list = ['vanilla', 'chocolate', 'pecan', 'strawberry']

for index, flavor in enumerate(flavor_list):
    print(f'{i + 1}: {flavor}')
```

## Item 8: Use zip to Process Iterators in Parallel
Use ``zip`` when looping 2 list with they are related
```python
names = ['Nhan', 'Huy']
ages = [20, 22]

for name, age in zip(names, ages):
    print(f"Age of {name} is {age}")
```
``zip`` default will loop with the shortest length from 2 list, if you want longest length, use ``zip_longest`` from ``itertools`` built-in module.
```python
from itertools import zip_longest

names = ['Nhan', 'Huy', 'Anh']
ages = [22, 20]

for name, age in zip_longest(names, ages):
    print(name, age)

# Output: 
# Nhan 22
# Huy 20
# Anh None
```

## Item 9: Avoid else Blocks After for and while Loops

In Python, an ``else`` block after a ``for`` or ``while`` loop means that if the loop was completed, then execute the ``else`` block.
```python
lst = [1, 2, 3, 4, 5]

for i in lst:
    print(i)
else:
    print("Finish")

# Output
# 1
# 2
# 3
# 4
# 5
# Finish
```

if the loop isn't completed, then the ``else`` block won't execute
```python
num = 2
lst = [1, 2, 3, 4, 5]

for i in lst:
    if num == i:
        print("Found")
        break
    print(i)
else:
    print("Finish")

# Output
# 1
# Found
```

Avoid using an ``else`` block after a loop because it can be very confusing.

One use case is searching an object in data list, if the object exsits in data list, then the ``else`` block execute. 

## Item 10: Prevent Repetition with Assignment Expressions
In usual way, we have to assign variable and then check it with logic:
```python
value = get_data()

if value:
    print(value)
```

we can make it shorter by using walrus operator (``:=``)
```python
if (value := get_data()):
    print(value)
```

we can use walrus operator in loop
```python
while (line := file.readline()):
    print(line)
```

Avoid using walrus too much as it can make your code harder to read
```python
if (n := len(data)) > 10 and (a := avg(data)) < 5:
```