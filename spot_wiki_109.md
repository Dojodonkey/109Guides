# SPOT Wiki 109
## Type coercions: explicit (e.g., using int(), str() and implicit)
1.  Which variable is coerced? Is it implicit or explicit coercion?
```python
x = 3.5
y = 5
z = x + y
```
- When we use arithmetic operators and one or both operands are *floats*, the result will be ***implicitly*** converted to a float.
    - `x` is assigned to a *float* value.
    - `y` is assigned to an *integer* value.
    - `z` is assigned to the returned value of the arithmetic expression `x + y`.
        -  The **value** of `y` is *implicitly coerced* to a ***float*** making the returned value of the expression, a ***float***.
        - `z` is assigned to the *float value*, `8.5`

2. What coercion is happening here? Is it implicit or explicit?
```python
a = 1
b = 2
print(a + b)
```
- Both `a` and `b` are assigned to `integer` values.
- `print(a + b)`
    - `print` *internally* performs ***implicit coercion*** to on the return value of `a + b` to a ***string***.
        - First the expression is evaluated, then it is ***internally implicitly coerced***
        - Note: This is not considered ***coercion***.
        ```python
        a = 1
        b = 2
        print(a + b) # 3
        print(type(a)) # <class: int>
        ```

3. What coercion is happening here? Is it implicit or explicit?
```python
month = "December"
day = int(input("What day is it? "))
print(f"Today is the {day} of {month}")
```
- `day = int(input("What day is it? "))`
    - The return value of `input("What day is it? ")` by default is a ***string***.
    - By using the built-in function `int()` that string value is ***explicitly coerced*** to an ***integer***.
- `print(f"Today is the {day} of {month}")` uses a ***formatted string literal*** to perform ***in-line string interpolation***.
    - `print()` *internally* ***implicitly coerces*** the argument passed to it into a ***string***.
        - This is technically ***not*** considered coercion.

## Numbers, including handling exceptions (ValueError, ZeroDivisionError)

### Basic questions:

- Are integers and floats mutable or immutable?
    - ***(Immutable)***
    - ***Immutable*** objects ***cannot*** be ***modified*** after being created.
        - Meaning, if modified, a ***new object*** will be created.
    - ***Mutable*** objects on the other hand can be ***modified directly in place***.
- Are integers and floats primitive or non-primitive?
    - ***(Primitive)***
    - ***Primitive data types*** are basic data types, storing simple values.
    - ***Non-Primitive data types*** are complex data types, that can store ***multiple*** objects.
- Are integers and floats literals?
    - ***Yes!***
- What is a literal?
    - Any syntatic notation that lets you directly represent an object in source code.

1. What does this return and why? What concept does this cover?
```python
def convert_to_int(string):
    try:
        converted_integer = int(string)
        return converted_integer
    except ValueError:
        return "That string cannot be converted to an integer"

print(convert_to_int("hello"))
print(convert_to_int("5"))
```
- Function Analysis:
    - `convert_to_int` takes one parameter, `string`.
    - Within the `try:` block:
        1. the string value assigned to the parameter `string` is explicitly converted to an ***integer*** and assigned to the variable `converted_integer`.
        2. the function returns `converted_integer`.
    - `exept ValueError:` is used in case a ValueError is thrown and if so returns `That string cannot be converted to an integer`.
    - We use a ***try except*** block to gracefully handle exceptions that might be raised during exection.
        - `ValueError` is raised when a data type is correct, but the value is not appropriate for the operation.
- Output:
    - `print(convert_to_int("hello"))` outputs `That string cannot be converted to an integer` because the string value contains ***alphabetic*** characters and therefore cannot be ***explicitly coerced*** to an integer.
    - `print(convert_to_int("5"))` outputs `5`.
- Underlying Concepts:
    - explicit coercion
    - error handling

2: What does this return and why? What concept does this cover?
```python
def division(number1, number2):
    numerator = number1
    denominator = number2

    try:
        result = numerator / denominator
        return result
    except ZeroDivisionError:
        return "The denominator cannot be zero"

print(division(5, 0))
```
- Function Analysis:
    - `division` takes two parameters `(number1, number2)`
    1. The local variable `numerator` is assigned to the value referenced by `number1`.
    2. The local variable `denominator` is assigned to the value reference by `number2`.
    3. Within a `try` block, the local variable `result` is assigned to the value returned by the expression `(numerator / denominator)` and returns `result`
    4. `except ZeroDivisionError:` is executed if a ZeroDivisionError is thrown and returns `The denominator cannot be zero`
- Output:
    - `print(division(5, 0))` outputs `The denominator cannot be zero` because `0` is assigned to `denominator` and in the `try` block, the expression assigned to `result` (`5 / 0`) raises `ZeroDivisionError` causing the `except` block to execute.
        - `ZeroDivisionError` is raised when an expression attempts to divide by `0`.
    - The `try/except` block is a way to gracefully handle errors allowing the program to continue executing instead of crashing.

- Underlying Concepts:
    - ZeroDivisionError
    - Error handling

3: What does this print and why, what concept does this demonstrate?
```python
def addition(number1, number2):
    number1 += number2

x = 1
y = 2

addition(x, y)
print(f"x is {x}, y is {y}")
```
- Function Analysis:
    - `addition` takes two parameters, `(number1, number2)`.
    - `number1` is reassigned ***locally*** using ***augmented reassignment***, adding the value assigned to `number2` to the value assigned `number1` and assigning the result of that expression to the ***local*** variable `number1`.
- Global Scope:
    - `x` is assigned to the integer value `1`.
    - `y` is assigned to the integer value `2`.
    - `addition(x, y)` is invoked.
- Output:
    - `print(f"x is {x}, y is {y}")` outputs `x is 1, y is 2`
        - because integer values are immutable, when the value of `number1` is modified a new object is created within the function but does not affect the integer value referenced in the global scope.
         ```python
         def addition(number1, number2):
            number1 += number2
            return number1

            x = 1
            y = 2

            print(addition(x, y)) # 3
            print(f"x is {x}, y is {y}")
            # x is 1, y is 2
- Underlying concepts:
    - pass by object reference
    - mutable versus immutable objects

4. What does this print and why? What concept does this cover? How would you refactor this to remove the space?
```python
print(2 + 3 * 4, 4 * (3 + 2))
```
- The output is `14 20`
    - Arithmetic operator precedence follows the PEMDAS acronym.
        - parentheses, exponents, multiplication/division (from left to right), addition/subtraction(from left to right).
        - We should never assume other people looking at our code are familiar with precedence and therefore the use of parentheses is ecouraged to clarify code.
    - Two seperate objects are output because `print()` and output multiple objects, seperated by a `,`.
- Underlying Concept
    - operator precedence
    - print()

5. What can be used in place of commas to make this more readable?
```python
print(123112940)
```
- We can use underscores ( _ ) to make large numbers more readable.
    ```python
    number = 11223344556
    print(f'{number:_}') # 11_223_344_556
    ```
    - use a ***format specifier*** in an f-string to auto format large numbers (thousand seperator).

## Strings

### Basic questions:

- Are strings mutable or immutable?
    - ***(Immutable)***
- When immutable objects are modified a new object is created.
Are strings primitive or non-primitive?
    - ***(Primitive)***
- Are strings literals?
    - ***(Literals)***
    - a literal is any ***syntactic notation*** that directly represents an object in source code.
    - String literals can be written in multiple ways:
    ```python
    'single quotes'
    "double quotes"
    '''triple quotes for multi-line strings'''
    """triple quotes for multi-line strings"""
    ```
- What is a text sequence?
    - a string of ***characters***.
- What kind of characters are used in a string?
    - ***Unicode characters***
- Are text sequences the same as ordinary sequences?
    - No, text sequences contain only ***Unicode characters***.
    - Ordinary sequences refer to any other sequence types that store multiple elements (lists, tuples, ranges)

1. What is the output of this code, and why? What is the concept covered here?
```python
str1 = "Hello, world!"
sub1 = str1[8:12]
print(sub1)
sub2 = str1[::-1]
print(sub2)
sub3 = str1[::2]
print(sub3)
```
- `str1` is assigned a string value.
- `sub1` is assigned to a segment of the string `str1` references using ***slicing notation***, `str1[8:12]` accesses the string value referenced by `str1` beginning at the 8th index and stopping at the 12th index (exclusive).
- `print(sub1)` outputs `orld`.

- `sub2` is assigned an accessed segment of the string value referenced by `str1` using slicing notation, `str[::-1]` by default, as no start and stop arguments are specified, beginning at the first index and stopping at the index equal to the length of the string. A ***negative step argument*** is specified, iterating through the string sequence, beginning at the highest index and moving to the lowest, every step-th count (in this case -1, returning the string sequence in reverse).
- `print(sub2)` outputs `!dlrow ,olleH`.

- `sub3` is assigned to a string segment referenced by `str1` using slicing notation. `str1[::2]` by default as no start and stop arguments are specified, begins at the lowest index at stops at the index equal to the length of the string referenced. But this time a positive ***step argument*** is specified (2). This means the string will slice at every 2nd member element beginning at the lowest index and moving towards the highest.
- `print(sub3)` outputs `Hlo ol!`

- Underlying concepts:
    - slicing strings

- Revised Answer from LSbot:
    •   `​sub1 = str1[8:12]​`: Extracts characters from index 8 up to (not including) index 12.
    •   `​sub2 = str1[::-1]​`: Reverses the entire string using a step of -1.
    •   ​`sub3 = str1[::2]​`: Selects every other character from the string.
    - String slicing uses the syntax `[start:stop:step]` where:
    •   Omitted start defaults to 0
    •   Omitted stop defaults to string length
    •   Omitted step defaults to 1
   - This demonstrates ***string slicing***, a fundamental technique for extracting portions of strings without modifying the original (since strings are immutable).

2. What does this print and why? What concept is this?
```python
print("Hello\nWorld")
```
    - The code uses what is called an ***escape character***.
    - `\n` moves the console cursor to a ***new line***.
    - Other common ***escape characters*** include:
        - `\t` => tab
        - `\'` => quotation mark signified as part of the string
        - `\r` => carriage return

3.  What does this print and why? What concept is this?
```python
name = 'Alexander Graham Bell'
print(name[0])
```
    - `print(name[0])` will output `A`
    - The underlying concept is using ***indexing syntax*** to access a string.

## f-strings

### Basic Questions:
- What are f-strings?
    - F-strings are ***formatted string literals*** that perform in-line string interpolation that can merge variables or expression into a string.
    - Advantages:
        - better readability than previous formatting methods.
        - evaluate expressions at runtime.

1. What does this print and why, what is the concept?
    ```python
    name = 'Abraham Lincoln'
    print(f"{name} was a President of the US")
    ```
    - `print(f"{name} was a President of the US")` outputs `Abraham Lincoln was a President of the US`
        - This is an example of using ***formatted string literals (f-strings)*** to perform ***in-line string interpolation*** merging variables with strings.
        - To use create an f-string, we use the ***prefix f*** before the string.

** String Methods

*** Basic Questions:

- How do you identify a method versus a function?
    - ***Method Identification***
    •   A method is invoked with ​dot notation​: `object.method()`
    •   Methods are bound to specific objects and operate on that object
    •   Example: `"hello".upper()` - the upper() method is called on the string object
    - ***Function Identification***
        •   A function is called directly by its name: function(arguments)
        •   Functions operate on the arguments passed to them
        •   Example: `print("hello")` - the print() function takes the string as an argument
    - Key Differences
        •   Methods belong to objects and are accessed through the object
        •   Functions exist independently and aren't tied to a specific object
        •   Methods ***implicitly*** use the object they're called on as their first argument
        •   Functions receive all their arguments ***explicitly***

1. What does this print and why?
    ```python
    mashup = "thIs is How we type careLEssly"
    cleaned = mashup.capitalize()
    print(cleaned)
    ```
    - `mashup` is assigned to a string value.
    - `mashup.capitalize()` returns a copy of the string referenced by `mashup` with the first string character converted to uppercase and the rest to lower case and returns the string value to the variable `cleaned`.
    - `print(cleaned)` outputs `This is how we type carelessly`

2. What do these print and why?
    ```python
    stuff = 'tHIS iS bACKWARDS'
    str1 = stuff.swapcase()
    str2 = stuff.upper()
    str3 = stuff.lower()
    print(stuff)
    print(str1)
    print(str2)
    print(str3)
    ```
    - `stuff` is assigned a string value.
    - `stuff.swapcase()` returns a copy of the string referenced by `stuff` with all upper case string characters converted to lower case and vice versa and returned string value is assigned to the variable `str1`
    - `stuff.upper()` returns a copy of the string referenced by `stuff` with all string characters converted to upper case and is assigned to the variable `str2`.
    - `stuff.lower()` returns a copy of the string with all string characters converted to lower case and is assigned to the variable `str3`.

    - `print(stuff)` outputs `tHIS iS bACKWARDS`.
    - `print(str1)` outputs `This Is Backwards`.
    - `print(str2)` outputs `THIS IS BACKWARDS`.
    - `print(str3)` outputs `this is backwards`.

3. What do these print and why?
    ```python
    s1 = "Hello"
    print(s1.isalpha())
    s2 = "Hello World"
    print(s2.isalpha())
    s3 = "Hello!"
    print(s3.isalpha())
    s4 = "Hello123"
    print(s4.isalpha())
    s5 = ""
    print(s5.isalpha())
    s6 = "こんにちは"
    print(s6.isalpha())
    s7 = "HelloWorld"
    print(s7.isalpha())
    words = ["apple", "banana", "cherry"]
    print(all(word.isalpha() for word in words))
    ```
    ```python
    s1 = "Hello"
    print(s1.isalpha())
    ```
    - `s1` is assigned to a string value.
    - `print(s1.isalpha())` outputs `True`.
        - `.isalpha()` is a predicate method that returns a boolean value, `True` if all the string characters in the string object are ***alphabetic***.

    ```python
    s3 = "Hello!"
    print(s3.isalpha())
    ```
    - `s3` is assigned to a string value.
    - `print(s3.isalpha()) outputs `False`.
        - `.isalpha()` is a predicate method that returns the boolean value `True` is ***all*** string characters in the object are ***alphabetic***. In this case, `!` is a ***special character***.

    ```python
    s4 = "Hello123"
    print(s4.isalpha())
    ```
    - `s4` is assigned to a string value.
    - `print(s4.isalpha())` outputs `False`.
        - `.isalpha()` is a predicate method that returns the boolean value `True` is all characters in the string object are ***alphabetic***.
            - In this case, `123` are digit characters.

    ```python
    s5 = ""
    print(s5.isalpha())
    ```
    - `s5` is assigned to an ***empty stringd***.
    - `print(s5.isalpha())` outputs `False`.
        - `.isalpha()` is a predicate method that returns `True` if ***all*** string characters are ***alphabetical*** or the string ***does not contain atleast one character***.

    ```python
    s6 = "こんにちは"
    print(s6.isalpha())
    ```
    - `s6` is assigned to a string value.
    - `print(s6.isalpha()) outputs `True`.
        - `.isalpha()` is a predicate method that checks if all string characters in the string value are ***alphabetical***. In this case, Unicode includes all languages, Japanese included.

    ```python
    s7 = "HelloWorld"
    print(s7.isalpha())
    ```
    - `s7` is assigned to a string value.
    - `print(s7.isalpha()) outputs `True`.
        - `.isalpha()` is a predicate method that returns the boolean value, `True` if all characters in the string are ***alphabetic***.
        - if `s7` were assigned to the string value `Hello World` the output would be `False` as a space (" ") is ***not*** considered alphabetic.

    ```python
    words = ["apple", "banana", "cherry"]
    print(all(word.isalpha() for word in words))
    ```
    - `words` is assigned to a list value containing string elements.
    - `print(all(word.isalpha() for word in words))` outputs `True`.
        - using the function `all()` passed in a ***generator expression***,
        the code iterates through the list value of `words` using the predicate method `.isalpha` to check if each `word` in `words` contains ***only*** ***alphabetic*** characters and if the string elements are not empty strings.

4. What does this print and why?
    ```python
    string1 = "HelloWorld"
    string2 = "12345"
    string3 = "Hello World"

    result1 = string1.isalpha()
    result2 = string2.isalpha()
    result3 = string3.isalpha()

    print("Is '{}' alphabetic?".format(string1), result1)
    print("Is '{}' alphabetic?".format(string2), result2)
    print("Is '{}' alphabetic?".format(string3), result3)
    ```
    - The code first initializes multiple variables to multiple string values.
    - Then boolean values are assigned to initialized variables by calling `.isalpha()` on multiple string values.
        - `.isalpha()` returns the boolean value `True` if all characters in a string are ***alphabetical***.   False, if otherwise.
    - `print("Is '{}' alphabetic?".format(string1), result1)` outputs `Is 'HelloWorld' alphabetic? True` using the format method in a string passed into the `print()` call, it uses brackets {} as placeholders for values referenced by arguments ***passed into*** the `.format()` method.
        - a comma is then used to print another object `result1`, outputting the boolean value referenced (`True` because all characters in `HelloWorld` are alphabetical (with no spaces))
    - `print("Is '{}' alphabetic?".format(string2), result2)` outputs `Is '12345' alphabetic? False` the same way as the print call on the previous line but this time, the boolean value referenced by `result2` is `False` because `12345` are digits and not alphabetic characters.
    - `print("Is '{}' alphabetic?".format(string3), result3)` outputs `Is 'Hello World' alphabetic? False` also using the `.format()` method and the boolean value referenced by `result3` returns `False` because `Hello World` contains a space character (' ') which is not considered alphabetical.
    - Underlying Concepts:
        - String method `.isalpha()`
        - `.format()` method

    - Answer from LSbot:
    1.  Three string variables are initialized with different values:
        •   string1 contains only letters
        •   string2 contains only digits
        •   string3 contains letters and a space
    2.  The .isalpha() method is called on each string:
        •   For string1: Returns True because all characters are letters
        •   For string2: Returns False because digits are not alphabetic characters
        •   For string3: Returns False because spaces are not alphabetic characters
    3.  Each print statement uses:
        •   String formatting with the .format() method to insert the string value into the message
        •   A comma to add the boolean result as a separate argument to print
   - The code demonstrates two important string concepts:
        1.  The .isalpha() method, which tests if a string contains only alphabetic characters
        2.  The .format() method for string interpolation, which is an alternative to f-strings
   - For .isalpha() to return True, the string must contain at least one character AND all characters must be letters (no spaces, digits, or special characters).

5. What do these print and why?
    ```python
    s1 = "123abc"
    print(s1.isdigit())
    s2 = "123$%^"
    print(s2.isdigit())
    s3 = ""
    print(s3.isdigit())
    s4 = "12345"
    print(s4.isdigit())
    ```
- Multiple variables are assigned to multiple string values.
- The print call passed in the predicate string method `.isdigit()` calls on those string values and returns a boolean value based on whether or not all characters in the string are digits ('0'-'9'). `print()` outputs that returned boolean value.
- `s1` is assigned to the string value `123abc`.
-  `print(s1.isdigit())` outputs `False` because `abc` are not digits.
- `s2` is assigned to the string value `123$%^`.
- `print(s2.isdigit()) outputs `False` because special characters are not digits.
- `s3` is assigned an empty string.
- `print(s3.isdigit())` outputs `False` because the predicate method will return False if there is not atleast 1 char in the string.
- `s4` is assigned to `12345`.
- `print(s4.isdigit())` outputs `True` because all characters are digits.

6. What do these print and why?
    ```python
    print("Hello World".isalnum())
    print("Hello@World".isalnum())
    print("".isalnum())
    print("Hello123".isalnum())
    ```
    - `.isalnum()` is a ***predicate string method*** that returns a ***boolean value*** based on whether or not ***all characters*** in the string value being called is ***alphanumeric*** ('a'-'z', 'A'-'Z', '0'-'9').
    - `print("Hello World".isalnum())` outputs `False` because the string contains a space character `' '`.
    - `print("Hello@World".isalnum())` outputs `False` because `@` is a special character and not alphanumeric.
    - `print("".isalnum())` outputs `False` because `.isalnum()` returns `False` if the string does not contain atleast one character and not all characters are alphanumeric.
    - `print("Hello123".isalnum())` outputs `True` because all string characters are alphanumeric.

7. What do these print and why?
    ```python
    name = 'HELLO'

    if name.isupper():
        print("WORLD")
    else:
        print("world")
    ```
    - `name` is initialized to a string value with all string character uppercased, `HELLO`.
    - Using ***conditional logic*** an ***if else block*** checks if all cased characters are upper cased using the predicate string method `.isupper()` calling the string value referenced by `name`. If `True`, a ***truthy value*** the code within the ***if block*** will execute and output `WORLD`. If the method returns `False` a ***falsy value*** the ***else block*** will execute ouputting `world`.
    - The output of this code is `WORLD`.

8. What do these print and why?
    ```python
    def punctuation_type(str):
        if str == str.upper():
            print('This is all caps')
        elif str == str.lower():
            print('This is all lowercase')
        else:
            print('Neither')

    str1 = 'HELLO'
    str2 = 'yolo'
    str3 = 'My Name Is: '

    punctuation_type(str1)
    punctuation_type(str2)
    punctuation_type(str3)
    ```
    - Function Analysis:
        - `puctuation_type` takes one parameter `(str)`.
        - Using ***conditional logic*** in an ***if elif else block***:
            1. using an equality operator (==), if the value referenced by `str` is equal to the value of `str.upper()`, the expression in the `if` statement is ***truthy*** and the function will output `This is all caps`. If ***falsy*** the conditional block will continue to execute.
                - `.upper()` is a string method that returns a copy of the string called with all characters converted to uppercase.
            2. again using an equality operator (==), if the value referenced by `str` is equal to the value of `str.lower()`, the expression in the `elif` statement is ***truthy*** and the function will output `This is all lowercase`.
                - `.lower()` is a string method that returns a copy of the string called with all cased characters converted to lower case.
            3. If neither the `if` block or `elif` block are ***truthy***, the else block will run and output `Neither`.
    - Global Code:
        - three variables are assigned different string values.
            ```python
            str1 = 'HELLO'
            str2 = 'yolo'
            str3 = 'My Name Is: '
            ```
    - Output:
        ```python
        punctuation_type(str1)
        punctuation_type(str2)
        punctuation_type(str3)
        ```
        - The code outputs:
        ```
        This is all caps
        This is all lowercase
        Neither
        ```

9. What do these print and why?
    ```python
    str1 = "    "
    str2 = "  Hello   "
    str3 = "Hello World"

    print(str1.isspace())
    print(str2.isspace())
    print(str3.isspace())

    sentence = "Hello     World!   How are you?   "
    word_count = sum(1 for word in sentence.split() if not word.isspace()) #conditional is redundant
    print("Number of words in the sentence:", word_count)
    ```
    ```python
    print(str1.isspace())
    print(str2.isspace())
    print(str3.isspace())
    ```
    -outputs the ***boolean value*** returned by the ***predicate string method*** calling the variable referencing a string object.
    - `.isspace()` returns a ***boolean value*** based on whether the string contains only ***white space characters***, `True` if so and `False` if not.

    ```python
    sentence = "Hello     World!   How are you?   "
    word_count = sum(1 for word in sentence.split() if not word.isspace())
    print("Number of words in the sentence:", word_count)
    ```
    - `sentence` is initialized to a string value.
    - `sum(1 for word in sentence.split() if not word.isspace())`
        - the function `sum()` takes a ***generator expression*** passed into it that iterates through a list created by the string method `sentence.split()` using only words that are not whitespace characters
        (`if not word.isspace()` This ***conditional*** is ***redundant*** as the `.split()` method treats consecutive white space characters as ***delimiters***) and returns the integer `1` for every `word` that is ***truthy***.
        The total of all ***truthy*** words (the return value of `sum()`) is assigned to the variable `word_count`.
    - Output:
        ```
        True
        False
        False
        Number of words in the sentence: 5
        ```

10. What do these print and why?
    ```python
    s = "   Hello, World!   "
    print(s.strip())
    print(s.strip(" !"))
    ```
    - Output:
        ```python
        Hello, World!
        Hello, World
        ```
        - This is because the `.strip()` method by default returns a copy of the string value referenced in the call with ***all leading and trailing whitespace characters removed***. If characters are specified ('char') the method will remove leading and trailing characters until a character is encountered that is not specified in the argument.

11. What do these print and why?
    ```python
    s = "www.example.com"
    print(s.lstrip('wcmo.'))
    ```
    - Output: `example.com`
        - `.lstrip()` is passed in characters as an argument specifying which ***leading characters*** (from left to right) should be stripped. Those characters specified will be stripped until a character is encountered that is not in the argument.

12. What do these print and why?
    ```python
    s = 'impatient'
    print(s.rstrip('tp'))
    print(s.rstrip('p'))
    ```
    - Output :
    ```
    impatien
    impatient
    ```

    - `.rstrip()` is passed in specified characters that should be stripped from the ***trailing*** side (from right to left). These characters are removed until a character is encountered that is not specified in the argument and ***returns a copy of original string with modifications***.

13. What do these print and why?
    ```python
    s = "Hello, World!"
    print(s.replace("Hello", "Hi"))
    print(s.replace("o", "0"))
    ```
- Output:
    ```python
    Hi, World!
    Hell0, W0rld!
    ```
- `.replace()` returns a ***copy*** of the string, by default replacing ***all occurences*** of the first substring specified, with the second string specified.

14. What do these print and why?
    ```python
    sentence = "This is a sample sentence."
    words = sentence.split()
    print(words)

    csv_data = "John,Doe,30,New York"
    fields = csv_data.split(",")
    print(fields)

    sentence = "This is a sample sentence."
    words = sentence.split(maxsplit=2)
    print(words)
    ```
    ```python
    sentence = "This is a sample sentence."
    words = sentence.split()
    print(words)
    ```
    - Output:
        `['This', 'is', 'a', 'sample', 'sentence.']`
        - `.split()` returns a list value of substrings. By default the method uses whitespace characters as ***delimiters*** (delimiters not included in the list value).

    ```python
    csv_data = "John,Doe,30,New York"
    fields = csv_data.split(",")
    print(fields)
    ```
    - Output:
        `['John', 'Doe', '30', 'New York']`
        - `.split(",")` returns a list of substrings using ',' as a delimiter and splitting the substring at every occurence. (The delimiter is not included in the list)

    ```python
    sentence = "This is a sample sentence."
    words = sentence.split(maxsplit=2)
    print(words)
    ```
    - Output:
        `['This', 'is', 'a sample sentence']
        - `maxsplit=2` is specified in the argument passed into `.split()` which splits the substrings at the first two occurrences of whitespace only and returns a list value of substrings.
            - meaning the total number of elements in the list is maxsplit + 1.

15. What does this print and why?
    ```python
    str1 = "hello world"
    str2 = str1.capitalize()
    print("Original string:", str1)
    print("Capitalized string:", str2)
    ```
    - Output:
    ```python
    hello world
    Hello world
    ```
    - `.capitalize()` returns a copy of the string value assigned to `str1` with the ***first character converted to upper case and all other cased character lower case*** and assigns it to `str2`.
    - Because string values are ***immutable***, when we modify a string, a new string object is created.

## Boolean vs. truthiness

### Basic Question:

- In Python, what values are considered Falsy and what are considered Truthy?
    - Falsy values include:
        - None
        - False
        - 0 (integer)
        - 0j (complex number)
        - 0.0 (float)
        - '' (empty string)
        - {} (empty dictionary)
        - () (empty tuple)
        - frozenset()
        - range(0)
    - All other values are considered truthy.
        - Truthy values allow us to use conditions or logical expressions.
    - Use the words, ***truthy*** or ***falsy*** when discussion ***expressions***.
    - Do not us ***True*** or ***is equal to True*** unless explicitly talking about the ***boolean value*** `True`.

1. What do these print and why?
    ```python
    truthy_values = [1, 2, 3, "hello", [1, 2, 3], {"a": 1}, True, 0, "", [], {}, None, False]

    print(“Values:”)
    for value in truthy_values:
        if value:
            print(f"{value} is truthy")
        else:
            print(f"{value} is falsy")
    ```
    - Output:
    ```
    Values:
    1 is truthy
    2 is truthy
    3 is truthy
    hello is truthy
    [1, 2, 3] is truthy
    {'a': 1} is truthy
    True is truthy
    0 is falsy
    is falsy
    [] is falsy
    {} is falsy
    None is falsy
    False is falsy
    ```
    - Using a for loop the code iterates through the list value assigned to `truthy_values`.
    - integers that are ***not 0*** are truthy.
    - strings, lists and dictionaries that are ***not empty*** are truthy.
    - the boolean value `True` is truthy.
    - All other member elements in `truthy_values` are falsy.

2. What do these print and why?
    ```python
    x = 5
    y = 10
    z = 15

    print(x > 0 and y < 20)
    print(not x == y)
    print(x < y and y < z)
    print(x > y or y > z)
    print(not (x > y))
    ```
    - `print(x > 0 and y < 20)` outputs `True` because when we use the ***ordered comparison operators*** to evaluate if the value of `x` (assigned to the integer value `5`) is greater than `0`, the expression is ***truthy***. Then `y` (assigned to the integer value `10`) is less than `20` and this expression is ***truthy*** as well. The ***logical operator*** `and` returns the boolean value `True` only if both operands in the logical expression are ***truthy***.
    - `print(not x == y)` outputs `True` because the ***unary logical operator*** `not` returns `True` is an expression is ***falsy*** and `False` if an expression is ***truthy***.
    - `print(x < y and y < z)` outputs `True` because ***both*** ***comparison expressions*** are ***truthy***.
    - `print(x > y or y > z)` outputs `False` because ***both comparison expressions*** are ***falsy*** as the logical operator `or` only needs one operand to be ***truthy*** to return `True`.
    - `print(not (x > y))` outputs `True` because the ***unary logical operator*** `not` returns `True` if an expression is ***falsy*** and `False` if an expression is ***truthy***.

3. What do these print and why?
    ```python
    a = 10
    b = 20

    print(a < b < 30)
    print(a > b or b == 20)
    ```
    - `print(a < b < 30)` outputs `True` because the integer value assigned to the variable `a` (10) is less than the integer value assigned to `b` (20) which is less than the integer value of `30`, so the ***comparison expression*** using ***ordered comparison operators*** returns the boolean value `True`.
    - `print(a > b or b == 20)` outputs `True` because the ***logical operator*** `or` requires only one operand expression to be truthy to return `True`. `a > b` is falsy, `b == 20` is truthy.

4. What do these print and why?
    ```python
    my_list = [1, 2, 3, 4, 5]
    print(3 in my_list)
    print(6 not in my_list)
    ```
    - The arguments passed into the print calls check for membership using the ***membership operator*** `in` and the ***logical operator*** `not`.
    - `print(3 in my_list)` outputs `True` because the integer value 3 is a ***member element*** of the list value referenced by `my_list`.
    - `print(6 not in my_list)` also outputs `True` because the integer value 6 is not a member element of the list value referenced by `my_list`.
        - The logical operator `not` will return `True` if a value falsy and `False` if a value is truthy.

5. What do these print and why?
    ```python
    temperature = 25
    time_of_day = "morning"

    if temperature < 30 and (time_of_day == "morning" or time_of_day == "afternoon"):
        print("It's a pleasant day!")
    else:
        print("It's either too hot or not the right time of day.")
    ```
    - An `if` statement contains a ***logical expression*** using ***ordered camparison, equality and logical expressions***. If evaluation is ***truthy*** a `print()` call outputs `It's a pleasant day!`.
    If the evaluation is ***falsy*** the `else` block is executed and outputs `It's either too hot or not the right time of day`.
    - The code outputs `It's a pleasant day` because the first, the ***ordered comparison expression*** `tempature < 30` is evaluated as ***truthy*** because the integer value `25` referenced by the variable `tempature` is less than `30`.
     Then the ***logical expression*** wrapped in parentheses is evaluated as ***truthy*** because only one of the operand expressions in the ***logical expression*** using the logical operator `or` needs to be truthy to be evaluated as ***truthy***. Since `time_of_day` is assigned to the string value `morning`, the expression in the parentheses is ***truthy***.
     The logical operator `and` requires both operands to be ***truthy*** to return the boolean value `True`. As this is the case, the expression is evaluated as ***truthy*** and the code within the `if` block is executed.

6. What does this print and why?
    ```python
    num = 12

    if num / 3 < 3 and num > 10:
        print("Hello")
    elif num >= 8 and num < 6 or num > 4 and num < 16:
        print("Hello 2")
    elif num % 4 == 0 or num < 7 and num < 10:
        print("Hello 3")
    else:
        print("Buy")
    ```
    - `Hello 2` is the output of this code as ***ordered comparison operators*** have a higher precedence than ***logical operators and the logical operator `and` has a higher precedence as `or`.
    - So the `elif` block after evaluating all the comparison expressions looks like `(True and False) or (True and True). This expression evaluates as ***truthy*** and the code within the block is executed.

## Ranges

### Basic questions:

- Is a range primitive or non-primitive?
    - ranges are ***non-primitive*** as they contain multiple integer values.
- Is a range mutable or immutable?
    - ***immutable*** as they cannot be modified after being created.
- Does range have a literal form or a type constructor?
    - ***type constructor*** as is a ***function or class*** that creates an object of a specific type.
        - `list()` returns a list object.
        - `tuple()` returns a tuple object.
        - `dict()` returns a dictionary object.
        - and `range()` returns a range object.
    - ***literals*** are anything that uses ***syntactic notation*** to directly represent an object directly in source code.
- Is a range a sequence or a collection?
    - A range is ***not*** a collection as ***collection*** is an umbrella term for any data type that contains multiple elements including ***un-ordered*** collections such as dictionarys. Sequences are collections, but not all collections are sequences.
    - A range is a ***sequence*** as sequences contain ***ordered elements*** that use ***indexing syntax*** to access or modify those member elements.
        - ***indexing syntax*** assigns every member element in a sequence a whole number (index) that can be used to access or modify the element of that index.
- What is the most common use of the range datatype?
    - ranges are commonly used to iterate over a range of numbers between two points.
Are ranges homogenous or heterogeneous?
    - ***homogenous*** as they only contain integer values.
- Why are ranges considered lazy?
    - ***yes!*** they are lazy sequences optimizing memory. Because Python does not store all the elements and only generates them when needed meaning we must ***explicitly coerce*** ranges to access their member elements.
    ```python
    my_range = range(5)
    print(list(my_range))
    # [0, 1, 2, 3, 4]
    ```

1. What do these print and why? What concept does this demonstrate?
    ```python
    print(range(0,10))
    print(len(range(5, 15)))
    print(my_range[1])
    print(str(range(3, 7)))
    print(list(range(12, 8, -1)))
    print(list(range(5, 5, 1)))
    print(5 in range(5))
    print(5 not in range(5, 10))
    ```
    - `print(range(0,10))` outputs `range(0,10)`
        - this is because the range method returns a ***range object*** which when printed represents it's ***constructor representation***.
    - `print(len(range(5, 15)))` outputs `10`.
        -  `len()` counts how many member elements are in the range.
    - `print(my_range[1])` raises an `NameError: name 'my_range' is not defined`
    - `print(str(range(3, 7)))` outputs a string, `range(3, 7)`
        -because the string function returns the ***representation*** of the range since it is a ***type constructor***.
    - `print(list(range(12, 8, -1)))` outputs `[12, 11, 10, 9]`
        - `list()` returns a list object containing the member elements of the range. The range function contains a specified start, stop (exclusive) and negative count, beginning iteration at `12`, stopping one number before stop, and iterating from the highest element to the lowest.
    - `print(list(range(5, 5, 1)))` outputs `[]`.
        -  `list()` returns an empty list because the range contains the same start and stop point, meaning the range is empty.
    - `print(5 in range(5))` ouputs `False`.
        - `range` contains only one argument, meaning that is the ***stop point***. Because the stop point is ***exclusive*** it is not included in the range. So the ***expression evaluating membership*** returns `False`.
    - `print(5 not in range(5, 10))` returns `False`.
        - `5` is the starting point of the range and is ***inclusive*** meaning it is part of the range.
        - The ***logical operator*** `not` returns the opposite of the return value of the expression.

2. What does this code print and why? What concept does this demonstrate?
    ```python
    example = range(0)
    if example:
        print(list(example))
    else:
        print(example)
    ```
    - Output: `range(0)` (an ***object representation*** of the range object assigned to `example`)
    - `example` is assigned to an empty range which is a ***falsy*** value by nature. Therefore the `if` block does not execute as it checks for ***truthiness*** which cause the code within the `else` block to run.
    - the underlying concepts in this code are ***truthy and falsy values*** and checking for them in conditional statements.

3. What does this code print and why? What concept does this demonstrate?
    ```python
    def number_range(number):
        match number:
            case n if n < 0:
                print(f'{number} is less than 0')
            case n if n <= 50:
                print(f'{number} is between 0 and 50')
            case n if 50 < n <= 100:
                print(f'{number} is between 51 and 100')
            case _:
                print(f'{number} is greater than 100')
    number_range(0)
    number_range(25)
    ```
    - Function Analysis:
        - `number_range` has one parameter and uses a ***match case block*** to determine what to output based on different conditional expressions using ordered comparison.
    - Output:
        - `number_range(0)` is called and outputs `0 is between 0 and 50`.
        - `number_range(25)` is then invoked and outputs `25 is between 0 and 50`.
    - Underlying concept:
        - match case statments
        - formatted string literals (f-strings)

## list and dictionary syntax

### Basic Questions

- What categories are lists and dictionaries?
    - lists and dictionaries are both ***collections*** while lists are more specifically ***sequences***, and dictionairies are ***mappings*** (data structures that associate *keys* with *values*).
- Are they mutable or immutable?
    - both are ***mutable*** meaning they can be modified in place after being created.
- Are they primitive or non-primitve?
    - both are ***non-primitive*** as they can contain multiple objects as member elements.
- Are they literals, or do they require type constructors?
    - both are ***literals*** as they ***use syntactic notation to be represented directly in source code.***
    - lists use `[]`
    - dictionaries use `{}`
- Are they sequences?
    - only lists are sequences as they are ***ordered***, meaning ***indexing syntax*** can be used to access or modify member elements (element reassignment)
    - dictionaries although they keep ***insertion order*** are considered ***unordered*** and therefore not a sequence.
- Does the order of the elements in both matter?
    - In lists, yes, as they are sequences.
    - In dictionaries, no, as they only keep ***insertion order*** but are still considered un-ordered.

### list methods: len(list), list.append(), list.pop(), list.reverse()

1. What does this print and why?
```python
my_list = [1, 2, 3, 4, 5]
length_of_list = len(my_list)
print("Length of the list:", length_of_list)
```
- Output: `Length of the list: 5`
    - the built-in function `len()` is used to return the total amount of member elements contained in `my_list` and is assigned to the variable `length_of_list`.
    - the `print()` call outputs two objects, a string, and the value referenced by `length_of_list`.

2. What does this print and why?
```python
lst_one = [0, 1, 2, 3]
lst_two = lst_one.append(4)
if lst_two:
    print(lst_two)
else:
    print(lst_one)
```
- Code analysis
    1. `lst_one` is assigned to a list value containing integers.
    2. `lst_two` is assigned to the return value of `.append()` method, appending the integer value 4 to the list value referenced by `lst_one`.
    3. An `if else` block conditional is used to determine output.
        - if the value referenced by `lst_two` is ***truthy***, that value is to be output.
        - if the value referenced by `lst_two` is ***falsy***, the else block runs and outputs the list referenced by `lst_one`.
- Output:
    - `[0, 1, 2, 3, 4]`
        - The list is modified by the value assigned to `lst_two`. But because the `.append()` method does not return anything, by default `None` (a ***falsy value***), the `if` conditional evaluates as ***falsy*** and the `else` conditional outputs the modifed list value referenced by `lst_one`.

3. What does this print and why?
```python
my_list = [1, 2, 3, 4, 5]
ele = my_list.pop()
print("Popped element:", ele)
print("List after popping:", my_list)
ele1 = my_list.pop(1)
print("Popped element at index 1:", ele1)
print("Modified list after popping at index 1:", my_list)
```
- Output:
    - `print("Popped element:" , ele) outputs `Popped element: 5`
        - by default, `.pop()` removes and returns the last member element of a list referenced by `my_list` and assigns it to the variable `ele`.
    - `print("List after popping:" , my_list) outputs `[1, 2, 3, 4]`
        - Lists are ***mutable*** so after using the `.pop()` method and removing the last element, the list is ***modified in place***.
    - `print("Popped element at index 1:", ele1)` outputs `Popped element at index 1: 2`
        - `ele1` is assigned to the member element removed and returned by `.pop(1)` specifying the first index in the list value referencedy by `my_list`.
    - `print("Modified list after popping at index 1:", my_list)` outputs [1, 3, 4]
- Underlying concepts:
    - List mutability
    - .pop() method

4. What does this print and why?
```python
elements = [0, 1 , 2, "Dima"]
print(elements.reverse())
print(elements)
```
- Output:
    - `print(elements.reverse())` outputs the value `None`.
        - this signifies that the  method has executed a modification to the original list value. This is possible because lists are ***mutable***.
    - `print(elements)` outputs `["Dima", 2, 1, 0]`. The modified original list.
- Underlying concepts:
    - List mutability
    - .reverse() method.
- Side note:
    - if we wanted to output the original list without modifying it in reverse we could use the built-in function `reversed()` to iterate the original list in reverse.
        - because `reversed()` is an ***iterator*** we must use the built-in function to display those member elements.
        - `print(list(reversed(elements)))` outputs `["Dima", 2, 1, 0]`.

## dictionary methods

5. What does this print and why?
```python
ages = {
    "dimo": 31,
    "olena": 32,
    "tetiana": 28
}

def get_val_of_dimo(info):
    try:
        info['dimo']
        return info['dimo']
    except KeyError:
        return "Typo"

print(get_val_of_dimo(ages))
```
- Function analysis:
    1. `get_val_of_dimo` contains one parameter `(info)`.
    2. Using a `try except block`, if the key `dimo` exists is `info`, the function returns the value mapped to the key `dimo`.
        - if the key `dimo` does not exist in `info`, a ***KeyError*** would be raised and matched with the ***except block*** and the code within will be executed returning a warning string.
- Output:
    - `print(get_val_of_dimo(ages))` outputs `31`.
- Underlying concepts:
    - key access syntax
    - exception handlying (try except statements)

6. What does this print and why?
```python
my_dict = {'a': 1, 'b': 2, 'c': 3}
keys = my_dict.keys()
print(keys)
for key in keys:
    print(key)
```
- code analysis:
    1. `my_dict` is assigned to a dictionary value containing keys and values.
    2. `keys` is assigned to a ***dictionary view object*** containing all ***keys*** in `my_dict` returned by the dictionary method `.keys()`.
    3. a `for` loop iterates over the ***dictionary view object*** assigned to `keys` and outputs each key.
- Output:
    ```python
    dict_keys(['a', 'b', 'c'])
    a
    b
    c
    ```
- Underlying concepts:
    - dictionary method `.keys()`
    - for loops to iterate over dictionaries


7. What does this print and why?
```python
my_dict = {'a': 1, 'b': 2, 'c': 3}
values = my_dict.values()
print(values)
for value in values:
    print(value)
```
- Code Analysis:
    1. `my_dict` is assigned to a dictionary object.
    2. `values` is assigned to a ***dictionary view object*** returned by the `.values()` method,  containing all ***values*** in the dictionary referenced by `my_dict`.
    3. a ***for loop*** iterates over the ***dictionary view object*** referenced by `values` and outputs a value at each iteration.
- Output:
```python
dict_values([1, 2, 3])
1
2
3
```
- Underlying Concepts:
    - .values() dictionary method (dictionary view objects)
    - for loops


8. What does this print and why?
```python
my_dict = {'a': 1, 'b': 2, 'c': 3}
items = my_dict.items()
print(items)
for key, value in items:
    print(key, value)
```
- Code Analysis
    1. `my_dict` is assigned to a dictionary object.
    2. `my_dict.items()` returns a ***dictionary view object*** containing keys and values as ***tuples***, and is assigned to `items`.
    3. a for loop loop using ***tuple unpacking*** iterates over the dictionary view object referenced by `items` and outputs the keys and values on seperate lines.
- Output:
```python
dict_items([('a', 1), ('b', 2), ('c', 3)])
a 1
b 2
c 3
```
- Underlying Concepts:
    - `.items()` method.
    - for loop using tuple unpacking to iterate over a dictionary view object.

## variable scope, global keyword, variables as pointers, variable shadowing

1. What does this print and why?
```python
name = 'John'

def greet():
    print(f"Hello, {name}!")

greet()
```
- `greet()` outputs `Hello, John!`.
    - the function definition takes no parameters but when using the formatted string literal to interpolate `name` with a string, no local variable name is found within the function. So Python searches external scopes and `name` is found in the global scope.
- Underlying concepts:
    - variable scope

2. What does this print and why?
```python
def assign():
    var = 20
    print(var)

assign()
```
- Function analysis:
    1. `assign` contains no parameters
    2. a local variable `var` is assigned to the integer value `20`
    3. The `print()` call then outputs that integer value.
- Output:
    `assign()` invocation outputs `20`.
- Underlying concepts:
    - variable scope
    - encapsulation
        - containing a variable within the functional scope without interaction with outside data (no side effects)

3. What does this print and why?
```python
try:
    print(var)
except NameError as e:
    print("Error occurred")
```
- Output: `Error occurred`
    - a `try` block attempts to output the value referenced by `var`.
    - becuase `var` is not defined, a `NameError` is raised and matched with the `except` block and executed.
        - outputs a warning string.

4. What does this print and why?
```python
var = 10

def function1():
    var = 20
    print(var)

function1()

try:
    print(var)
except NameError:
    print("Error occurred")

def function2():
    var += 5
    print(var)

try:
    function2()
except UnboundLocalError:
    print("Error occurred")

def function3():
    global var
    var += 5
    print(var)

function3()
print(var)
```
- Code analysis:
    - `var` is assigned in the ***global namespace*** to an integer value of `10`.
    - Function analysis: `function1()`
        1. takes no parameters.
        2. assigns a local variable `var` to the integer value `20`.
        3. outputs the value of the local variable.
            - this is an example of ***variable shadowing*** as the function uses the local variable instead of the global variable with the same label.
    - `function1()` is called and outputs `20`
    - a `try except` block attempts to print the value referenced by the variable `var`.
        - `var` is found in the global scope and `10` is output.
    - Function analysis: `function2()`
        1. no parameters
        2. a local variable `var` is reassigned to the value referenced by `var` + 5.
            - the local variable is output.
            - this will raise an `UnboundLocalError` as the augmented reassignment attempts to reference a local variable that has not been defined.
    - the following `try/except` block attempts to invoke `function2()` and an `UnboundLocalError` is raised and matched with the `except` block which outputs a warning string `Error occurred`
    - Function analysis: `function3()`
        1. contains no parameters
        2. `global` keyword is used to reference the value assigned to the global variable `var`.
        3. ***augmented reassignment*** is used to reassign `var` to a ***new*** integer value.
            - ***integers*** are immutable so when we modify them, a new integer value is created.
        4. The function outputs that new integer value assigned to `var`.
    - `function3()` is called and outputs `15`.
    - `print(var)` outputs `15`.
        - because the `global` keyword was used inside the function definition of `function3()`, when it is reassigned inside the definition, it affects the variable on the global scope.
    - Underlying concepts:
        - variable shadowing
        - functional scope
        - global keyword
        - immutability of integers
        - error handling (try/except statements)

5. What does this print and why?
```python
var = 10

def function1():
    print(var)

function1()

def function2():
    var = 20
    print(var)

function2()
print(var)
```
- Code analysis:
    1. `var` is assigned to the integer value `10`.
    2. `function1()` contains no parameters and outputs the value referenced by `var`.
    3. `function2() contains no parameters and a local variable `var` is assigned to the integer value `20`.
        - outputs the value referenced by the local variable.
- Output:
    - `function1()` is called and outputs `10`.
        - no matching variable is found in the local namespace but in the global namespace.
    - `function2()` is invoked and outputs `20`
        - the local variable performs `variable shadowing` as the `print()` call within the function definition uses the value assigned to the local variable, not the global variable with the same name.
    - `print(var)` outputs `10`.
        - the `print()` call cannot access data within functional scopes and because integer values are ***immutable*** and no `global` keyword is used in `function2()`, the value of the global variable `var` remains unchanged.

6. What does this print and why?
```python
def function1():
    x = 10

    def function2():
        y = 20
        print(x)

    function2()
    print(x)

function1()
print(x)
print(y)
```
- Function analysis: `function1()`
    1. local variable `x` is assigned to the integer value `10`.
    - Nested function analysis: `function2()`
        1. local variable `y` is assigned to the integer value `20`.
        2. `print(x)` outputs the value referenced by `x`.
    2. `function2()` is invoked
    3. `print(x)` outputs the value referenced by `x`.
- Output:
    - `function1()` outputs:
```python
10
10
```
        - because the nested function `function2` is invoked within `function1` and then `print()` again outputs the value of `x`.
    - `print(x)` raises a NameError as `x` is not defined in the ***global namespace*** and the print call cannot access data within functions.
- Underlying concepts:
    - nested functions
        - lexical scope
    - function scope
    - NameErrors

7. What does this print and why?
```python
var = 10

def access():
    print(var)
```
- Technically this code does not output anything as the function `access` is never invoked.
- Underlying concepts:
    - function invocation
    - variable scope
    - lexical scope