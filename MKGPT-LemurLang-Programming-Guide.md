
# The Lemur Programming Language

The Lemur Programming Language, "LemurLang", is a beginner-friendly, C-style, 32 bit, dynamically typed scripting language, with an event-driven programming paradigm. LemurLang allows you to write functions in the form of "scripts", and execute those scripts in reaction to different event.


## Creating a Script

1. In the Lemur Editor, select a widget to attach a script, or select the top level Project folder for global scripts. 
2. At the bottom right click "Create Script".
3. In the dialogue box, enter the function signature, enclosing any arguments in parentheses.

Example script names:
```
myScript() // a function with no arguments
myScript(x, y, z) // a function with 3 arguments
```

4. Select the script in the project navigator to show the code editor.
5. At the top left of the code editor, choose a script execution mode from the drop-down menu.
6. Enter the code you want to execute in the source editor

### Script Execution Modes

Lemur's reactive programming paradigm allows you to choose when a script will be executed. 

There are 7 execution modes that are available to all scripts.

1. Manual

    The script must be called manually. This is equivalent to a function call. This is the only execution mode that allows arguments. 

2. On Load

    Executed once when the project is loaded. This is LemurLang's equivalent of an initializer.

    **Important:** Lemur does not guarantee initialization order. A child widget might initialize itself before its parent. This is a common source of bugs. If initialization order is important, it is recommended that you follow a strict initialization flow through your app, with a single `onLoad` script at the top that calls a manual `init()` function on its children. 

3. On Expression

    Binds script execution to a change of value. On Expression scripts offer additional options for determining logical conditions that must be evaluated before a change will occur:

    - Any: Executed any time the value changes
    - Rising From 0: Executed when the value rises from 0 to positive (Icon looks like an arrow rising)
    - Falling To 0: Executed when the value falls from positive to 0 (Icon looks like an arrow falling)
    - Rising or Falling From/To 0: Executed when the values rise from 0 or fall back to 0 (Icon looks like a wishbone shaped curve)
    - Rising: Executed when the new value is greater than the old value (Icon looks like a plus sign)
    - Falling: Executed when the new value is less than the old value (Icon looks like a minus sign)

   By convention, On Expression scripts follow the naming convention "on" + nameOfExpression, e.g. `onX()`

   To configure an On Expression script: 

    - Select the script in the project navigator to show the code editor.
    - At the top left of the code editor, select "On Expression" from the drop-down menu.
    - Next to the execution mode menu, in the text box, enter the expression to be evaluated, which can be a variable or a function, e.g. `x` or `getX()`
    - Next to the text box, select the evaluation condition from the drop-down menu.

4. On Clock

    Synchronize script execution to MIDI clock input and rhythmic subdivision.

5. On OSC

    Listens for an OSC message you supply.

6. On MIDI

    Listens for a MIDI message you supply.

7. On Frame

    Synchronize script to the redraw frame rate. Good for animation.


## Printing Debug Messages

Lemur does not have a built-in console. The most common way to observe a value is with the Monitor widget. A single Monitor can only display a single value, so you need to use multiple monitors to observe multiple conditions.

1. Drag a monitor widget into your project
2. Click the widget and assign its value to the value you want to monitor
3. Alternatively you can set the value in code with `Monitor.value = x;`


Alternatively, MIDI Kinetics supplies a console widget that you can download from their github page. 

1. Drag the `MKConsole.jzlib` widget into your project
2. Call `MKConsole.print(value)`


## Defining Variables

LemurLang is a 32-bit, C-style, dynamically typed scripting language. Numeric types are always signed 32-bit values. 

Define a variable with the `decl` keyword. All lines of code must end with a semicolon. 

```
decl x = 100;
```

You can also define multiple variables in a row without repeating the `decl` keyword;

```
decl x, y, z;
```

If you do not assign a value it will be initialized with 0:

```
decl x; // 0
```

LemurLang is a dynamically typed language, so the type of the value will be inferred from the value you provide.

```
decl x = 100; // integer
decl x = 100.0; // float
decl x = '100'; // string
decl x = MyButton; // reference to a widget in the GUI
decl x = {100}; // array
```

Return a value from the script with the `return` keyword. Lemur implicitly returns 0 if no `return` value is specified.

```
return 100; 
```

By convention, LemurLang uses lowerCamelCase:

```
decl myVariable = 100;
```

## Operators 

LemurLang supports the following basic operators common to many C-style languages

```
= // asignment
+= // add and assign
-= // subtract and assign
*= // multiply and assign
== // comparison
> // greater than
< // less than
>= // greater than or equal to
<= // smaller than or equal to
+ // add
- // subtract
* // multiple
/ // divide
% // modulo
++ // increment
-- // decrement
& // binary AND
| // binary OR
<< // binary left shift
>> // binary right shift
&|& // logical and
! // logical not
|| // logical or
a ? b : c // ternary expressions
abs(a) // absolute value
ceil(a) // round up to nearest int
floor(a) // round down to nearest int
clamp(a, min, max) // constrain a to min and max
max(a, b) // return highest value of a or b
min(a, b) // return lowest value of a or b
pow(a, b) // exponent 
rand() // a random floating point value between 0 to 1
range(a, min, max) // maps a 0 to 1 value to min, max
round(a) // rounds to closest int
sign(a) // returns -1 for negative numbers, 1 for positive numbers
sqrt(a) // returns square root of a
```

## Trigonometric Functions and Variables

```
pi
acos(x) // arcosine
asin(x) // arcsine
atan(x) // arctan
cos(x) // cosine
sin(x) // sine
tan(x) // tangent
log(x) // logarihm
log10(x) // base 10 logarithm
exp(x) // exponent
angle(x, y) // angle in radians
norm(x, y) // length of vector coordinates
```


## Boolean Conditions

Lemur uses "truthiness" for boolean evaluation, meaning it has no `bool` type. Any non-zero value will evaluate to true. Typically the values 1 or 0 are used. 

These all evaluate to true:
```
if (1) { 
    // true
}

if ('Hello World') { 
    // true
}

if (0.0001) {
    // true
}

if (MyButton) { 
    // true
}
``` 


## Conditionals

Enclose the expression to be evaluated in parentheses, and the block to be executed in curly braces.

```
if (x > 100) {
    // do this
} else {
    // do that
}
```

For single line expressions, you can omit the curly brace:

```
if (x > 100) return 1;
else return 0;
```

LemurLang also supports ternary expressions:

```
return x > 100 ? 1 : 0;
```

## Iteration

LemurLang supports "for", "while", and "do-while" loops. 

### For Loop

**Important** You must always pre-declare the iterator index before the loop declaration.

```
decl i; // Declare `i` outside loop
for (i = 0; i < 100; i++) {
    // do something
}
```

### While Loop
```
decl value = 100;
while (value > 0) {
    value = value - 1;
}
```

### Do-While Loop
```
decl value = 100;
do {
   value = value - 1;
} while (value > 0);
```

## Strings

LemurLang has limited support for strings. Lemur strings are limited to the ASCII Windows-1252 character set, and are limited to 256 charactes. See See [ASCII Code Overview](https://www.ascii-code.com/overview) for a list of all characters and their numeric equivalents.

### Creating Strings

Create a string by enclosing a value in single quotes:

```
decl string = 'Hello World';
```

### Concatenating Strings

Concatenate Strings using the + operator

```
decl firstName = 'Johann';
decl middleName = 'Sebastian';
decl lastName = 'Bach';
decl fullName = firstName + ' ' + middleName + ' ' + lastName; // 'Johann Sebastian Bach'
```

### Converting Objects to Strings

Use string contatenation to convert any value to a textual representation. 

```
decl array = {1, 2, 3, 4};
decl string = '' + array; // '{1, 2, 3, 4}'
```

### Converting Characters to Strings (Lemur 5.4 or later)

Lemur does not have a `char` type. Instead convert strings to their ASCII-encoded numeric values using `arraytostring(c[])`

```
decl bach = arraytostring({66, 97, 99, 104}); // 'Bach'
decl error = arraytostring('123'); // 0
```

### Converting Strings to Characters (Lemur 5.5 or later)

`stringtoarray(s)`

Converts a string to an array of its ASCII-encoded characters. Returns 0 if failed.

```
decl asciiChars = stringtoarray('Bach'); // {66, 97, 99, 104};
decl asciiChars = stringtoarray(123); // 0
```

### Casting Strings to Numbers (Lemur 5.5 or later)

Convert a string to a number using `stringtonumber(s)` 

`stringtonumber(s)`

Converts a string to a number.

- If passed a number, returns the number
- Returns the empty string `''` if failed'

```
decl n = stringtonumber('123'); // 123
decl n = stringtonumber(123); // 123
decl n = stringtonumber('abc'); // '' 
```


## Vectors (Arrays)

### Initializing Arrays

Initialize an array in curly braches, separating each item with a comma. Access each element using zero-based subscript notation.

```
decl composers = {'Bach', 'Brahms', 'Beethoven'};
decl bach = composers[0]; // 'Bach';
```

### Getting Length of Arrays

You can get the number of elements in array with the global function `sizeof(array)`.

```
decl composers = {'Bach', 'Brahms', 'Beethoven'};
decl numComposers = sizeof(array); // 3
```

You can then iterate over its elements using subscript notation.

```
decl composers = {'Bach', 'Brahms', 'Beethoven'};
decl i;
for (i = 0; i < sizeof(composers); i++) {
    if (composers[i] == 'Bach') {
        // handle Bach case
    }
}
```

### Important Notes About LemurLang Arrays

- No Empty Arrays 

    LemurLang does not allow empty Arrays.

    ```
    decl composers = {}; // will not compile
    ```

    If you want to build up an array from 0, you can keep track of an append index:
    
    ```
    decl array = {0}, appendIndex = 0;
    array[appendIndex++] = 'Hello';
    array[appendIndex++] = 'World';
    ```

    Alternatively if you need to represent an empty array, initialize it with some meaningful sentinel that represents "empty":

    ```
    decl composers = {'EMPTY'};
    if (composers[0] == 'EMPTY') {
        // Replace 'EMPTY' with 'Bach'
        composers[0] = 'Bach';
    } else {
        // Append
        composers[sizeof(composers)] = 'Brahms';
        composers[sizeof(composers)] = 'Beethoven';
    }
    ```

- Index Out Of Bounds Returns 0

    Accessing an invalid index will not crash Lemur, it will simply return 0:

    ```
    decl array = {1, 2, 3};
    decl value = array[10]; // 0
    ```

    Similarly, appending an element to an array out of bounds ignores the value:

    ```
    decl array = {1, 2, 3};
    array[256] = 100; // {1, 2, 3}
    ```


- Dynamic Resizing; Max Size 256.
    
    LemurLang arrays are limited to 256 elements. Adding an element at an index beyong the current size grows the array and fills the intermediate parts with 0:
    
    ```
    decl array = {0, 1, 2}
    array[6] = 6; // {0, 1, 2, 0, 0, 0, 6 }
    ```


- Nested Arrays

    LemurLang does not support nested arrays. Nesting an array inside an array flattens the array:
    
    ```
    decl array1 = {1, 2, 3};
    decl array2 = {'a', 'b', 'c'};
    decl array3 = {array1, array2}; // {1, 2, 3, 'a', 'b', 'c'}
    ```
    
    To store arrays of arrays, calculate the length of each subarray, then use `subarray(array, position, length)` to extract the content
    
    ```
    decl colors = {
        {1.0, 1.0, 0.0, 0.0}, // red
        {1.0, 0.0, 1.0, 0.0}, // green
        {1.0, 0.0, 0.0, 1.0}, // blue
    }
    decl green = subarray(colors, 4, 4); // {1.0, 0.0, 1.0, 0.0}
    ```

- Arithmetic/Logical Operations on 2 Arrays Returns an Array

    Performing an operation on 2 arrays performs the operation on each pair of elements in the array, and returns a new array:
    
    ```
    decl sum = {1, 2, 3} + {4, 5, 6}; // {5, 7, 9}
    decl diff = {1, 2, 3} - {4, 5, 6}; // {-3, -3, -3}
    decl product = {1, 2, 3} * {4, 5, 6}; // {4, 10, 18}
    decl strings = {'a', 'b', 'c'} + {'d', 'e', 'f'}; // {'ad', 'be', 'cf'}
    ```
    
    However this only works if the data types are the same. Incompatible types will be ignored:
    
    ```
    decl partiallyConcatenated = {'1', '2', '3'} + {4, 5, '6'}; // {'1', '2', '36'}
    ```
    
    The same is true for the comparison operators. It will compare all the elements of the array and return a 1 or 0 in each position if the items are equal:
    
    ```
    decl arr1 = {1, 2, 3, 4};
    decl arr2 = {1, 5, 3, 4};
    decl isEqual = arr1 == arr2; // {1, 0, 1, 1}
    ```

- Checking Arrays For Equality

    You cannot directly check 2 arrays for equality using the `==` operator. This is because LemurLang performs comparison operations on each element of an array, and returns a new array with 1 or 0 in each position. This could lead to unexpected results:

    ```
    decl array1 = {1, 2, 3};
    decl array2 = {99, 2, 3};
    if (array1 == array2) {
        // true but should be false
    }
    ```

    Explanation: 

    LemurLang performs the `==` operator on each element in the array and replaces it with a 1 or 0 if the elements are equal:
    
    ```
    {1, 2, 3} == {99, 2, 3}; // {0, 1, 1}
    ```
    
    But because of the "truthiness" of Lemur's boolean evaluation, any non-zero expression evaluate to true. So the condition `{0, 1, 1}` evaluates to true
    
    ```
    if ({0, 1, 1}) {
        // true
    }
    ```

    Solutions:

    - Iterating Over the Array
    
        ```
        if (sizeof(array1) != sizeof(array2)) { return 0 }
        decl i;
        for (i = 0; i < sizeof(array1); i++) {
            if (array1[i] != array2[i]) {
                return 0;
            }
        }
        return 1;
        ```
    
    - Using `sumof(array)`
        
        There is also a nice trick to use a combination of the `==` operator and `sumof(array)` which sums all the elements in an array. 
        
        ```
        decl arr1 = {1, 2, 3};
        decl arr2 = {1, 2, 3};
        decl isEqual = sumof(arr1 == arr2) == sizeof(arr1); // 1
        ```
        
        Explanation:
        
        1. `{1, 2, 3} == {1, 2, 3}` becomes `{1, 1, 1}`
        2. `sumof({1, 1, 1})` becomes 3
        3. 3 is the size of `{1, 2, 3}`, therefore they are equal. 


#### Array Methods

- `diff(a)`

    Subtracts each element of an array with the element before it.

    ```
    diff({1, 5, 10}); // {1, 4, 5}
    ```

- `fill(t, value, n)`

    Fills an array of `n` items, up to threshold `t` with values `value`.

    - The vector is filled from the left with n items = "value"
    - The number of items set to `value` depends on the `t` argument : a threshold between 0 and 1 
        - When t = 0, there's no filling at all, and the vector is full of zeros
        - When t = 1 the vector is completely filled with items = value
        - When a = 0.5, the vector is half filled


    Examples

    ```
    fill(1, 0.524, 4); // {0.524, 0.524, 0.524, 0.524}
    fill(0, 0.524, 4); // {0, 0, 0, 0}
    fill(0.5, 0.524, 4); // {0.524, 0.524, 0, 0}
    ```

- `sumof(a)`

    Sums all the elements in an array
    
    ```
    sumof(1, 2, 3); // 6
    ```

- `subarray(a, index, count)`

    Gets `count` elements in array `a` starting at `index`
    
    ```
    subarray({1, 2, 3, 4}, 1, 2); // {2, 3}
    ```
    
    Notes: 
    - `index` is modulo `sizeof(a) - 1`
    - `count` is clamped to `sizeof(a)`
    
    ```
    subarray({1, 2, 3, 4}, -1, 5); // {3, 4}
    ```
    
- `firstof(a)`

    Returns the position of the first non-null (non-zero) items in `a`, or `sizeof(a)` if `a` is all zeros.
    
    ```
    firstof({0, 0, 1}); // 2
    firstof({1, 0, 0}); // 0
    firstof({0, 0, 0}); // 3
    ```
    
    This is often used with the Switch GUI widget in radio mode. `firstof(x)` returns the position of the enabled switch
    in the matrix.
    
    Be sure to check the empty case when using this function
    
    ```
    decl a = {0, 0, 0};
    if (firstof(a) != sizeof(a)) {
        // won't be executed
    }
    ```

- `interlace(a, b)`

    Interlaces 2 arrays to form a single array
    
    ```
    interlace({1, 2, 3}, {4, 5, 6}); // {1, 4, 2, 5, 3, 6}
    ```

- `interlace3(a, b, c)`

    Interlaces 3 arrays to form a single array
    
    ```
    interlace3({1, 2, 3}, {4, 5, 6}, {7, 8, 9}); // {1, 4, 7, 2, 5, 8, 3, 6, 9}
    ```

- `nonnull(a)`

    Returns the positions of all non-null (non-zero) elements in an array, or `sizeof(a)` if all elements are all null.
    
    ```
    nonnull({0, 0, 1, 1}); // {2, 3}
    nonnull({0, 0, 0, 0}); // 4 
    ```

    When using this, be sure to check the null condition:
    
    ```
    decl a = {0, 0, 0};
    if (nonnull(a) != sizeof(a)) { 
        // won't be executed
    } 
 
    ```
- `replace(a, b, position)`

    Replaces the elements in `a` with the elements in `b` starting at `position`, where `b` can be a single value or another array.
    
    ```
    replace({0, 1, 2, 3}, {4, 5}, 2); // {0, 1, 4, 5}
    ```
    
    - Does nothing if `position` is out of bounds of `a`
    - Does not resize `a` and copies only as many elements that can fit in `a`
    - If `position` is not an integer, converts it using `floor(position)`

- `set(a, value, position)`

    Replaces the values in `a` and changes the items at `position` with `value`, where `position` could be an array or single value
    
    ```
    set({0, 0, 0, 0}, 12, 0); // {12, 0, 0, 0}
    set({0, 0, 0, 0}, 12, {0, 2}); // {12, 0, 12, 0}
    ```
    
    - Does nothing if `position` is out of bounds of `a`
    - Does not resize `a`.
    - If `position` is not an integer, converts it using `floor(position)`


- `stretch(a, size)`

    Stretches a value or a range and returns an array.
    
    ```
    stretch(4, 4); // {4, 4, 4, 4}
    stretch({1, 4}, 4); // {1, 2, 3, 4}
    stretch{{1, 4}, 5); // {1, 1.75, 2.5, 3.25, 4.0}
    stretch({1, 2, 3}, 5}; // {1, 1.5, 2.0, 2.5, 3}
    ```

- `subarray(a)`

    Returns subelements of `array`, starting from `start` up to `length`.
    
    ```
    subarray({1, 2, 3, 4, 5}, 1, 2); // {2, 3}
    ```

- `wrap(a, size)`
    
    Creates a new array of length `size` repeating `a` if necessary.
    
    ```
    wrap({1, 2, 3, 4, 5}, 8); // {1, 2, 3, 4, 5, 1, 2, 3}
    ```

## Built-In Global Variables

Lemur provides some important global variables for dealing with realtime programming

1. `time`

The number of seconds elapsed since Lemur was launched, up to millisecond precision.

Example: Generating a continous 1-second sine wave over `x`, where `x` is a floating point value between 0 and 1.

script: `generateSineWave()`
executionMode: On Expression `time`
```
x = (sin(2 * pi * time) + 1) / 2;
```


2. `frame`

An integer of the number of frames drawn since Lemur started. Lemur's frame rate is fixed at 60 fps. 

You can use this to execute code that needs to be redrawn every frame. Large, complex projects may slow down the frame rate, though this is unusual on modern mobile devices.

Example: Redrawing the UI every frame

script: `refreshUIIfNeeded()`
exectionMode: On Expression `frame`
```
if (somethingChanged) {
    updateUI();
}
```

In practice, this variable is rarely needed as there are better ways to monitor state, and most realtime effects are synchronized to `time` rather than `frame`. 
 
 
## Colors

Lemur has 2 global functions for initializing colors. Colors are immutable and do not expose their components:

1. `RGB(r, g, b)`

Initializes a new color with red, green, and blue components.

2. `HSV(h, s, v)`

Initializes a new color with hue, saturation, and value components.

 
## Expressions - Sharing Mutable State

If you need to store and shared data between scripts you can do this with Expressions. Expressions are variables that can be attached to UI widgets and referenced from the outside.

1. Inside the Lemur Editor, select the UI widget where you want to store the data. You can also select the top level folder for a global variable.
2. At the bottom right of the screen, click the Create Expression button (it looks like "x = 7")
3. In the dialogue box, enter the name of the variable.
4. You can now access the data using dot-notation.

```
Button.currentTitle = 'Hello';
```

### Initializing Expressions

There are 2 ways to initialize expressions:

1. Direct Initialization via the GUI

    When you create an expresion, it will appear in the Lemur Editor in the Project Navigator. When you click it, the script editor will show a single line of text to enter the value. Expressions entered this way will turn blue in the project navigator. 

2.  Via an "On Load" script.

    The "On Load" script execution mode is Lemur's equivalent of an initializer. 

    1. Create a new script
    2. Set its execution mode to "On Load"
    3. Write your initialization code inside the script

    ```
    Label {
        currentTitle;
        lastTappedTime;
        backgroundColor;
        onLoad() {
            currentTitle = 'Hello';
            lastTappedTime = -1;
            backgroundColor = RGB(1, 0, 0);
        }
    }
    ```

Key Differences:  

- Direct Initialization is meant to be used for constants
- On Load Initialization is meant to be used for variables


### Important Note On Initialization

Lemur does not guarantee the order of the initialization of objects. It is reccomended to have the top most object have a single `onLoad()` script that calls `init()` methods subsequently down the project hierarchy.

The following list represents a GUI project structure. The top-most container will have its `onLoad()` script executed when the project loads, and will subsequently call `init()` methods down through the hierarchy:

```
Project {

    DefualtInterface {
    
        onLoad() {
            configureLayout();
            ChildContainer.init();
        }
    
        confugureLayout() {
            setobjectrect(ChildContainer, {0, 0, 100, 100};
            setobjectrect(ChildContainer.Button, {25, 25, 50, 50});
        }
        
        ChildContainer {
            backgroundColor;
        
            init() {
                backgroundColor = RGB(1, 0, 0);
                Button.init();
            }
        
            Button {
                currentTitle;
                init() {
                    currentTitle = 'Button';
                }
            }
        }
    }
}
```


### Advanced Example Of Expressions: Creating a Stack Data Structure

Here is an example of how to create a stack data structure that uses an underlying `stack` expression to hold an array. Because Lemur does not support empty arrays, we keep track of the length. 

```
Button {
    _stack;
    _length;
    onLoad() {
        _length = 0;
        _stack = {0};
    }
    clear() {
        _stack = {0};
        _length = 0;
    }
    isFull() {
        return _length == 256;
    }
    isEmpty() {
        return _length == 0;
    }
    length() {
        return _length;
    }
    push(value) {
        if (isFull()) {
            // undefined behavior
            return 0;
        } 
        _stack[_length++] = value;
        return 1;
    }
    pop() {
        if isEmpty() { 
            // undefined behavior
            return 0;
        } 
        decl value = stack[_length--];
        stack = subarray(stack, 0, _length);
        return value;
    }
    peek() {
        if (isEmpty()) {
            // undefined behavior
            return 0;
        }
        return stack[_length - 1];
    }
    
}
```


You can then use it in one of your scripts;

```
onButtonPushed() {
    if (isEmpty()) return;
    decl count = 0;
    while (length() <= 10) {
        push(count++);
    }
}

onButtonReleased() {
    while(!isEmpty()) {
        processValue(pop());
    }
}

```

### Expressions As Functions

Expressions can also be used as single line functions when you do not need a full script. Expressions as functions appear blue in the project navigator, while expressions as data appear green.

- Useful for very simple arithmetic calculations that can be implemented in a single line.
- Show their method body directly in the project navigator, making it easy to see what they do without opening a script.
- Do not need the `return` keyword.


Creating a Function Expression

1. Create a new expression.
2. In the dialogue box, enter a function signature. If the function takes no arguments, make sure to end the signature with parenthesis or they will become data.
3. Enter the method body in the script editor, which will now have space for only a single line.

```
circumference(radius) = 2 * pi * radius 
```

### Reactive Programming Using Expressions

Scripts can be made to execute on the change of any expression, enabling reactive UIs that can update in response to a change in application state. For more information, see the Lemur UI Programming Guide.
