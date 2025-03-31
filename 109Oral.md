1. Basic​: Explain the difference between mutable and immutable data types in Python. Provide examples of each type.

- The difference between `mutable` and `immutable objects` is that `mutable objects` can be modified in place after being created, while `immutable objects` cannot. A variable is ***reassigned*** to a newly created object when we attempt to modify an ***immutable object***.
- ***mutable objects*** include:
    - lists
    - dictionaries
    - sets
- ***immutable objects*** include:
    - strings
    - numbers (integers, floats, complex)
    - tuples
    - ranges
    - frozen sets

```python
# example using an immutable object
x = 'Hello'
print(id(x)) #outputs memory address
x += ' World'
print(id(x)) #outputs different memory address

# example using a mutable object
lst = [1, 2, 3]
print(id(lst)) #outputs memory address
lst.append(4)
print(id(lst)) #outputs the same memory address
```

2. Intermediate​: What does it mean that Python is "pass by object reference"? How does this differ from "pass by value" and "pass by reference"?

- ***Pass by value*** means functions are passed ***a copy*** of the original object and operations performed on that object within the function have no affect on the original object outside the function.
- ***Pass by reference*** means function are passed a reference to an argument's assigned value and when operations are performed on that referenced object, the original object referenced is changed in place.
- Python combines behaviours of both! ***Pass by reference object*** always passes *references to objects* and never copies or the objects themselves.
    - The distinction between ***mutable*** and ***immutable objects*** dictates whether the original object is modified through the reference or not. Because we cannot mutate an ***immutable object***, the variable used to reference the original object is reassigned to a new object.
```python
# example of 'Pass by value':
name = 'Jim'

def change_name(var):
    var = 'Bob'

change_name(name)
print(name) # 'Jim'

# example of 'Pass by reference`:
my_lst = [1, 2, 3]

def add_to_list(lst):
    lst.append(4)

add_to_list(my_lst)
print(my_lst) # [1, 2, 3, 4]

#example of pass by object reference
def reassign_lst(lst):
    lst = [4, 5, 6]

def modify_lst(lst):
    lst.append(4)

my_list = [1, 2, 3]

reassign_lst(my_list)
print(my_list) # [1, 2, 3] as the parameter is initialized locally and does not affect the global var.

modify_lst(my_list)
print(my_list) # [1, 2, 3, 4] as the original list was modified.
```

3. ​Basic​: Describe how variable scope works in Python. What's the difference between local and global variables, and how can you modify a global variable from within a function?

The ***scope*** of an identifier determines where it can be used.
- ***global variables*** are initialized outside of functions and can be used anywhere, including inside of functions.
- ***local variables*** are initialized inside of functions and can only be used within the function (functional scope) or nested functions because outside operations have no access to data within functions.
- a global variable can be modified within a function using the `global` keyword followed by the variable before modifying or reassigning the variable within the function.

4. Intermediate: What will this code output and why?

```python
a = [1, 2, 3]
b = a
a.append(4)
print(b)
```
- code explained:
    - `a` is assigned to a list value containing integer elements.
    - `b` is assigned to the same value referenced by `a`.
    - `a.append(4)` adds the integer `4` to the end of the list value referenced by `a`.
    - `print(b)` outputs `[1, 2, 3, 4]`.
        - because lists are ***mutable objects*** they can be ***modified in place***.
        - `b` references the same list value as `a` so the outputs is the modified list.

5. ​Advanced​: Explain Python's truthiness concept. What values are considered "falsy" in Python? How does this relate to boolean operations?
- The following values are evaluated as  ***falsy***:
    - `None` (None type)
    - `False` (boolean)
    - `0`, `0.0`, `0j` (0 numbers including ***integers, floats and complex numbers***)
    - `""` (empty strings)
    - `[]` (empty lists)
    - `{}` (empty dictionaries)
    - `()` (empty tuples)
    - `range(0)` (empty ranges)
    - `frozenset()` (empty frozensets)
- Everything else is considered ***truthy***.
- ***Truthiness*** refers to the nature of values (how values are evaluated) while the boolean values `True` and `False` refer to boolean values.
---------------------------------------------------------------------------------------------------------------
1.  ​Basic​: What is the primary distinction between `==` and `is` operators in Python? Provide an example where they give different results.

- The ***equality operator*** `==` checks if two operands have ***equal values*** while the ***identity operator*** `is` checks if two operands both reference the same object in memory.
```python
list1 = [1, 2, 3]
list2 = [1, 2, 3]
print(list1 == list2) # True
print(list1 is list2) # False
```

2.  ​Intermediate​: Given the following code:
```python
def change_value(lst):
    lst[0] = 'changed'
    lst = ['completely', 'new', 'list']

my_list = ['original', 'values']
change_value(my_list)
print(my_list)
```
What will be printed and why?

- `my_list is assigned to a list value containing string elements.
- `change_value` is invoked with the variable `(my_list)` passed into it.
    - function analysis:
        - `lst[0] = 'changed'` performs ***element reassignment***, reassigning the first element in `lst` to `changed`.
        - the parameter `lst` reassigned to a new list object containing string elements.
    - Because lists are ***mutable objects*** when the function is invoked, the list value referenced by the argument is ***modified in place***.
    - The ***parameter reassignment*** within the local namespace only affects the local variable and not the global variable.
- `print(my_list)` outputs `['changed', 'values']`.


3.  ​Basic​: Explain how string formatting works in Python. Compare and contrast at least two different methods for formatting strings.

- We use string formatting to interpolate variables into strings.
    - One way to do this is to use the `.format()` method.
        - We use curly braces as placeholders for arguments passed into the method.
```python
name = 'Joe'
print('Welcome to the club, {}!'.format(name))
```
    - Another way to do this is by using a ***formatted string literal (f-strings)*** to perform in-line string interpolation, merging variables and/or expressions into strings.
```python
name = 'Bill'
print(f'Welcome to the club, {name}!')
```
- The advantage that f-strings have over using the format method include better readability and quicker execution.
    - f-string allow for ***directly embedding expressions*** while the `.format()` method does not.

4.  ​Intermediate​: What's the difference between a tuple and a list in Python? Besides syntax, when would you use one over the other?

- Both lists and tuples are ***heterogenous, ordered sequences*** that use ***index syntax*** to access or modify member elements.
- Lists are ***mutable*** while tuples are ***immutable***.
    1. We use ***tuples*** when:
        - we want an ***immutable sequence*** (shows other developers that data should not change.)
        - you need to use ***dictionary keys*** (lists cannot use dictionary keys.)
        - performance is critical (tuples are slightly faster than lists).
    2. We use ***lists*** when:
        - the sequence needs to be modified after creation.
            - append, extend or remove elements.
            - we need to sort or reverse the order of elements.


5.  ​Advanced​: Consider this code:
```python
def outer_function():
    value = 10

    def inner_function():
        nonlocal value
        value = 20

    inner_function()
    return value

print(outer_function())
```
What will this code output? Explain how the nonlocal keyword works and how it differs from global.

- `print(outer_function())` outputs `20`.
    - Variables that are assigned within functions are ***local*** unless explicitly marked as otherwise.
    - Because the definition of the nested function `inner_function` uses the `nonlocal` keyword, instead of initializing another local variable named `value`, Python ***reassigns*** the variable to a new integer value `20`.
        - The `nonlocal` keyword tells Python the variable being reassigned is initialized in an ***external scope***, outside the nested function but not in the global scope.
    - `inner_function` is invoked in the ***local scope*** of `outer_function` executing the code within the function definition.
    - `outer_function` then returns the value referenced by `value`.
----------------------------------------------------------------------------------------------------------
1.  ​Basic​: Explain what Python's list comprehension syntax is and how it differs from using a regular for loop with append. Provide a simple example of each approach.

- Both a regular `for` loop and ***list comprehension*** can be used for the same result.
    - ***list comprehension*** is a more concise way to iterate through sequences and filter elements using a condional to create a new list object with specified member elements.
        - `[expression for item in iterable if condition]` is list comprehension syntax.
    - Using a regular ***for loop*** we first must initialize a variable to an empty object. Then a `for` statment initializing the first element of a sequence uses a conditional within the statement block to filter member elements and then perform an operation based on the condition.
    - The advantage of a regular `for` loop is that multiple conditionals are easier to read and can lead to messy code using ***list comprehension***.
```python
my_list = [1, 2, 3, 4, 5, 6]

even_list = [num for num in my_list if num % 2 == 0]
print(even_list) # [2, 4, 6]

odd_list = []
for num in my_list:
    if num % 2 != 0:
        odd_list.append(num)

print(odd_list) # [1, 3, 5]
```

2.  ​Intermediate​: Consider this code:
```python
def double_numbers(numbers):
    for i in range(len(numbers)):
        numbers[i] *= 2
    return numbers

original = [1, 2, 3, 4]
result = double_numbers(original)
print(original)
print(result)
```
What will be printed? What does this demonstrate about working with list objects in Python?

- function analysis:
    1. `double_numbers` uses 1 parameter, `(numbers)`.
    2. Using a ***for loop***, `i` is initialized to the first element of a ***range sequence*** containing a sequence of numbers from 0 to the total length of the sequence referenced by `numbers` (exclusive).
        3. For every iteration of the ***for loop***, ***augmented element reassignment*** is used to reassign the element contained in each index to the element multiplied by 2.
    4. The function returns the sequence referenced by the parameter `numbers`.
- `original` is assigned to a list value containing integers.
- `double_numbers` is invoked and passed in the argument `(original)`.
    - The return value is assigned to the variable `result`.
- `print(original)` outputs `[2, 4, 6, 8]`
- `print(result)` outputs `[2, 4, 6, 8]`
- underlying concepts
    - mutable objects
        - because lists are mutable, they are ***modified in place*** and modifications do not result in new objects.
        - `original` and `result` both point to the same object in memory.
    - element reassignment
        - using ***index syntax*** and ***augmented reassignment*** to modify the list object

3.  ​Basic​: What are Python's built-in functions? Identify at least three common built-in functions and explain what they do.

- Built-in functions are pre-defined functions that work on various data types and are available without requiring an import statement.
- 3 common built-in functions are:
    1. `print()` - displays output to the console.
        - take multiple arguments seperated by a `,` and displays them with a space between them.
        - can specify a ***seperator character*** and ***end characters***.
        - `print('Hello', 'World', sep='-', end='!') #Hello-World!
    2. `len()` - returns the length (number of items) in an object (lists, tuples, dictionaries).
        ```python
        my_list = [1, 2, 3, 4]
        print(len(my_list)) #4
        print(len('Python')) #6
        ```
    3. `type()` - returns the type of an object.
        - useful for debugging and understanding what type of data the code is working with.
        ```python
        my_dict = {'a': 1, 'b': 2}
        my_string = 'Hello world!'
        my_tuple = (1, 2, 3)
        print(type(my_dict)) #<class 'dict'>
        print(type(my_string)) #<class 'str'>
        print(type(my_tuple)) #<class 'tuple'>
        ```

4.  ​Intermediate​: How do default parameter values work in Python functions? What potential issue might arise when using mutable objects as default parameters?

- Default parameters are used in case an argument is not passed into the function when invoked.
- Using mutable objects as default parameters can lead to unexpected behaviour because default parameters are modified in place and shared (maintaining modifications) throughout function calls.
```python
def mut_display(lst = []):
    lst.append(1)
    print('List inside function:', lst)
    return lst

call1 = mut_display()
call2 = mut_display()
call3 = mut_display()

print('Final Lists:')
print('list one:', call1)
print('list two:', call2)
print('list three:', call3)
```


5.  ​Advanced​: Consider the following code:
```python
words = ['hello', 'world', 'goodbye', 'planet']
result = sorted(words, key=len, reverse=True)
print(result)
```
What will this code output and why? Explain how the key parameter works in Python's sorting functions.

- `print(result)` outputs `['goodbye', 'planet', 'hello', 'world']`
- The specified parameters in `sorted()` include:
    1. `key=len` telling the function to sort the member elements based on their length.
        - by default in ***ascending order***.
    2. `reverse=True` tells the function to sort the member elements in reverse order.
        - by ***descending order***.
