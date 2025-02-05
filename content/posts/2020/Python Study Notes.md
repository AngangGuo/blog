---
title: "Notes - Python"
date: 2020-05-13T22:21:22-07:00
categories:
 - Tech
 - Programming
tags:
 - Python
draft: false
---


## 1. Intro

Python Crash Course 2E Study Notes

### Interpreter
```
C:\Users\Andrew>python
Python 3.8.2 (tags/v3.8.2:7b3ab59, Feb 25 2020, 23:03:10) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

### Tools
* https://repl.it/languages/python3
* https://www.jetbrains.com/pycharm/

### Run Code
```
python hello.py
```

## 2. Variable and Simple Data Types
### Naming and Variables
* Start with string or underscore
* Can use string, underscore, and number thereafter

### String
```
# String literals in python are surrounded by either single quotation marks, 
# or double quotation marks.
"Hello"
'World'
```
### String Methods
```
s="hello, World"
s.upper()
s.lower()
s.title()
```

### `f` String
```
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
message = f"Hello, {full_name.title()}!"
print(message)
```

### Tabs and Newlines
```
>>> Print("Hello, Eric's wife!")
Hello, Eric's wife!
>>> print("\tHello,\n\tWorld!")
    Hello,
    World!

```
### Stripping Space
```
>> s=" Andrew "
>> s.rstrip()
' Andrew'
>> s.lstrip()
'Andrew '
>> s.strip()
'Andrew'
```

## Number

### Interger
```
1+2
3-1
4*5
7/8 # 0.875
9**3 # 729
(8+9)*3 # 51
```

### Float
* Python defaults to a float in any operation that uses a float, even if the output is a whole number.
* you can group digits using underscores to make large numbers more readable

```
0.1*2 # 0.2
0.1+0.1 # 0.2

# arbitrary number of decimal may places in your result
3*0.1 # 0.30000000000000004
0.1+0.2 # 0.30000000000000004

# Divide any two integer get float
4/2 # 2.0
# Mix integer and float: get float
3+1.0 # 4.0

# Multiple assignment
x, y, z = 0, 0, 0
```

### Constants
* No built-in constant types
* Convention: Use all capital letters
```
MAX_CONNECTS = 20
```

### Comments
Use `#` to comment code. Anything after `#` are ignored.

### Zen Of Python
```
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

## 3. List

### Assign and Access
```
>>> colors = ['red', 'green', 'blue', 'black', 'white']
>>> print(colors)
['red', 'green', 'blue', 'black', 'white']
>>> print(colors[3])
black
>>> print(colors[-2])
black
>>> len(colors)
5
```

### Modify, Add, Remove
```
# Modify
>>> colors = ['red', 'green', 'blue', 'black', 'white']
>>> colors[0] = 'amber'
>>> print(colors)
['amber', 'green', 'blue', 'black', 'white']

# Add
>>> colors.append('almond')
>>> print(colors)
['amber', 'green', 'blue', 'black', 'white', 'almond']
>>> colors.insert(1, 'red')
>>> print(colors)
['amber', 'red', 'green', 'blue', 'black', 'white', 'almond']
>>> colors.insert(-2, 'amazon')
>>> print(colors)
['amber', 'red', 'green', 'blue', 'black', 'amazon', 'white', 'almond']

# Delete and discard
>>> del colors[0]
>>> print(colors)
['red', 'green', 'blue', 'black', 'amazon', 'white', 'almond']

# Delete and Keep the value
# Last
>>> color = colors.pop()
>>> print(color)
almond
>>> print(colors)
['red', 'green', 'blue', 'black', 'amazon', 'white']
# Third
>>> color = colors.pop(2)
>>> print(color)
blue
>>> print(colors)
['red', 'green', 'black', 'amazon', 'white']
# By value
# Note: The remove() method deletes only the first occurrence of the value you specified.
>>> colors.remove('amazon')
>>> print(colors)
['red', 'green', 'black', 'white']
```

### Pitfall

If you change line A and line B, 
```
# Correct
guests = ["Andrew", "Kevin"]
del guests[1] # A
del guests[0] # B
print(guests)
# output: []
============================
# Wrong:
guests = ["Andrew", "Kevin"]
del guests[0] # B
del guests[1] # A
print(guests)

Traceback (most recent call last):
  File "C:/Users/Andrew/PycharmProjects/pycrash/ch3/3-7.shrink.py", line 26, in <module>
    del guests[1]
IndexError: list assignment index out of range

# Correction => change line A as: del guests[0]
```

### Sort

```
# Permenant change the list
>>> print(colors)
['red', 'green', 'black', 'white']
>>> colors.sort()
>>> print(colors)
['black', 'green', 'red', 'white']
>>> colors.sort(reverse=True)
>>> print(colors)
['white', 'red', 'green', 'black']

# Temporary: just for display, the original list order doesn't change
>>> print(sorted(colors))
['black', 'green', 'red', 'white']
>>> print(colors)
['white', 'red', 'green', 'black']

# Reverse
>>> colors.reverse()
>>> print(colors)
['black', 'green', 'red', 'white']

# Reverse doesn't sort the list, just reserse the order
# -----------------------------------------------------
>>> colors.insert(2, 'amazon')
>>> print(colors)
['black', 'green', 'amazon', 'red', 'white']
>>> colors.reverse()
>>> print(colors)
['white', 'red', 'amazon', 'green', 'black']
```

### Index Errors
```
>>> print(colors)
['white', 'red', 'amazon', 'green', 'black']
>>> print(colors[8])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
>>> print(colors(-5))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'list' object is not callable
>>> print(colors[-5])
white
>>> print(colors[-6])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

## 4. Working With Lists

### For Loop
`for item in items:`

```
# 1. Simple for loop: end with colon
colors = ["red", "blue", "yellow"]
for color in colors:
    print(color)
# output
red
blue
yellow

# 2. Doing sth after for loop: indent is important
colors = ["red", "blue", "yellow"]
for color in colors:
    print(f"I like {color.upper()} flower.")
print("done.")

# output
I like RED flower.
I like BLUE flower.
I like YELLOW flower.
done.

# 3. Indent error
# 3-1
colors = ["red", "blue", "yellow"]
for color in colors:
print(f"I like {color.upper()} flower.")

# output
  File "C:/Users/Andrew/loop.py", line 3
    print(f"I like {color.upper()} flower.")
    ^
IndentationError: expected an indented block

# 3-2
msg="hello"
    print(msg)

# output
  File "C:/Users/Andrew/scratch.py", line 6
    print(msg)
    ^
IndentationError: unexpected indent
```

### range() Function
```
# range(start, stop)
for v in range(1,4):
...     print(v)
...     
1
2
3

# single parameter - starting from 0: range(stop)
for v in range(3):
...     print(v)
...     
0
1
2

# list()
even = list(range(2, 11, 2))
print(even)
[2, 4, 6, 8, 10]
```
### Statistics functions
```
digits=list(range(11))
print(digits)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(min(digits))
0
print(max(digits))
10
print(sum(digits))
55
```

### List Comprehensions
```
v=[value**2 for value in range(2,9,2)]
print(v)
[4, 16, 36, 64]

# Normal
v = []
for value in range(2,9,2):
     v.append(value**2)
print(v)
[4, 16, 36, 64]
```


## 6. Dictionaries

As of Python 3.7, dictionaries retain the order in which they were defined. 
When you print a dictionary or loop through its elements, you will see the 
elements in the same order in which they were added to the dictionary.


```
# simple directory
>>> student = {"name": "Andrew", "age": 12}
# access the value with the key
>>> print(student["name"])
Andrew

# adding new key-value pairs
>>> student["grade"] = 5
>>> print(student)
{'name': 'Andrew', 'age': 12, 'grade': 5}


# starting with an empty dictionary
>>> person = {}
>>> person["first name"] = "Andrew"
>>> person["age"] = 22
>>> print(person)
{'first name': 'Andrew', 'age': 22}

# modify the value
>>> person["age"] = 33
>>> print(person)
{'first name': 'Andrew', 'age': 33}

# remove key-value pairs
>>> del person["age"]
>>> print(person)
{'first name': 'Andrew'}

# store many objects
>>> marks = {
... "Andrew": 89,
... "Lisa": 77,
... "Ace": 99,
... }
>>> print(marks["Ace"])
99

# Error when no key-value pair found
>>> print(marks["ace"])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'ace'

# using get() to access values. Default: None
>>> marks.get("ace") # show nothing
>>> marks.get("ace") == ""
False
>>> marks.get("ace") == None
True

# or return the optional second parameter
>>> marks.get("ace", "Nothing") == "Nothing"
True
>>> marks.get("ace", "Nothing")
'Nothing'

```

### Loop Through Dictionaries
```
# use items() to get the key, value pair
for k, v in my_dict.items():

# loop through keys
for key in my_dict.keys():

# it's the default behaviour of looping through dictionary by keys
for key in my_dict:

# loop through values
for v in my_dict.values():

# A set is a collection in which each item must be unique
# set - 1
>>> my_set = {'python', 'c', 'go', 'python', 'java'}
>>> print(my_set)
{'java', 'python', 'c', 'go'}
# set - 2 
for language in set(favorite_languages.values()):

# return keys as list
if 'erin' not in favorite_languages.keys():

# loop in sorted order
for name in sorted(favorite_languages.keys()):


>>> print(marks)
{'a': 3, 'b': 4, 'c': 1, 'u': 9}
>>> print(marks.keys())
dict_keys(['a', 'b', 'c', 'u'])
>>> print(marks.values())
dict_values([3, 4, 1, 9])
>>> print(marks.items())
dict_items([('a', 3), ('b', 4), ('c', 1), ('u', 9)])
>>> print("a" in marks.keys())
True
>>> print("a" not in marks.keys())
False
```

### Nesting
```
# dictionary in a list

cat1 = {"owner": "Andrew", "color": "black"}
cat2 = {"owner": "Kevin", "color": "white"}
cat3 = {"owner": "Will", "color": "grey"}
# option 1
cats = [cat1, cat2, cat3]
# option 2
cats = []
cats.append(cat1)
cats.append(cat2)
cats.append(cat3)
# list all
for cat in cats:
    print(cat)
    
# list in a dictionary
# --------------------
pizza = {
    "size": 12,
    "crust": "think",
    # best practice to include the comma at the last line
    "toppings": ["mushrooms", "cheese"],
}
print(len(pizza))
print(f"Size: {pizza['size']}, \nCrust: {pizza['crust']}")
print("Toppings:")
for topping in pizza["toppings"]:
    print(f"\t{topping}")

# output    
3
Size: 12,
Crust: think

Toppings:
        mushrooms
        cheese


# directory in directory

{
    'Abhishek': {'FG(CLIENT)': 11, 'FG(RL-LIQ)': 3}, 
    'Andrew': {'FG(CLIENT)': 26, 'FG(RL-LIQ)': 10}, 
    'Lisa': {'FG(CLIENT)': 10, 'FG(RL-LIQ)': 1}, 
    'Kyle': {'FG(RL-LIQ)': 2}
}

```





