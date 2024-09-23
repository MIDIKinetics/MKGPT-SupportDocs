# Lemur UI Programming Guide


## Structure of a Lemur UI

A Lemur UI forms a hierarchical tree structure. 

- At the root level is the *Project*. This is analogous to a document, and what is saved to disk.
- A project can have multiple *Interfaces*. An Interface represents a full screen. 
- Interfaces can have *Objects*, which are UI widgets such as Buttons, Faders, and Text.
- Use a "Container Object" to create nested hierarchies.

Example:

- Project  
  - Interface1  
    - Button  
    - Text  
  - Interface2  
    - FaderContainer  
      - Fader1  
      - Fader2  
      - Fader3  

## Creating a New Project

Click the file icon at the top left of the Lemur Editor, or use the key command CMD+N.

### Adding an Interface

Click the Create Interface button at the bottom left of the Project Tree Navigator window. The icon looks like 2 overlapping rectangles.

### Adding Objects

Drag an object from Creation Pallete window onto the interface. 

### Adding a Container

1. Drag a Container object onto the interface window
2. Drag objects from the Creation Pallete into the container
3. To move existing objects into a container
    - Select the objects in the UI and cut them to the clipboard (CMD+X)
    - Select the container
    - Paste them in from the clipboard (CMD+V)



## Referencing Objects in Code

The easiest way to get a reference to an object is to reference it by name:

```
decl button = MyButton;
```
    
Use dot-notation to get child objects:

Example:

- Project  
  - Interface  
    - Container  
      - Container  
         - Button  

Code:
                
```
decl button = Container.Container.Button;
```

### Special Handling of Interfaces and Projects

1. Interfaces and Projects are pseudo-objects that are not referenceable in code.

    Example:
    
    - Project  
      - Interface  
        - Button  
    
            
    Code:
    ```
    // Not possible
    decl project = Project; 
    decl interface = Interface;
    decl button = Interface.Button
    ```
  
2. Root objects in interfaces are siblings

    Example:  Button, Fader, and Text are siblings.
    
    - Project  
      - Interface1 
        - Button 
      - Interface2  
        - Fader  
      - Interface3  
        - Text  
    
    
3. Scripts and expressions at the project level are in the global namespace.

    Example:
    
    - Project  
        `globalScript()`  
        `globalExpression = 0`.   
        - Interface1  
        - Interface2  
    

    Avoid code in the global namespace. Prefer a container to encapsulate shared data.

    Example: 

    - Project  
        - Interface  
          - AppTheme  
            - `defaultTextColor = RGB(1, 1, 1)`  
            - `defaultFontSize = 16`  
            
4. Interfaces cannot have scripts or expressions.

    
## Naming Rules and Conventions

1. Siblings must have unique names

    Not Allowed:
    
    - Project  
        - Interface  
            - Container  
            - Container // Duplicate of Container  
        - Interface // Duplicate of Interface  
    

    Corrected:


    - Project  
        - Interface  
            - Container  
            - Container2 // OK  
        - Interface2 // OK  
 
2. Names follow C-style naming conventions and cannot start with numbers, nor contain special characters or spaces:

    Not allowed:
    
    ```
    Button 1 // contains space
    1Button // starts with number
    Button-1 // contains minus sign
    ```
    
    Corrected:
    
    ```
    Button1
    Button_1
    ```
    
    **Exception for Interfaces**

    Interface names are regular strings and can contain any characters, but must be unique. This is because Interfaces are pseudo-objects and not referenceable in code.


    - Project  
        - My First Interface   
        - 2 - My Second Interface  
        - A Possible Interface ?  
        



## Object Scope Rules

1. Objects have direct access to siblings and ancestors:

    Example:
    
    - Project  
        `script1() { }`    
        `expression1 = 0;`   
        - Interface     
            - Container  
                `script2() { }`  
                `expression2 = 0;`    
                - Button  
                    `script3() { }`    
                    `expression3 = 0;`    

    Code:

    ```
    script3() {
        script1();         // Calls script1() 
        expression1 = 1;   // Sets expression1 to 1
        script2();         // Calls Container.script2()
        expression2 = 2;   // Sets Container.expression2 to 2
        expression3 = 3;   // Sets Button.expression3 to 3
    }
    ```

2. Parents must use an explicit path to children:

    *Example:*
    
    - Project  
        `execute() { }`    
        `expression = 0;`   
        - Interface    
            - Container  
                `script() { }`  
                `expression = 0;`    
                - Button  
                    `script() { }`    
                    `expression = 0;`    
    
    *Code:*
    ```
    execute() {
        Container.expression = 1;
        Container.script();
        Container.Button.expression = 1;
        Container.Button.script();
    }
    ```

3. Objects are shadowed by their nearest ancestor, with local members taking precedence:

    Example:
    
    - Project  
        - `script() { }`  
        - `expression = 0;`  
        - Interface  
            - Container  
                - `script() { }`    
                - `expression = 0;`    
            - Button  
                - `execute() { }`  
                - `expression = 0;`  
        
    Code:
    ```
    execute() {
        expression = 1; // Sets Button.expression to 1
        script();       // Calls Container.script()
    }
    ```
    
    To avoid shadowing, use an explicit path:

    ```
    execute() {
        Container.expression = 1; // Sets Container.expression to 1
    }
    ```


4. Interfaces are pseudo objects with no scope. Their direct children are siblings.

    *Example: Container and Container2 are siblings*
    
    - Project  
        - Interface1  
            - Container  
        - Interface2  
            - Container2  


## Traversing Hierarchies in Code

LemurLang also provides a number of functions for traversing relative hierarchies:

- `getobject()`

    Returns a reference to the current object.

    ```
    decl this = getobject();
    ```

- `getparent(object)`

    Returns a reference to the parent of `object`, or 0 if no parent.
    
    Example:
    
    - Project  
        - Interface  
            - Container  
                - Button  
    
    ```
    decl container = getparent(Button); // Container
    ```
    
    **Important:**
    
    The topmost parent is the interface. 
        
    ```
    decl inteface = getparent(Container); // Interface
    ```

- `getnext(object)`

    Returns a reference to the next sibling of `object`, or 0 if no sibling. Siblings are returned in lexicographical order.
    
    Example:
    
    - Project  
        - Interface  
            - Container  
                - Button1  
                - Button2  
                - Button3  
                
    Code:
    
    ```
    decl object = getfirst(Container); // Button1
    while (object) {
        object = getnext(object); // Button2, Button3, 0
    }
    ```
    
    **Important:**
    
    It is not possible to traverse through interfaces using `getnext`.

 - `getfirst(object)`
 
    Returns a reference to the first child of `object`, or 0 if no children. Children are returned in lexicographical order.
    
    Example:
    
    - Project  
        - Interface   
            - Container  
                - Button1  
                - Button2  
                
    Code:
    
    ```
    decl button1 = getfirst(Container); // Button1
    decl first = getfirst(Button2); // 0
    ```


- `findchild(object, name)`

    Performs a breadth-first traversal of all the descendants of `object`, and returns a reference to the first descendant named `name`, where `name` is a string. Returns 0 if no child found.   
    
    Example:
    
    - Project  
        - Interface  
            - Container  
                - Container  
                    - Button1  
    
    Code:
    
    ```
    decl button1 = findchild(Container, 'Button1'); // Button1
    decl button2 = findchild(Container, 'Button2'); // 0
    ```
    
### Hierarchy Traversal Examples

1. Finding the topmost object in an interface

    Hierarchy:
    
    - Project  
        - Interface   
            - Container1    
                - Container2    
                    - Container3    
                        `findParent()`    
    
    Code:
    ```
    findParent() {
        decl object = getobject(); // Container3
        while (getparent(object)) {
            object = getparent(object);
            // iteration 1: object = Container2
            // iteration 2: object = Container1
            // iteration 3: object = Interface
        }
        return getfirst(object);
    }
    ```

2. Getting all siblings

    Hierarchy:
    
    - Project  
        - Interface    
            - Container    
                - Button1    
                - Button2  
                - Button3  
                - Button4  
                `getAllButtons()`    
                
    Code:
    ```
    getAllButtons() {
        decl this = getobject();
        decl index = 0;
        decl buttons = 0; // array
        decl object = getfirst(this);
        while (object) {
            buttons[index++] = object;
            object = getnext(object);
        }
    }

    ```
    
3. Getting all descendants

    Hierarchy:
    
    - Project  
        - Interface  
            - Root  
                `onLoad()`  
                `children(object, index)`    
                - Container1  
                    - Container2  
                    - Container3  
                - Container4  
                    - Container5  
                    - Container6  
                    
                    
    Code:
    
    ```
    onLoad() {
        decl children = children(getobject(), 0);
        // {Container1, Container2, Container3, Container4, Container5, Container6}
    }
    
    children(object, index) {
        decl result;
        decl child = getfirst(object);
        while (child) {
            result[index++] = child;
            decl children = children(child, 0);
            if (children) {
                decl j;
                for (j = 0; j < sizeof(children); j++) {
                    result[index++] = children[j];
                }
            }
            child = getnext(child);
        }
        return result;
    }
    ```
## Programmatic Interface Selection

- `selectinterfae(index)`

    Displays the interface at `index`

## Programmatic Layout

### Setting Object Position

LemurLang provides two methods, `getobjectrect(object)` and `setobjectrect(object, rect)` to enable programmtic layout. Both methods assume the origin in the top-left.

- `getobjectrect(object)` 

    Returns the frame of `object` relative to its parent's coordinates as an array in the form of {x, y, width, height}


- `setobjectrect(object, rect)`

    Sets the frame of `object` to `rect`, where `rect` is an array in the format `{x, y, width, height}`.
    

### Showing/Hiding Objects

- `show(object, isShown)`
    
    Shows or hides `object` (1 = shown, 0 = hidden)
    
### Tips:

1. Getting an object's bounds

    Use the array method `replace(arr, value, pos)` to set the x and y to 0 represent the interior bounds of an object
    
    ```
    decl bounds = replace(getobjectrect(object), {0, 0}, 0);
    ```

2. Containers

    The outer stroke of a container takes up 8 pixels, so get the inner bounds of a container, subtract 16 from the width and height (8 from each horizontal side, and 8 from each vertical side).
    ```
    decl containerBounds = getobjectrect(Container) - {0, 0, 16, 16};
    ```


### Code Example: Grid Layout

Hierarchy:

- Container  
    `layout()`  
    - Button1  
    - Button2  
    - Button3  
    - Button4  
    - Button5  
    - Button6  
    - Button7
    - Button8  
    
Code:

```
layout() {
    decl this = getobject();
    decl bounds = replace(getobjectrect(this), {0, 0}, 0) - {0, 0, 16, 16};
    decl columns = 2;
    decl rows = 4;
    decl spacing = 2;
    decl columnWidth = (bounds[2] - ((columns - 1) * spacing)) / columns;
    decl rowHeight = (bounds[3] - ((rows - 1) * spacing)) / rows;
    
    decl tx = 0, ty = 0, i = 0, button = getfirst(this);
    for (i = 0; i < rows * columns; i++) {
        setobjectrect(button, {tx, ty, columnWidth, rowHeight});
        if (i > 0 && (i + 1) % columns == 0) {
            tx = 0;
            ty += rowHeight + spacing;
        } else {
            tx += columnWidth + spacing;
        }
        button = getnext(button);
    }
}
```

## Configuring Widgets

Lemur comes with a variety of UI widgets such as Buttons and Faders and Knobs. Each of these have their own set of configuration options that can be modified either in the Lemur Editor's Objects Panel, or programmatically in code. Refer to the Lemur User Guide for a full listing of all their configuration options. 

1. Drag a widget from the Creation Panel onto the Lemur Editor UI
2. Select the widget
3. Open the Objects panel to see the available options

### Custom Widgets
It is also possible to create completely new widgets using Lemur's *Canvas Widget*, which is based on  HTML5 Canvas. See the Lemur User Guide Addendum.


### Programmatic Configuration

### Attributes

Attributes represent a widget's built-in behavioral and stylistic configuration options. Generally these correspond to the options in the Objects Panel. LemurLang provides 2 methods , `getattribute` and `setattribute`, for changing these programmatically:

- `getattribute(object, name)`

    Returns the attribute named `name` from `object` where `name` is the string representation of the attribute, or 0 if the attribute doesn't exist.
    
    Example: Checking if a Switch Widget is in radio mode:
    ```
    decl isRadioMode = getattribute(Switches, 'radio');
    decl is
    ```
- `setattribute(object, name, value)`

    Sets the attribute named `name` to `value` on `object`, where `name` is the string representation of the attribute. The type of `value` depends on the attribute.
    
    Example: Setting the text on a Text Widget
    
    ```
    setattribute(Text, 'content', 'Hello World');
    ```
Important Points:

- The string representations usually differ from the name that appears in the Objects Panel. Check the user guide for a listing of all the available attributes.
- Not all attributes are configurable in code. For example, it is not possible to change the font size of a Text widget programmatically (you can use a Canvas Widget if you need this).

### Expressions

Expressions differ from attributes in 2 ways:

1. Expressions typically represent the state of an interactive control. 
    
    For example, a Fader Widget's `x` expression represents the cap position of the fader using a floating point value in the range of 0...1; It's `z` expression represents its touched state (1 = touched, 0 = not touched).

2. You can add *new* expressions to widgets to create new behaviors. You cannot add an attribute. 


LemurLang provides both `getexpression` and `setexpression` to changed these programmatically:

- `getexpression(object, name)`
    
    Gets the current value of the expression `name` on `object` where `name` is a string, or 0 if it doesn't exist.

    ```
    decl x = getexpression(Fader, 'x');
    decl q = getexpression(Fader, 'q'); // 0, Fader doesn't have a `q`
    ```
    
- `setexpression(object, name, value)`
    
    Sets the expression named `name` on `object` to `value`, where `name` is a string. The type of `value` depends on the type of expression.

    ```
    setexpresion(Fader, 'x', 1.0);
    ```
    
### Creating an Expression

1. Select the widget in the Editor UI
2. Click the "Create Expression" button in the Project Navigator (the button looks like "x=7")
3. Give the expression a name, and click OK

### Code Example: 
Desired Behavior: 

Setting all Faders in a container to their center position, but only if they are not touched. 

Project:

- Container   
    - `resetAllFadersIfNotTouched()`  
    - Fader1   
        -`x`  
        -`z` 
    - Fader2   
        -`x`  
        -`z` 
    - Fader3   
        -`x`  
        -`z` 

Code:

```
resetAllFadersIfNotTouched() {
    decl this = getobject() // Container
    decl fader = getfirst(this) // Fader1
    while (fader) {
        if (getexpression(fader, 'z') != 1) {
            setexpression(fader, 'x', 0.5);
        }
        fader = getnext(fader);
    }
}
```

## Reactive Programming

Lemur features rich support for reactive programming paradigms to create event-driven UIs that allow widgets to react to changes in the application state. This is achieved through *Script Execution Modes*, which allow script execution to be bound to any expression. 

There are 7 execution modes that are available to all scripts. 

1. Manual
2. On Load
3. On Expression
4. On Clock
5. On OSC
6. On MIDI
7. On Frame

For a complete description of each, refer to the LemurLang Programming Guide. See also the Lemur MIDI Programming Guide to see examples of how to react to MIDI messages

### Widget-Specific Execution Modes

Some UI widgets have additional execution modes that are designed for their specific use. These options will be available in their Execution Mode drop down menu when you create a script on one of these widgets.

Example:
1. The Canvas widget has an `onRedraw` execution mode which will be called on every frame when the widget is set to redraw mode `always`, or when manually called through `canvas_refresh(canvas)`
2. The `Pads` widget has 2 execution modes for reacting to button presses, `onPadsPressed(indices)` and `onPadsPressed(indices)`, which supply the indices of the currently tapped pad.

For a complete listing of all the widgets and their available options, see the Lemur User Guide.

### Code Examples

#### 1. Binding a change in color to an error code 

Project:

- Container  
    - `errorCode`  
    - `colorForErrorCode(errorCode)`
    - Fader1  
        - `onErrorCode()`   
    - Fader2  
        - `onErrorCode()`   
    - Fader3  
        - `onErrorCode()`  
        
Code:

```
colorForErrorCode(errorCode) {
    if (errorCode == 0) {
        return RGB(0, 1, 0);
    } else if (errorCode == 1) {
        return RGB(1, 1, 0)
    } else {
        return RGB(1, 0, 0);
    }
}
onErrorCode() {
    // Exection mode: "On Expression"
    // Expression: errorCode
    // Condition: any
    setattribute(getobject(), 'color', colorForErrorCode(errorCode));
}
```

Use:

```
Container.errorCode = 2; // All faders turn red
```


#### 2. Updating layout in response to change of size

Project:

- Container 
    - `onGetObjectRect()`  
    - Text  

Code:
```
onGetObjectRect() {
    // Execution Mode: On Expression
    // Expression: getobjectrect(getobject())
    // Condition: any
    
    decl frame = getobjectrect(getobject());
    decl bounds = replace(frame, {0, 0}, 0) - {0, 0, 16, 16};
    setobjectrect(Text, {bounds[0], bounds[1], bounds[2], bounds[3]})
}
```

Use:
```
setobjectrect(Container, {0, 0, 100, 100}); // Fills Text to container
```


#### 3. Binding to `time` to create a sine wave

Project:

- Fader  
    - `onTime()`  

Code:

```
onTime() {
    // Execution Mode: On Expression
    // Expression: time
    // Condition: any
    
    decl amplitude = 1;        
    decl offset = 1;           
    decl frequency = 1/2;        
    decl scalingFactor = 2;    
    x = (amplitude * sin(2 * pi * frequency * time) + offset) / scalingFactor;
}
```

#### 4. Binding to `time` to animate horizontal position

Project:
- Container  
    - Text  
        - `onTime()`  
        - `onLoad()`   
        - `animationStartTime`
        - `startAnimation()`  
        - `stopAnimation()`

Code:

```
onLoad() {
    // Execution Mode: On Load
    stopAnimation();
}

stopAnimation() {
    animationStartTime = -1;
}

startAnimation() {
    animationStartTime = time;
}

onTime() {

    // Execution Mode: On Expression
    // Expression: time
    // Condition: any

    // Return if animtion stopped
    if (animationStartTime == -1) return;
    
    // Set desired animation duration in seconds
    decl duration = 0.5;
    
    // Calculate progress by normalizing from 0...1
    decl progress = (time - animationStartTime) / duration;
    
    // Abort if animation ended
    if (progress > 1) {
        stopAnimation();
        return;
    }
    
    // Apply sine easing function to make smoother
    progress = sin((progress * pi) / 2);
    
    // Calculate frame 
    decl this = getobject();
    decl frame = getobjectrect(this);
    decl parentFrame = replace(getobjectrect(getparent(this)), 
            {0, 0}, 0) - {0, 0, 16, 16};
    decl startX = parentFrame[0];
    decl endX = parentFrame[2] - frame[2];
    decl currentX = (endX - startX) * progress;
    
    // Move
    setobjectrect(this, {currentX, frame[1], frame[2], frame[3]});
}
```

Use:

```
Text.startAnimation();
```



## Important Final Points

### Stroring an object in a variable erases its type


Though it is possible to set an expression or call a script directly using dot-notation, this is only possible if you have a direct reference to an object:

*OK:*
```
Fader.x = 0.5;
Fader.myProperty = 42;
Fader.myFunction();
```

Storing an object in a variable erases its type. Once you store the object in a variable, it has been *type-erased*, and there is no casting operator in LemurLang to retrieve it.

*Not Possible:*
```
decl fader = Fader;
fader.x = 0.5; // won't compile
fader.myProperty = 42; // won't compile
fader.myFunction(); // won't compile
```

The same is true for references acquired through the hierarchy traversal methods, `getobject`, `getparent`, `getnext`, or `findchild`:

*Not Possible:*
```
decl fader = findchild(Container, 'Fader');
fader.x = 0.5; // won't compile
```
Instead, use `getattribute`, `setattribute`, `getexpresion`, and `setexpression`. These methods will attempt to get or set the value if it exists, or fail silently if they don't. 

*OK*:
```
decl fader = findchild(Container, 'Fader');
setexpression(fader, 'x', 0.5); // Fader.x = 0.5
setexpresion(fader, 'q', 1); // Does nothing
```

### No Dynamic Method Dispatch
Additionally, there is currently no way to call a script dynamically. There is no such equivalent to `setexpression` for methods.

*Not Possible:*
```
callFunction(fader, 'myScript', args); // No such method
```

Workaround:


If you must call a function dynamically, one workaround is to create a script that executes when a value changes. This is a sort of implemenation of a lamda object.

Behavior: Display the sum of `a` and `b` in a Text widget.

Project:
- TextWidget  
    - `onLoad()`   
    - `a`  
    - `b`
    - `sum`
    - `trigger`  
    - `execute()`  

Code:

``` 
 onLoad() {
     // Execution Mode: "On Load"
     
     trigger = 0;
     a = 0;
     b = 0;
     sum = 0;
 }
 
 execute() {
    // Execution Mode: "On Expression"
    // Expression: trigger
    // Condition: Rising
     
    if (trigger == 0) return; // prevents initial trigger
     
    sum = a + b;
    setattribute(Text, 'content', '' + sum);
 }
 
```
Use:

```
// Find the widget dynamically
decl textWidget = findchild(Container, 'TextWidget');

// Set up the values to sum
setexpression(textWidget, 'a', 2);
setepression(textWidget, 'b', 3);

// Increment `trigger` which causes `execute` to fire.
setexpression(object, 'trigger', getexpression(object, 'trigger') + 1));

// get the sum if you need it
decl sum = getexpression(textWidget, 'sum');
```

If you find yourself needing this type of operation, consider the costs associated with its complex setup. LemurLang's reactive programming paradigm often makes such a workaround unnecessary. 

### No Dynamic Memory Allocation


Finally, it is **not possible to create widgets, scripts, or expressions programmatically**. Lemur doesn't have a `new` keyword. All components of a Lemur UI must be created via the Lemur Editor.

*Note: This is different than the original Jazzmutant Lemur Hardware Device, which did have this ability. However, this controller device has been discontinued.*
