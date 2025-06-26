# Chapter 1: Pythonic Thinking

## Item 1: Know Which Version of Python You're using

Use Python 3
```bash
python --version
```
```bash
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

```
