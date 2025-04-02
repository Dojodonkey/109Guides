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
--------------------------------------------------------------------------------------------------------------

2.  ​Intermediate​: Consider this code:
```python
   def remove_vowels(string):
       result = ""
       for char in string:
           if char.lower() not in "aeiou":
               result += char
       return result

   print(remove_vowels("LaunchSchool"))
```
What will this code output? Explain the string processing technique being used here and its efficiency.

- `print(remove_vowels("LaunchSchool"))` outputs `LnchSchl`.
    - the function `remove_vowels` is defined with one parameter `(string)`.
    - the variable `result` is assigned to an empty string.
    - `for char in string:` iterates of a sequence initializing the first character of that sequence to `char`.
        - a conditional expression checks if the copy of `char` lowercased, returned by the `.lower()` method, is a member of `"aeiou"`, a string containing vowels, using the logical operator `not` and the membership operator `in`.
            - if the conditional expression evaluates as ***truthy***, `result` is reassigned using ***augmented reassignment***, adding `char` to the string referenced by `result` and creating a new string object.
    - the function returns the string value referenced by `result`.
- Underlying concepts:
    - strings are immutable objects.
    - augmented reassignement
    - checking for membership

3.  ​Advanced​: Examine this code:
```python
   my_dict = {'a': 1, 'b': 2}

   def modify_dict(d):
       d['c'] = 3
       d = {'new': 'dict'}
       return d

   result = modify_dict(my_dict)
   print(my_dict)
   print(result)
```
What will be printed? Explain how dictionary mutability and reassignment work in function parameters, and how this relates to pass-by-object-reference in Python.

- `my_dict` is assigned to a dictionary object.
- function analysis:
    1. `modify_dict` uses 1 parameter `(d)`.
    2. `d['c'] = 3` adds a key value pair to the dictionary object referenced by `d`.
    3. `d = {'new': 'dict'}` the parameter `d` is reassigned to a new dictionary object.
    4.  the function returns the value referenced by `d`.
- `result = modify_dict(my_dict)`
    - `modify_dict` is invoked and passed the value referenced by `my_dict` as an argument.
    - when a function is called with an argument, the parameter inside the function definition becomes a reference to the same object that the argument is referencing.
    - the return value of the function is assigned to `result`.
- output:
    - `print(my_dict)` outputs `{'a': 1, 'b': 2, 'c': 3}
        - because dictionaries are ***mutable objects*** they are be modified in place.
    - `print(result)` outputs `{'new': 'dict'}
        - because `my_list` is reassigned in the local namespace and the the dictionary object it references is returned by the function.
        - This reassignment is local to the function and doesn't affect what my_dict references in the global scope.

-------------------------------------------------------------------------------------------------------------------------------
2.  ​Intermediate​: Consider this code that uses a nested data structure:
    ```python
   contacts = {
       'John': {'phone': '555-1234', 'email': 'john@example.com'},
       'Mary': {'phone': '555-6789', 'email': 'mary@example.com'}
   }

   def update_contact(contacts_dict, name, field, value):
       pass # write function definition here

   update_contact(contacts, 'John', 'email', 'john.doe@example.com')
   print(contacts['John']['email'])  # Should print: john.doe@example.com
   ```
Implement the update_contact function. Explain how nested dictionaries work in Python and how you would access and modify nested data.

- To update dictionary we use ***key access syntax***.
    - solution:
        `contacts_dict[name][field] = value`
    - the first key `name` accesses the nested dictionary.
    - the second key `field` access the specific key within the nested dictionary.
- because dictionaries are ***mutable objects*** when they are referenced by the argument passed into the function and when used in operations within the function, ***they are modified in place*** affecting the original object created in the global scope.


3.  ​Advanced​: Describe Python's approach to error handling. Explain the try/except pattern, when you would use it, and provide an example of catching specific exceptions versus catching all exceptions. What are potential drawbacks of using a broad exception handler?

- To gracefully handle exceptions, we can use a `try/except` block.
    1.  ​Try block​: Contains code that might raise an exception
    2.  ​Except block​: Catches and handles specific exceptions
    3.  ​Else block​ (optional): Executes if no exceptions occur in the try block
    4.  ​Finally block​ (optional): Always executes, regardless of exceptions.
```python
try:
    num = int('abc')
except ValueError:
    print('Invalid argument')

try:
    num = int('abc')
except Exception:
    print('Something went wrong')
```
- The disadvantage of catching all exceptions:
    1. treats different types of exceptions the same making it difficult to diagnose issues when they occur.
    2. hides bugs that should be fixed rather than handled.
--------------------------------------------------------------------------------------

```python
def process_string(input_string):
    result = ""
    for i in range(len(input_string)):
        if i % 2 == 0:
            result += input_string[i].upper()
        else:
            result += input_string[i].lower()
    return result

sample = "Python Programming"
output = process_string(sample)
print(output)
```
Explain the code, what it outputs and how immutable objects are reassigned

- `sample` is assigned to a string value.
- `process_string` is invoked and passed in the object referenced by `sample`.
    - The return value of the function is assigned to the variable `output`.
- function analysis:
    1. the local variable `result` is assigned to an empty string.
    2. a for loop initializes `i` to the first element of a ***range sequence*** and iterates through that ***range sequence***, beginning at 0 and stopping at the return value of the built-in function `len()` passed in the object referenced by `input_string`.
        3. a conditional `if/else` block is executed at each iteration.
            - `if i % 2 == 0:` checks if `i` is an ***even number*** and if the arithmetic expression evaluates to a ***truthy value***,
            `result` is ***reassigned*** using ***augmented reassignment*** to a new object, ***concatenating*** `result` with the character at the index of `input_string` converted to upper case using the string method `.upper()`.
            - the code within the `else` block is executed if the ***expression*** in the `if` statement is ***falsy***.
                - `result` is reassigned to a new string object ***concatenating*** `result` with the character at the index of `input_string` converted to lower case using the string method `.lower()`.
    4. the function returns `result`.
- output:
    - `print(output)` outputs `PyThOn pRoGrAmMiNg`
        - All the characters at even indexes are upper case and odd are lower case.
-Underlying concepts:
    - string concatenation
    - immutable objects are reassigned because they cannot be modified in place.

3.  ​Advanced​: Examine this code involving default arguments:
```python
def add_item(item, shopping_list=[]):
    shopping_list.append(item)
    return shopping_list

list1 = add_item('apple')
list2 = add_item('banana')
print(list1)
print(list2)
```
What will be printed and why? Explain the concept of mutable default arguments in Python functions and how you would rewrite this function to avoid potential issues.

- function analysis:
    1. `add_item` accepts 2 parameters, `item` and a default parameter `shopping_list = []`.
    2. `item` is appended to `shopping_list` using the list method .append().
    3. the function returns the value referenced by `shopping_list`.
- `add_item` is invoked and passed in the object referenced by the string value `apple`.
    - the return value is assigned to the variable `list1`.
- `add_item` is again invoked and passed in the object referenced by the string value `banana`.
    = the return value is assigned to the variable `list2`.
- output:
    - `print(list1)` and `print(list2)` both output the same list object, `['apple', 'banana']`.
        - because the value referenced by the default parameter is ***mutable*** it is modified in place and shared across multiple invocations.
        - this is important to be aware of as this concept can lead to unexpected behaviour when using mutable objects as default parameters.
        - a rewritten function to avoid such potential issues:
        ```python
        def add_item(item, shopping_list = None):
            if shopping_list = None:
                shopping_list = []
            shopping_list.append(item)
            return shopping_list
        ```
--------------------------------------------------------------------------------------------------------------------------------
'''
Identify all the identifiers in the following code snippet. For each identifier, categorize it as a function name, variable name, parameter name, or built-in function/method. Also note which ones follow Python naming conventions and which ones don't.
'''
```python
def calculate_total(items_list, tax_rate):
    SubTotal = 0 # sub_total

    for item in items_list:
        SubTotal += item

    def apply_discount(amount, discount_percent):
        return amount * (1 - discount_percent/100)

    total = SubTotal * (1 + tax_rate)
    if total > 100:
        total = apply_discount(total, 10)

    print(f"Your total is: ${total:.2f}")
    return round(total, 2)

my_items = [24.99, 19.95, 5.49]
SALES_TAX = 0.08
final_amount = calculate_total(my_items, SALES_TAX)
```








----------------------------------------------------------------------------------------------------------------------------------
Advanced​: Analyze the following code that combines variables, mutability, functions, and loops:
```python
def process_data(data, multiplier=2):
    result = []
    for item in data:
        if isinstance(item, list):
            new_item = process_data(item, multiplier + 1)
            result.append(new_item)
        else:
            result.append(item * multiplier)
    return result

numbers = [1, 2, [3, 4], 5]
output = process_data(numbers)
print(numbers)
print(output)
```
What will this code output? Explain your answer by discussing:

- function analysis:
    1. `process_data` accepts 2 parameters, `data` and a default parameter `multiplier = 2`.
    2. `result` is assigned to an empty list.
    3. a for loop is used to iterate through the sequence referenced by `data` initializing and assigning `item` to the first element of the sequence.
        4. an `if/else` block within the for loop is a conditional that uses the built-in function `isinstance()` function to check if `item` is a list object.
            - if the expression in the list statement returns `True` the code within the `if` block will execute.
                1. `process_data` is invoked and passed in the object referenced by `item` and `multiplier + 1'.
                    - The return value of the function is assigned to `new_item`.
                2. `result.append(new_item)` appends `new_item` to the list object referenced by `result`.
            - if the expression in the if statment returns `False` the code within the `else` block is executed:
                1. `item * multiplier` is appended to the list object referenced by `result`.
    4. The function returns the value referenced by `result`.
- `numbers` is assigned to a list object containing integers and a nested list.
- `process_data` is invoked and passed in the object referenced by `numbers`.
- output:
    - `print(numbers)` outputs `[1, 2, [3, 4], 5]`
    - `print(output)` outputs `[2, 4, [9, 12], 10]`

1.  How the recursive function works with nested data structures
    - This is not included in the PY109 study guide but i believe the recursive function call within the function definition loops through the nested list and modifies the elements by executing the code within the else block (the default parameter now assigned to a new integer value (3)).
2.  How the default parameter is used across recursive calls
    - because the default parameter is a mutable object, when it is reassigned to a new integer object, upon the next recursive function invocation, the modification is forgotten and not shared.
3.  The role of mutability in this example
    - becaue `result` is assigned to a list, a mutable object, it is modified in place with each iteration of the for loop.
4.  How Python handles the multiplication operation differently for numbers vs lists
    - because integer values are immutable, when we attempt to modify them using arithmetic, a new object is created.
    - but if we attempt to concatenate lists using, *, a new list object is also created.
