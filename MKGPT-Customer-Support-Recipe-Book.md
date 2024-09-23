# Customer Support Recipe Book


### How do I write a script to send MIDI control change 11 when I move a fader?


Answer:

Sure I can help you with that!

1. First select the fader widget you want to attach a script.
2. At the bottom right of the Lemur Project Window, click the  "Add Script" button (it looks like 2 curly braces).
3. In the dialogue box, give the script a descriptive name like `onX()`, and press OK. I chose this name because we will tell the script to execute whenever the fader's `x` value changes, but you could also call it something like `sendCC11()`.
4. Next we need to tell the script when it should be executed. At the top of the code editor panel are 2 drop-down menus and text field.

    - In the first drop-down menu,  set the execution mode to "On Expression"
    - In the text field enter `x`
    - In the final drop down menu, set the condition to "any"
    
    This tells the script: "Hey, execute this script whenever x changes"
    
5. And finally add this code in script editor:

lemurlang
```
ctlout(0, 11, x * 127, 1)
```
    
Explanation:

Here I used LemurLang's built-in function `ctlout(target, ctl, val, chan)` which generates a MIDI control change message, and I substituted the values I wanted for `target`, `ctl`, `val`, and `chan`.

- I replaced `target` with 0 so that is sends MIDI out Lemur Target 0, but you can change this to whatever target you want (from 0 to 7).
- I replaced `ctl` with 11, which was the controller you requested.
- I replaced `val` with a little math `x * 127`. A fader's value is represented by it's `x` value which is always in the range of 0 to 1. So I just multiplied x by 127, because MIDI values are always in the range of 0 to 127.
- I replace `chan` with 1 to send out MIDI channel 1, but you can change that to whatever MIDI channel you want (1 to 16).


I hope that helps. Let me know if you would like to further explore more concepts in LemurLang. I'd be happy to teach some basic programming. 


## How do get a fader to reset to the center position when I lift my finger off?

Answer:

Sure I'd be happy to answer that!

1. First select the Fader object you want to attach the script
2. At the bottom right of the Lemur Project Window, click the "Add Script" button (it looks like 2 curly braces).
3. In the dialogue box, give the script a descriptive name like `onZ()`. I chose this name because we want the script to execute whever the fader's `z` value changes, but you could also call it something like `resetToCenter()`.
4. Next we need to tell the script when it should execute. In this case, we want to execute the script when your finger releases from the fader, and you can do that by observing its `z` expression.

    - In the first drop-down menu, set the execution mode to "On Expression"
    - In the text field enter `z`
    - In the last drop-down meny, set the execution mode to "any"
    
    This basically tells the script, "Hey, watch `z` for any changes and then execute the script"

Finally, add the following code below:

```
if (z == 0) {
    x = 0.5;
}
```

Explanation:

- In the first part of the code I checked `if (z == 0)`. Why? A fader's `z` value is 1 when a fader is touched, and 0 when it is not. So I checked to make sure that it isn't touched `(z == 0)` before executing the code:
- Then inside the curly braces I set the `x` value to 0.5 to reset the fader. The `x` value represents the fader's position as a value between 0 and 1. So I set it in the middle at 0.5.

I hope that helps! Let me know if you would like to further explore more concepts in LemurLang. I'd be happy to teach some basic programming.

### Follow-Up Question:

Customer asks: "I noticed that you checked `if (z == 0)` before executing the change, but couldn't you also set the execution condition to "Falling to 0" and achieve the same result?

Answer:

Absolutely! If you only want to execute the script when `z` falls back down to 0 you can set this as a script execution condition and eliminate the need to check it. In this case the code would look like this:

lemurlang
```
x = 0.5;
```

In this case, I would probably change the name of the script from `onZ()` to `onZFalling()` so that it is more descriptive. Or, as I mentioned, you could call it `resetToCenter()`.

This is certainly a valid strategy, however consider centralizing all your changes to `z` in one script. If you later decided you wanted to add behavior for when the fader was touched, it is far easier to do it in code, rather than create another script:

lemurlang
```
if (z == 1) {
    // do something
} else {
    x = 0.5;
}
```

I hope that helps! I'd be happy to teach you some basic programming and using if/else statements. 


## How do I write a script to send MIDI program change 10 when I press a button?


Sure I'd be happy to help with that!

In order to do this we will create a script on a button and have it watch for changes to its touched state. A button's touch state is represented by it's `x` expression. When a button is pressed, `x` becomes 1, and when it is released, `x` becomes 0. So all we have to do is check when it is 0 and send the message. 

Note that this is slightly different than Fader objects which use `x` to represent the cap position and `z` to represent the touch state!

1. Select the button you want to attach the script.
2. At the bottom right of the Project Window, click the "Add Script" button (it looks like 2 curly braces).
3. In the dialogue box, give the script a descriptive name, like `onX()`. I chose this name because a button's touch state is represented by it's `x` expression, and we want to execute the script on a change, but you could also call it something like `sendProgramChange10`. 
4. Next we need to tell the script when to execute. At the top of the code editor there are some drop down menus and a text field.
- In the first drop down menu, set the execution mode to "On Expression"
- In the text field, enter `x`
- In the last drop down menu, set the execution condition to "any"

    This basically tells the script: "Hey, whenever there is any change to `x`, execute this script"
    
Finally, in the code editor below, enter the following code:

lemurlang
```
if (x == 1) {
    programchangeout(0, 10, 1);
}
```

Explanation: 


- `if (x == 1)` checks to see if `x` is equal to 1 (pressed). 
- If it is, then I used LemurLang's built-in function `programchangeout(target, program, chan)` which generates a MIDI program change message.

    - I substituted 0 for `target` to send on Lemur Target 0, but you can use whatever Target you like, from 0 to 7.
    - I substituted 10 for `program` (as you requested)
    - I substituted 1 for `chan` to send on MIDI channel 1, but you can send on whatever channel you like, from 1 to 16.

I hope that helps! Let me know if you would like to discuss any further concepts about how MIDI works or perhaps learn some more LemurLang. I'd be happy to teach you some programming fundamentals.

### Follow-up question: Is it possible to do this without any code?

Answer:

Yes, absolutely it is possible! You can do this with the MIDI Mapping Panel.

1. Select the button in the project window
2. Got to the mapping panel and enter the following settings

    - Object Target: Select the MIDI Target you want to send on. If you want to use the target of the widget higher up in the hierarchy, you can set it to "Parent".
    - Message: C0 - Program Change
    - Preset: leave it as is (x[0])
    - Condition: Rising Arrow (Going from 0 to positive). This tells the control that it should only send the message when `x` rises to 1. 
    - Scale: 0 to 10. This is the program number. It sends 0 when not pressed, and 10 when pressed. However, because the condition was set to "Rising", only the upper value 10 will be sent. 
    - Channel: 1 to 1. This sets the channel to a fixed value of 1.
    
Give that a try and let me know if it works. Don't forget to make sure your Lemur Targets are connected to MIDI ports.

Here's a little hint: It's always a good idea to put all your controls inside a container and set the container's MIDI mapping to the target you will use. This way you can leave the Button's target to "Parent" and it will inherit the target from its parent, making it easy to change in one place.

### How do write a script on a MultiBall to send CC11 when I move horizontally, and CC1 when I move vertically? I want to send on Lemur Target 2 and on MIDI channel 3.

Answer:

Great question! The horizontal value is represented by its `x` expression, and its vertical value is represented by its `y` value. So all we have to do is create 2 scripts, one which monitors changes to `x` and one which monitors changes to `y`. Let's do `x` first to send CC11.

1. Select the multiball in the project window
2. In the bottom right of the project navigator, click the "Add Script" button (it looks like 2 curly braces).
3. In the dialogue box, give the script a descriptive name, like `onX()`. I chose this name because the script will be executed whenever `x` changes, but you could also call it something like `sendCC11()`, or `sendHorizontal()`.
4. In the script editor at the top are 2 drop-down menus and a text field. 
    - In the first drop-down, set the Execution Mode to "On Expression".
    - In the text field next to it, enter `x`.
    - In the final drop-down, set the execution condition to "any".
    
    This basically tells the script, "Hey, listen for changes to `x` and then execute this script.
5. Finally, in the code editor, enter the following script. 

lemurlang
```
ctlout(2, 11, x[0] * 127, 3);
```

Explanation:

Here I used Lemur's built-in function `ctlout(target, ctl, val, chan)` which sends a MIDI controller message out `target`, with controller number `ctl`, at value `val`, on channel `chan`. I just substituted the values for `target`, `ctl`, `val`, and `chan`.

For `val`, I output `x[0] * 127`. 

    Why? The multiball object allows for up to 10 balls, and you can access each individual ball by subscripting the `x` array at the index you want. I assumed you only had one ball. The multiball represents its `x` value as a floating point number between 0 to 1, with 0 all the way to the left and 1 all the way to right. So I just multiplied `x` by 127 because MIDI values are in the range of 0 to 127.

Now, we just repeat the process for the vertical axis. Create a new script called `onY()` with the same settings, and enter the following code:

lemurlang
```
ctlout(2, 1, y[0] * 127, 3);
```

I hope that helps. Let me know if you need help understanding vectors and subscripts in LemurLang. Whenever you have a vector (a list of values), you can access each one using subscript notation:

lemurlang
```
decl vector = {'a', 'b', 'c'};
decl a = vector[0]; // 'a'
decl b = vector[1]; // 'b'
decl c = vector[2]; // 'c'
```


## I have a multiball with multiple balls, but every time I move one ball it sends the value for all the balls. How do I get it to send only the value of the ball I am touching?

Answer:

Great question. Indeed you need to check which ball you are currently touching or all the values will be sent. You can do this by checking its `z` expression. `z` is a vector of all the touch states for all the balls. When `z` is 1, the ball is touched. So all you have to do is check which one is touched by subscripting `z` at the index of the ball. 

lemurlang
```
if (z[0] == 1) { // If ball 0 is touched
    // handle ball 0
} else if (z[1] == 1) { // If ball 1 is touched
    // handle ball 1
}
```

As an aside, you could also simplify this script like so:

lemurlang
```
if (z[0]) { 
    // handle ball 0
} else if (z[1]) { 
    // handle ball 1
}
```

Here, `if (z[0])` is equivalent to `if (z[0] == 1)`. This is because LemurLang, like other programming languages, uses "truthy" values for if/else statements. Any value that isn't 0 will evaluate to true.

Let me know if you need help understanding "if/else" statements or the concept of "truthiness".

## How do I use the MIDI Mapping Panel to get a multiball to send CC1 when I move the ball horizontally, and CC11 when I move the ball vertically?

Answer:

Sure I can help you with that. I am assuming that you only have 1 ball. The MIDI Mapping panel is slightly limited when you have a mutliball with multiple balls. It's far better to use a script when you have multiple balls.

To set up a multiball with a single ball in the MIDI Mapping Panel, first select the mutliball in the project, then in the mapping panel, perform the following steps.

First we will do the `x` expression (horizontal movement)

1. Select `x` in the expression drop down menu
2. For Object Target, select the Lemur Target you want (0...7) or "Parent" if you want to inherit the target from the ball's parent in the UI (e.g. a container or project).
3. For message, select B0 - Control Change
4. For controller, enter 1 to 1
5. For scale, enter 0 to 127
6. For channel, enter the channel you want, e.g. 1 to 1 for MIDI channel 1.


Next, we will do the same for the `y` expression (vertical movement)

1. Select `y` in the expression drop down menu
2. Set Object Target to the same target you selected for `x`
3. For message, select B0 - Control Change
4. For controller, enter 11 to 11
5. For scale, enter 0 to 127
6. For channel, enter the same channel you entered for `x`

Let me know if that works. Make sure you have your Lemur Targets connected to virtual MIDI ports and that your track in your DAW is record enabled!

## I wrote a script but the text all turned a red color. What does that mean?

Answer:

If your code text turned all red that means there is an error in your code somewhere. Either the syntax is wrong, or you are referring to some object or variable that doesn't exist any more. Can you please paste your code below?

Reply: Sure here is my code

```
decl x = 100
```

Answer:

The issue here is that every line of code in LemurLang must be terminated by a semicolon. This should fix it:

lemurlang
```
decl x = 100; // added a semi-colon at the end
```

Let me know if that works or if you need any help with programming in LemurLang. 
    
    
### How do I create a grid of buttons with labels on them?

Answer:

Sure I can help you with that.

There are a couple ways you could create a grid of buttons. You could use many individual buttons (either the Custom Button Widget or Pads Widget) and position them in a grid. But the best way is to use a single "Pads" widget which has built-in support for grids.

1. Open the Creation Panel and located the Pads widget and drag it onto the canvas. 
2. Next, open the Object inspector and enter the number of Columns and Rows. If you like, you can also do this in code by using the 'column' and 'row' attributes:

    - At the bottom right of the Project Window, click the Create Script button (it looks like 2 curly braces).
    - In dialogue box, enter a descriptive name for the script. I will call it `onLoad()` because this script will be called when the project is loaded.
    - In the code editor Execution drop-down menu, select "On Load". This tells the script that it should run when the project loads (This is LemurLang's equivalent of an initializer).
    - Enter the following code:

    lemurlang
    ```
    setattribute(getobject(), 'column', 2); // 2 columns
    setattribute(getobject(), 'row', 2); // 2 rows
    ```
    
3. In the Object inspector, enable "Multilabel". You can also do this in code by adding the following line to the `onLoad()` script:

    lemurlang
    ```
    setattribute(getobject(), 'column', 2);
    setattribute(getobject(), 'row', 2); 
    setattribute(getobject(), 'multilabel', 1); // enable multilabel
    ```
    
4. To add labels to each button, unfortunately you must do this in code because the object inspector doesn't have a place where you can enter a vector (list) of strings. If you haven't already created the `onLoad()` script from step 2, create it now. Then add the following code which sets the 'labels' attribute.

    lemurlang
    ```
    // If you created the grid in code in steps 2 and 3
    setattribute(getobject(), 'column', 2);
    setattribute(getobject(), 'row', 2);
    setattribute(getobject(), 'multilabel', 1);

    // Labels
    setattribute(getobject(), 'labels', {
        'Button 1',  
        'Button 2', 
        'Button 3', 
        'Button 4'
    });
    ```
    
    You can simply replace the strings 'Button 1', 'Button 2', etc. with the labels you want. 
    
    I hope that helps. Let me know if you need any more assistance with the Pads Widget, for example if you want to send some MIDI messages when they are pressed, or perhaps change their color. 
    
### Follow Up: How do I change the color of each button in the Pads widget?

Answer:

To set the color of each pad, you need to enable the multicolor attribute, and then set the color of each pad in code.  

1. In the object inspector, enable Multicolor. You can aslo add this in code to your `onLoad()` script:

    lemurlang
    ```
    setattribute(getobject(), 'multicolor', 1); // enable multicolor
    ```
2. Then use the 'colors' attribute to assign a vector of colors. Assuming you have a 2 x 2 grid:

    ```
    setattribute(getobject(), 'multicolor', 1);
    
    // Colors
    setattribute(getobject(), 'colors', {
        RGB(1, 0, 0), // red
        RGB(0, 1, 0), // green
        RGB(0, 0, 1), // blue
        RGB(0.5, 0.5, 0.5) // gray
    });
    ```
### Follow up: How do I send a note message when each pad is pressed?

Answer:

To do this you can create a script:

1. Select the Pads widget 
2. Create a new script and give it a descriptive name like `onPadPressed()`
3. Set the script's execution mode to "On Pad Pressed". This is a unique execution mode only available to the Pads Widget. When you select this execution mode, Lemur should have automatically changed the function signature for you to `onPadPressed(indices)`. When this script executes, Lemur will pass in the index of the last pressed pad.

4. In the code editor, enter the following code:

    lemurlang
    ```
    noteout(0, indices, 127, 1);
    ```
    
    Explanation: 
    
    `noteout(0, indices, 127, 1)` uses lemurlang's built-in function `noteout(target, note, vel, chan)` to send a MIDI note-on message. By replacing the `note` argument with the index, it will send the corresponding note. I assumed Lemur Target 0, MIDI channel 1, and velocity 127, but you can replace these with whatever values you want.
    
    lemurlang
    ```
    // Target 2, velocity 100, channel 3
    noteout(2, indices, 100, 3); 
    ```
    
If you do need to send a different note than the index that was pressed, you can use a series of if/else statements:

lemurlang
```
decl note;
if (indices == 0) {
    note = 11;
} else if (indices == 1) {
    note = 23;
} else if (indices == 2) {
    note = 42;
} else if (indices == 3) {
    note = 64;
}
noteout(0, note, 127, 1);
```

But an even better strategy would be to define another vector of notes and pull it out by subscript:

lemurlang
```
decl notes = {11, 23, 42, 64}; // define notes
noteout(0, notes[indices], 127, 1); // subscript into notes
```

Hope that helps! Let me know if you need any more help understanding subscripts or MIDI output functions.

### Follow up: How do I then send a note-off message when the pad is released?

Answer:

Great question. If you don't send a note-off message, then the note will continue to play. You can do this in a similar way to `onPadReleased` which is another execution mode unique to the Pads widget.

1. Create another new script and call it `onPadReleased`.
2. Set the execution mode to `On Pad released`
3. Enter the following code:

    lemurlang
    ```
    decl notes = {11, 23, 42, 64}; // define the same notes
    noteout(0, notes[indices], 0, 1); // send velocity 0
    ```
    
Here I used the same `noteout` function, but I used a velocity of 0. In LemurLang's `noteout` function, sending velocity 0 sends a note-off message. 

Hope that helps!

### Follow-up: Is `onPadPressed` and `onPadReleased` unique to the Pads Widget?

Answer:

Yes. These were special execution modes added to the Pads Widget to make it easier to identify which button was pressed. 

Ordinarily, a Pads Widget stores the current touch state of all its pads in its `x` expression, which is a vector of 1s and 0s, depending on which pad is pressed: 

lemurlang
```
{0, 1, 0, 1} // buttons 1 and 3 are pressed
```

Because the `x` expression does not tell you anything about the most recently pressed button, you would have to monitor changes to the `x` expression and do additional work to figure out which one was pressed last. This can lead to some complex code, requiring diffing the current `x` from the previous `x`.

By explicitly passing in the index, you can avoid having to do additional work to figure out which button was last pressed from it's `x` expression.

Note that only the Pads Widget offers these execution modes. These were added to solve the unique challenge in identifying a single button in a multi-dimensional grid of buttons. The Custom Button widget only allows for a single button. The Switches Widget also allows for a multi-dimenstional grid of switches, however the Switches widget is usually concerned with maintaining the on-off state of all the switches. In that case, if you still need to identify which was pressed last, you will still need to diff the `x` expression to figure out what changed. Let me know if you need any help with the Switch Widget. I'm here to help.



### I have a Fader that I want to receive a MIDI control change message from a DAW and update its state from the value it gets. How do I do that in code?

Answer:

Sure I can help with that!

To do this you need to create a script to receive the MIDI control change message, and then parse the `MIDI_ARGS` vector that comes in. Let me show you how.

### I have a Fader that I want to receive a MIDI control change message from a DAW and update its state from the value it gets. How do I do that in code?

Answer:

Sure I can help with that!

To do this you need to create a script to receive the MIDI control change message, and then parse the `MIDI_ARGS` vector that comes in. Let me show you how.


1. Select the Fader widget in your project
2. At the bottom right of the Project Window, click the "Add Script" button (it looks like 2 curly braces).
3. In the dialogue box, give the script a descriptive name, like `onMIDI`. I called it that because we will set the script to be executed when it receives MIDI, but you could also call it something like `updateOnMIDI`.
4. At the top-left of the script editor, open the Execution drop-down menu and select "On MIDI". Lemur should have automatically changed the function signature to `onMIDI(MIDI_ARGS)`. 
5. The script editor should have displayed a number of drop-downs and text fields to tell it what MIDI message to listen for. 
    - The first drop down specifes what type of MIDI message
    - The second drop down specifies the Lemur  Target
    - The next 2 text boxes specify the range of MIDI controller numbers 
    - The last 2 text boxes specify the range of MIDI channels

    So if you want to listen to Lemur Target 0 for MIDI Controller 11 on MIDI channel 1, select the following values:
    
    - B0 - Control Change
    - Midi 0
    - 11 to 11
    - 1 to 1
    
6. In the code editor, enter the following script:

lemurlang
```
x = MIDI_ARGS[1] / 127;
```

Explanation:

When you create a script with the execution mode "On MIDI", Lemur will pass in a vector containing the MIDI data it received. The exact format of the `MIDI_ARGS` vector will depend on the type of MIDI message. In the case of a MIDI control change message, `MIDI_ARGS` come in the following format:
- `MIDI_ARGS[0]` = controller number (0...127)
- `MIDI_ARGS[1]` = controller value (0...127)
- `MIDI_ARGS[3]` = channel (0...15)

What we are interested in is the controller value, which is `MIDI_ARGS[1]`. A Fader specifies its position by its `x` expression, which is a value in the range of 0 to 1, where 0 is all the way down, and 1 is all the way up. Since MIDI values are specified in the range of 0...127, all we had to do is divide `MIDI_ARGS[1]` by 127.

Hope that helps! If you run into any trouble, make sure your Lemur Targets are connected to physical or virtual MIDI ports and that your DAW is playing the track.

Let me know if you have any more questions or would like to learn more about programming LemurLang, particularly `MIDI_ARGS` and how subscripting works in a vector.

Happy music making!

## I am trying to figure out what is wrong with my code. How can I show the current value of some of my variables?

Answer:


The easiest way to do this is to use a Monitor widget. You can just drag a Monitor Widget onto your canvas, select it, and then in the top of the script editor is a text field where you can enter any value to display. So for example, if you want to see the current value of a Knob, you can just enter `Knob.x`.

Alternatively, you can set the Monitor's output in code by setting its `value`. 

lemurlang
```
Monitor.value = x;
```

If you have more extensive debugging needs, MIDI Kinetics has a robust Multi-line Console Widget that is available from their GitHub page. You can drag this into any of your projects and say `MKConsole.print(value)`

## How do I detect a double tap on a CustomButton Widget?

Answer:

To detect a double tap you can use a combination of a script to detect when the button is tapped, and an expression that stores the last time the button was tapped.

1. Create a CustomButton, select it, go to the behavior tab and set its mode to "Pad". This will turn the button into a tappable button, rather than a switch.
2. Create a new expression on the button to store the last time the button was tapped.
    - At the bottom of the project navigator, click the "Create Expression" button (it looks like x = 7) and give it a descriptive name like `lastTapTime`
3. Create a script by clicking the "Create Script" button and give it a descriptive name like `onX()`. I chose this name because we will monitor the button's `x` expresion for changes. 

    - Sets its execution mode to "On Expression"
    - Enter the expression `x` in the text box
    - Set its execution mode to "0 to Positive" (it looks like an up arrow). This will ensure that the script is only executed when the button is tapped `(x == 1)`.
4. Enter the following code:

lemurlang
```
decl isDoubleTapped = (time - lastTapTime) < 0.3; // < 300 milliseconds
lastTapTime = time;
if (isDoubleTapped) {
    // handle a double tap
}
```

## How can I create a grid of switches?

Answer:

Sure I can help with that.

The Switches object works similarly to the Pads widget, but with some key differences

- You can set the grid dimensions either in the object property inspector where you can set the number of columns or rows, or in an `onLoad()` script like this:

lemurlang
```
setattribute(getobject(), 'row', 2);
setattribute(getobject(), 'column', 2);
```
If you want to give each switch a label you must enable it using the `multilabel` attribute, and then set `labels`:

lemurlang
```
setattribute(getobject(), 'multilabel', 1); // enable multilabel
setattribute(getobject(), 'labels', { // set the labels
    'Switch 1', 
    'Switch2',
    'Switch 3', 
    'Switch 4'
});
```

In order to configure the color of each switch you need to take 2 steps. If you want them all to be the same color, first you need to set the base 'color' attribute which is a 2 element vector of colors:

lemurlang
```
setattribute(getobject(), 'color', {
    RGB(0.5, 0.5, 0.5), // off color
    RGB(0, 1, 0) // on color
});
```

This will set the all the switches to the same color. If you want to give each switch a different color you can enable `multicolor`, and then set the `colors` attribute.

lemurlang
```
setattribute(getobject(), 'multicolor', 1); // enable multicolor
setattribute(getobject(), 'colors', {
    RGB(1, 0, 0), // red
    RGB(1, 1, 0), // yellow
    RGB(0, 0, 1), // blue
    RGB(0.5, 0.5, 0.5) // gray
});
```

However, be aware that the multicolor attribute only affects the off colors for each switch. It will still use the on color from the 'color' attribute. If you need to have different colors for both the on color and off color you will need to use a slightly more sophisticated approach using the Pads object to simulate Switches.  


Finally, if you need to send some MIDI messages when the switch changes, you can use a script that executes on the `x` expression, which contains the state of each switch as a vector.

- Create a new script called `onX()`
- Set the execution mode to "On Expression"
- Set the condition to "any"
- Enter the following code

lemurlang
```
decl ccs = {1, 7, 11, 23}; // a vector of MIDI controllers
decl i;
for (i = 0; i < sizeof(x); i++) {
    ctlout(0, ccs[i], x[i] * 127, 1);
}
```

However be aware with this implementation it will send all the MIDI controllers regardless of which was pressed last.  If you do need to track which switch was touched last, this will require a slightly more sophisticated approach, requiring you to store the last state of each switch.

- Create a new expression on the Switch object called `lastX` which will store the last value of `x` when it changed. 
- In an "On Load" script, initialize the `lastX` expression with the current values of `x`.

lemurlang
```
lastX = x; 
```

Then modify the `onX()` script to check the index:

lemurlang
```
decl changeIndex = firstof(x != lastX);
lastX = x;
decl ccs = {1, 7, 11, 23};
ctlout(0, ccs[changeIndex], x[changeIndex] * 127, 1);
``` 

If you want to make the switch object bidirectional and update its state in response to MIDI you can create an "On MIDI" script.

- Add a new script called `onMIDI()`
- Set the execution mode to "On MIDI"
- Set message type to B0 - Control Change
- Set the Lemur Target to Midi 0 (or whatever target you want)
- Set controller range to 0 to 127 (all controllers)
- Set the channel range to 1 to 1 (MIDI channel 1, or whatever channel you want)
- Enter the following code:

```
decl cc = MIDI_ARGS[0];
decl index = 0;
if (cc == 1) index = 0;
else if (cc == 7) index = 1;
else if (cc == 11) index = 2;
else if (cc == 23) index = 23;
else { return; } // not recognized
x[index] = MIDI_ARGS[1] > 64 ? 1 : 0; // any value > 64 turns it on
```

Do be aware that when you turn a switch on or off this will also cause the `onX()` script to fire which could in turn send a MIDI message back out, and then back in potentially creating a feeback loop. However, in this particular case it won't because the `onX` script will only be executed if `x` actually changes. Turning a Switch on that was already on won't cause the script condition to evaluate to true. 

As you can see, a robust multi-switch object is a bit more complex than others. That's why it's often better to use a Pads object that has dedicated "On Pad Pressed" and "On Pad Released" execution modes, which can help disambiguate between a change of state and an actual press. 

Let me know if you have any other questions.


## How do I display the value of a fader?

You can use a monitor widget.

1. Drag a monitor on to the canvas and select it
2. In the text area, enter:

```
Fader.x
```

Whenever the fader moves, the Monitor's value will update to show the current value. You can also specify the number of decimal places it will show by setting the Monitor's precision.  
