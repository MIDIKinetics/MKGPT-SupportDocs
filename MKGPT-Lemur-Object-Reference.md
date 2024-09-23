# Lemur Object Reference

Reference guide to the various widgets available for desinging a Lemur UI. 

For information on the Canvas, StepSlider, StepSwitch, and StepNote widgets, refer to the Lemur User Guide Addendum.

### Colors

The value of the color attributes can range from 0 to 8355711, or using the RGB(r,g,b) or HSV(h,s,v) internal functions to specify color component arguments in a 0 to 1 range eg RGB(0.5,0.5,0.5).

## Breakpoint

The Breakpoint Object is a multi-segment envelope editor, which assigns each finger to track one of a number of points (up to 64) in a rectangular space. If the edit Option is activated, new points can be created in real-time by double-touching the object’s surface. By default the object constraints a point’s horizontal displacement so that it does not travel further than its left neighbour (x[2]>=x[1]>=x[0]), but that can be deactivated by ticking the free form Option.

### Expressions

- **x**: A vector containing the points’ horizontal positions.
- **y**: A vector containing the points’ vertical positions.
- **z**: A vector containing the touch state of each point.

### Properties

- **Name**: Name of the Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed on the Object.
- **Live Edition**: If checked, new points can be added by double-touching the object’s surface.
- **Points**: Number of points (1 to 64).
- **Coordinates**: If checked, x and y coordinates are displayed when touched.
- **Background**: Choose the color of all the points except the first, together with the outline around the Breakpoint.
- **First point**: Choose the first point’s color with the color picker.
- **Light**: Controls the luminosity of the points. -2 means black, +2 means white, and you can choose any decimal number in-between.

### Behavior

- **Grid**: If checked, the range of values produced by the Breakpoint is quantized into [grid] steps. The maximum number of steps for the Breakpoint’s axis is 33.
- **Free Form**: If checked, points can be displaced freely on the horizontal axis, without the default constraint of moving no further than its left neighbor.
- **Physics**:
  - **None**: Points follow finger positions immediately. Attraction and Friction parameters are ignored.
  - **Interpolate**: Points follow the fingers’ positions according to the value of Attraction.
  - **Mass-Spring**: Attraction and Friction are both active, with Friction affecting how long points keep moving.
  - **Super-Spring**: The segments between points also behave as springs, with the Rest parameter setting their initial and resting lengths.
- **Attraction**: The amount of attraction the cursors (your fingers) have on the points.
- **Friction**: The amount of friction the Object’s surface applies on the points.
- **Speed**: This value multiplies the points’ speed after Physics computation by a user-defined expression.
- **Rest**: When Super-Spring mode is enabled, this sets the initial and resting lengths of the segments between the points.
- **HoldX**: Freezes the respective point on its current position on the x-axis.
- **HoldY**: Freezes the respective point on its current position on the y-axis.

### Attributes

- **color**: The color of the background and first point.
- **coordinates**: 0 or 1, coordinates off/on.
- **editable**: 0 or 1, live edition off/on.
- **free**: 0 or 1, free form off/on.
- **grid**: 0 or 1, grid off/on.
- **grid_steps**: Number of grid steps on the x and y axis (1 to 33).
- **label**: 0 or 1, label off/on.
- **name**: The name of the object.
- **nbr**: Number of points (1 to 64).
- **physic**: Physics mode (0 to 3).
- **rect**: Object’s position and dimensions {X,Y,W,H}.
- **zoom**: Zoom in object (1 to 50).


### Additional Execution Modes

None

## Container

Containers can enclose any number of objects inside them, including other Containers. A Container can also be made tabbed by ctrl-clicking/right-clicking on it and choosing the option "Make tabbed." Additional tabs can then be added with the option "Add tab," and their respective content displayed by touching the corresponding tab. The tabs’ order can be modified by ctrl-clicking/right-clicking within a Container and choosing "Bring up" or "Bring down."

### Expressions

None

### Properties

- **Name**: The Container's name, used as a prefix for addressing objects inside it (e.g., `Container/Fader.x`).
- **Lock**: Prevents editing and display of the Container's content in the Project panel when checked.
- **Transparent**: If checked, objects lying "under" the Container (but not belonging to its content) are shown, and the Container's frame is not displayed.
- **Color**: The color of the Container's frame.

### Behavior

None

### Attributes

- **color**: Integer, color of the container (range 0 to 8355711).
- **label**: 0 or 1, label off/on.
- **name**: Text, Container name.
- **rect**: {X,Y,W,H}, object's position and dimensions.
- **tabbar**: 0 or 1, tabbar off/on.
- **transparent**: 0 or 1, transparent off/on.

### Additional Execution Modes

None


## Custom Button

The Custom Button can act as a pad or switch. You can set the text of the button for the on and off states. Alternatively, you can have the state displayed by two different geometric forms.

### Expressions
- **x**: The state of the button.

### Properties
- **Name**: The name of the object, also used as its default OSC address.
- **Style Off**: A menu for choosing how the Off state is depicted (text or symbols).
- **Style On**: A menu for choosing how the On state is depicted.
- **Font**: A menu for choosing the font size for the text (8pt to 24pt).
- **Alignment**: A graphical menu for choosing the position of the text within the object boundaries. 
- **Color Off/On**: Colors for the respective button states.
- **Light**: Controls the luminosity of the button. -2 means black, +2 means white.

### Behavior
- **Capture**: Determines if the object reacts only to cursors created within its area.
- **Mode**: Determines if the button acts as a Switch or a Pad. A Switch changes state on touch and doesn't revert. A Pad changes its state back when the finger is removed.

### Attributes
- **behavior**: 0 or 1 (Switch or Pad mode)
- **bitmap**: {0 to 14, 0 to 14} (Decoration bitmap in off state, in on state)
- **capture**: 0 or 1 (Capture off/on)
- **color**: {integer*, integer*} (Color for off state, color for on state)
- **label_off**: text (Button text in off state)
- **label_on**: text (Button text in on state)
- **name**: text (Object name)
- **outline**: 0 or 1 (Outline off/on)
- **rect**: {X, Y, W, H} (Object’s position and dimensions)


### Additional Execution Modes

None


## Fader

The Fader tracks your finger with a virtual cap and generates a value corresponding to the position of the cap on the fader. The Fader can be oriented vertically or horizontally. Grab and drag a corner to change its orientation.

### Expressions

- **x**: The location of the cap. When the cap is at the top or right-most position of the fader (depending on orientation), the value is 1 by default. When it is at the bottom or at the left-most position, respectively, the value is 0.
- **z**: A flag variable for detecting if the Fader is being touched. 1 if touched, 0 if untouched. This is useful to emulate fader touch of control surfaces.

### Properties

- **Name**: The name of the Fader Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Value**: If checked, the current value of the Fader is displayed in the Interface. In addition, you can enter a formula for how the value is displayed. This does not affect the actual value sent by the Fader, which remains between 0 and 1.
- **Unit**: This user-specified text is appended at the end of the value display. Use it to specify the type of value, as in dB or ms.
- **Precision**: Specifies the number of decimal places for the value display. The default value is 3 and the maximum number is 6. This setting has no influence on the actual output of the Object. You have to scale the output using expressions or on the Target side.
- **Color**: Pick a color for the fader’s body; its cap always keeps the pink outline.

### Behavior

- **Grid**: If checked, the range of values produced by the Fader is quantized into [grid] steps. The maximum number of steps for the Fader is 33.
- **Capture**: If Capture is checked, an Object will only react to cursors that were created inside its area. Even if the cursor later leaves the Object for another position, it will remain in control of the original Object, until it is destroyed eventually. When Capture is off, the old-school way from previous versions is restored, meaning an Object will react to whatever cursor is present at any moment in its area.
- **Cursor Mode**: 
  - **Limited**: The Fader will respond to a new cursor only if the original one has been destroyed (i.e., finger is raised).
  - **Get Newer**: Whenever a new cursor appears inside the Object’s area, it gains full control of the Object.
  - **Barycentric**: Each cursor, old or new, has the same amount of influence on the Object.
  - **Cap Only**: The Object acts like a conventional fader that doesn’t react to cursors outside of the cap area.
- **Physics**: 
  - **None**: The Fader cap tracks one finger immediately. If other fingers touch the fader, they are ignored.
  - **Interpolate**: The cap moves according to the value of Attraction from its current location to the location of your finger. Larger values for attraction (up to 1) cause the cap to move to the new finger location more quickly. When Attraction is set to 0, the cap cannot be moved by your finger.
  - **Mass-Spring**: Attraction and Friction are both active. Friction ranges between 0 and 1. Lower values of friction mean that if the cap is moving it will tend to keep moving. With a value of 0, the cap will never stop moving. At a value of 1, the cap exactly follows your finger. Values of 1 for Attraction and Friction are essentially the same as if Physics is set to None.

### Additional Execution Modes

None


### Attributes

- **capture**: 0 or 1 (capture off/on)
- **color**: integer* (color of the object)
- **cursor**: 0 to 3 (cursor mode)
- **grid**: 0 or 1 (grid off/on)
- **grid_steps**: 1 to 33 (number of grid steps)
- **label**: 0 or 1 (label off/on)
- **physic**: 0 to 2 (physics mode)
- **precision**: 0 to 6 (decimal places of displayed value)
- **name**: text (Object name)
- **rect**: {X,Y,W,H} (object’s position and dimensions)
- **unit**: text (unit of displayed value)
- **value**: 0 or 1 (value off/on)
- **zoom**: 1 to 50 (zoom in object)

## Knob

The Knob Object emulates two types of knobs: Classic knobs constrained between a min and max value (0 and 1 by default), and endless encoders that can reach any value through multiple successive turns.

### Expressions
- **x**: The current value of the knob, based on the absolute angle position in Classic mode, or rotation count in Endless mode.

### Properties
- **Name**: The name of the Knob Object, also used as its default OSC address.
- **Endless Knob**: If checked, the knob switches to endless mode.
- **Label**: If checked, the Object’s name is displayed above the knob.
- **Value**: If checked, the current value of the Knob is displayed in the Interface. Additionally, you can enter a formula for how the value is displayed. This does not affect the actual value sent by the Knob, which remains between 0 and 1.
- **Unit**: This user-specified text is appended at the end of the value display. Use it to specify the type of value, as in dB or ms.
- **Precision**: Specifies the number of decimal places for the value display. The default value is 3, and the maximum number is 6. This setting has no influence on the actual output of the Object. You have to scale the output using expressions or on the Target side.
- **Background**: Pick the Knob’s background color.
- **Foreground**: Pick the Knob’s foreground color.

### Behavior
- **Grid**: If checked, the range of values produced by the Knob is quantized into grid steps. The maximum number of steps for the Knob is 33.
- **Capture**: If Capture is checked, an Object will only react to cursors created inside its area. Even if the cursor later leaves the Object for another position, it will remain in control of the original Object until it is destroyed. When Capture is off, the original behavior is restored, meaning the Object will react to whatever cursor is present at any moment in its area.

### Attributes
- **capture**: 0 or 1, capture off/on.
- **color**: Integer* color of the object.
- **cursor**: 0 to 3, cursor mode.
- **grid**: 0 or 1, grid off/on.
- **grid_steps**: 1 to 33, number of grid steps.
- **label**: 0 or 1, label off/on.
- **physic**: 0 to 2, physics mode.
- **precision**: 0 to 6, decimal places of displayed value.
- **name**: Text, Object name.
- **rect**: {X,Y,W,H}, object’s position and dimensions.
- **unit**: Text, unit of displayed value.
- **value**: 0 or 1, value off/on.
- **zoom**: 1 to 50, zoom in object.

### Additional Execution Modes

None


## Leds

The Leds Object is a two-dimensional array of Leds. It can be a single led or a whole matrix of them, all with their individual light and value parameters. Remember that those properties can be set to vectors when dealing with several columns or rows of Leds.

The Leds are transparent to touch but not transparent in terms of display. This means you can implement switch or pad functionality by placing a hidden Switches or Pads Object underneath the Leds.

### Expressions

- **value**: Represents the state of each Led (0 for off, 1 for on). In Bargraph mode, it represents the percentage of Leds switched on. If Bargraph mode is off, only 1 and 0 are accepted as values. If you have multiple rows/columns, you can use a vector to individually address the Leds.

### Properties

- **Name**: The name of the Leds Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed above the Leds.
- **Transparent**: If checked, the Leds' background is transparent.
- **Columns**: The number of columns of Leds contained in the Object. Only 16 columns of Leds can be set.
- **Rows**: The number of rows of Leds contained in the Object. Only 16 rows of Leds can be set.
- **Multicolor**: If checked, the ‘colors’ attribute of the Object can be set to a different value for each Led.
- **BarGraph**: The Bar Graph mode makes a matrix or vector of Leds act as your typical bar graph: the Leds work together to graphically display the current value.
- **Color Off**: Pick a color for the Leds’ off-state.
- **Color On**: Pick a color for the Leds’ on-state.
- **Light**: Controls the luminosity of your Objects. It can be a constant, a vector, or any mathematical expression. -2 means black, +2 means white, and you can choose any decimal number in-between.

### Behavior

- **None**: No additional behavioral properties are listed for the Leds Object.

### Attributes

- **bargraph**: 0 or 1 (bargraph off/on).
- **color**: {integer*, integer*} (color off, color on).
- **colors**: {integer*, integer*, ...} (vector of colors for multicolor use).
- **column**: 0 to 16 (number of columns).
- **label**: 0 or 1 (label off/on).
- **name**: text (Object name).
- **rect**: {X,Y,W,H} (object’s position and dimensions).
- **row**: 0 to 16 (number of rows).
- **transparent**: 0 or 1 (transparent off/on).

### Additional Execution Modes

None


## Menu

The Menu Object is a "pop-up" menu selection tool that can display a list of up to 32 individual text items. By default, the object outputs the index of the selection.

### Expressions
- **selection**: The current index of the selection (1,2,3,4…)

### Properties
- **Name**: The name of the Menu Object, also used as its default OSC address.
- **Scale Output**: Scale output to MIDI values.
- **Transparent**: If checked, the object's background becomes transparent.
- **Font**: Text content font size.
- **Alignment**: A graphical menu for choosing the position of the text within the boundaries of the Menu's cells.
- **Color**: The color of the object.
- **Items**: A list for editing the menu's content.

### Attributes
- **color**: integer* - Object's color.
- **items**: {text,text…} - Menu items.
- **name**: text - Object name.
- **rect**: {X,Y,W,H} - Object’s position and dimensions.
- **scale**: 0 or 1 - Scale output off/on.
- **transparent**: 0 or 1 - Transparent off/on.


## Monitor

The Monitor object is designed to display information. It does not generate data when touched. 

It is one of the most useful widgets for helping debug: 

```
Monitor.value = x;
```

If you need a more robust console for printing messages, visit the MIDI Kinetics github page and download the MKConsole widget.

### Expressions
- **None**: The Monitor object does not have any expressions associated with it.

### Properties
- **Name**: The name of the Monitor object, also used as its default OSC address.
- **Label**: If checked, the object's name is displayed above the value.
- **Value**: This checkbox is non-functional. The text field next to Value represents the monitor’s default value. Any value can be sent to the monitor, and there is no 0-1 limitation.
- **Unit**: Appends arbitrary text to the end of the value display.
- **Precision**: Number of floating-point digits displayed. A precision of 0 shows only the integer part of numbers the monitor receives.
- **Font**: A menu for choosing the font size for the label and value of the monitor. The font size ranges from 8pt to 24pt.
- **Alignment**: A graphical menu for choosing the position of the text within the boundaries of the object. There are 9 different positions to choose from.
- **Transparent**: If checked, only the label and value are displayed, and the monitor's background becomes transparent. This feature can turn the monitor into a text label anywhere in the interface.
- **Color**: Select the color of the monitor.

### Attributes
- **color**: `integer*` — The color of the object.
- **label**: `0 or 1` — Label on/off.
- **name**: `text` — The name of the object.
- **precision**: `0 to 6` — Number of decimal places of the displayed value.
- **rect**: `{X,Y,W,H}` — The object's position and dimensions.
- **transparent**: `0 or 1` — Transparent on/off.
- **unit**: `text` — Unit of displayed value.
- **value**: `0 or 1` — Value on/off.

### Additional Execution Modes

None


## MultiBall

The MultiBall Object assigns each finger to track one of up to 10 balls in a rectangular space. Balls can either always be visible or only appear when you touch the space; the latter is called ephemeral mode. The brightness of the balls is sent as the `z` expression in the Object.

### Expressions
- **x**: A list of the horizontal positions of all the balls.
- **y**: A list of the vertical positions of all the balls.
- **z**: A list of the brightness values of all the balls. Brightness values change only when the MultiBall Object is in ephemeral mode.

### Properties
- **Name**: The name of the MultiBall Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Numbers**: If checked, a running number is displayed on each ball.
- **Multilabel**: If checked, individual labels may be used for the Object’s parts.
- **Multicolor**: If checked, individual colors may be used for the Object’s parts.
- **Balls**: Number of balls (1 to 10).
- **Color**: Pick a color for the MultiBall’s frame.

### Behavior
- **Grid**: If checked, the range of values produced by the Balls is quantized into [grid] steps. The maximum number of steps for the MultiBall axis is 33.
- **Ephemeral**: If checked, the MultiBall behaves in a mode where the balls disappear until you touch it with one or more fingers. The brightness of the balls becomes the value of the `z` variable of the Object.
- **Capture**: If Capture is checked, an Object will only react to cursors that were created inside its area. Even if the cursor later leaves the Object, it will remain in control of the original Object until it is destroyed.
- **Cursor Mode**: 
  - **Limited**: Balls will respond to a new cursor only if the original one has been destroyed.
  - **Barycentric**: Each cursor has the same amount of influence on the Object.
  - **Get Oldest Ball**: If all the balls are currently controlled by your fingers and a new cursor comes in, it will take control of the oldest ball.
  - **Get Closest Ball**: If all the balls are currently controlled by your fingers and a new cursor comes in, it will take control of the closest ball.
- **Physics**:
  - **None**: Balls move to finger positions immediately.
  - **Interpolate**: Balls move toward finger positions according to the value of Attraction.
  - **Mass-Spring**: Attraction and Friction are both active, simulating a spring-like motion.

### Attributes
- **ball_enable**: 0 or 1 (ball enable off/on).
- **capture**: 0 or 1 (capture off/on).
- **color**: Integer (color of the Object’s frame).
- **colors**: Vector of colors for multicolor use.
- **cursor**: 0 or 1 (cursor off/on).
- **ephemere**: 0 or 1 (ephemeral off/on).
- **grid**: 0 or 1 (grid off/on).
- **grid_steps**: Number of grid steps on the x and y axis (1 to 33).
- **label**: 0 or 1 (label off/on).
- **labels**: Vector of labels for multilabel use.
- **multicolor**: 0 or 1 (multicolor off/on).
- **multilabel**: 0 or 1 (multilabel off/on).
- **nbr**: Number of balls (1 to 10).
- **physic**: Physics mode (0 to 2: None, Interpolate, Mass-Spring).
- **polyphony**: 0 or 1 (polyphony off/on).
- **rect**: Object’s position and dimensions {X,Y,W,H}.
- **zoom**: Zoom in object (1 to 50).

### Additional Execution Modes

None


## MultiSlider

The MultiSlider Object tracks movement across an array of sliders. You can “wipe” all the faders to a set value with one horizontal gesture, something that is difficult to do with traditional faders.

### Expressions

- **x**: A vector of the vertical positions of all the individual sliders.
- **z** A vector of the touch state of all the individual sliders.

### Properties

- **Name**: The name of the Object, also used as its default OSC address.
- **Horizontal**: Swaps the MultiSlider’s orientation from vertical to horizontal.
- **Bipolar**: If checked, the Object’s zero position is the slider’s mid-length.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Slider**: Number of sliders (1 to 64).
- **Gradient**: If checked, a gradient is applied to the Object’s color.
- **Multicolor**: If checked, the ‘colors’ attribute is active and can be used to change the colors of individual sliders.
- **Color**: Drag the color bar to change the “thematic” color of the sliders.
- **Light**: Controls the luminosity of your Objects. The value can range from -2 (black) to +2 (white) and can be any decimal number in-between.

### Behavior

- **Grid**: If checked, the range of values produced by the Sliders is quantized into grid steps. The maximum number of steps for the MultiSlider is 33.
- **Capture**: If Capture is checked, an Object will only react to cursors that were created inside its area. When Capture is off, the Object will react to whatever cursor is present at any moment in its area.
- **Physic**: If checked, the MultiSlider emulates the physics of an Object similar to a plucked string anchored at the left and right sides of the array of sliders. 

    - **Tension**: A value between 0 and 1 corresponding to the tension on a string. As tension increases, the frequency of oscillation of the string increases.
    - **Friction**: A value between 0 and 1 corresponding to the damping on a string. As friction increases, the damping on the oscillation increases.
    - **Height**: When Tension is enabled, the height (0 to 1) is the value of the initial and resting position of the string.

### Attributes

- **bipolar**: 0 or 1 bipolar off/on
- **capture**: 0 or 1 capture off/on
- **color**: integer* color of the object
- **colors**: {integer*, integer*, …} vector of colors for individual sliders
- **grid**: 0 or 1 grid off/on
- **grid_steps**: 1 to 33 number of grid steps
- **horizontal**: 0 or 1 horizontal off/on
- **label**: 0 or 1 label off/on
- **multicolor**: 0 or 1 multicolor off/on
- **nbr**: 1 to 64 number of sliders
- **physic**: 0 or 1 physics off/on
- **rect**: {X,Y,W,H} object’s position and dimensions

### Additional Execution Modes

None


## Pads

The Pads Object is a two-dimensional array of buttons that are triggered by touch. They are intended to trigger events instead of representing state, as they eventually return to an “off” value after being touched.

### Expressions

- **x**: A list of the envelope (brightness) values of the pads.

### Properties

- **Name**: The name of the Pads Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Numbers**: If checked, each pad is labeled with a number corresponding to the order in which its value is transmitted, starting with 0.
- **Columns**: The number of columns of pads contained in the Object (max 16).
- **Rows**: The number of rows of pads contained in the Object (max 16).
- **Multilabel**: If checked, the ‘labels’ attribute is active, and each pad can be labeled individually.
- **Multicolor**: If checked, the Object’s ‘colors’ attribute is active, and each Pad can take its own color value.
- **Color Off**: The color for the Object’s off state.
- **Color On**: The color for the Object’s on state.
- **Light**: Can be a constant, a vector, or any mathematical expression, and controls the luminosity of your Objects. -2 means black, +2 means white, and you can choose any decimal number in-between.

### Behavior

- **Capture**: If Capture is checked, an Object will only react to cursors that were created inside its area. Even if the cursor later leaves the Object for another position, it will remain in control of the original Object until it is destroyed. When Capture is off, an Object will react to whatever cursor is present at any moment in its area.
- **Attack**: The Attack value specifies the number of seconds over which the x variable (pad brightness) increases from its initial value of 0 to a maximum of 1 after you touch the screen. For example, if the Attack value is 0, the pad will be at full brightness the moment you touch the screen. An attack value of 10 means the pad will take 10 seconds to reach full brightness.
- **Decay**: The Decay value specifies the number of seconds over which the x variable (pad brightness) decreases after the initial Attack portion of the envelope has completed. During the Decay portion of the envelope, the x variable (pad brightness) will decrease from 1 to the level set by the Sustain value.
- **Sustain**: The Sustain value is the level (between 0 and 1) at which the x variable (pad brightness) will remain as long as your finger is touching the pad. The Sustain level is reached after the Attack and Decay portion of the envelope have completed. If your finger lifts up from the touch surface before the completion of the Attack and/or Decay portion of the envelope, the Release portion of the envelope is triggered immediately after the Decay portion completes, and the brightness ultimately goes to 0.
- **Release**: The Release value specifies the number of seconds over which the x variable (pad brightness) will decrease from its Sustain level to 0, starting at the moment that you lift up (“release”) your finger from the touch surface.
- **Hold**: Its effect is similar to a sustain pedal, freezing the Object’s state as long as its value is 1. When set to 0, it has no effect. This parameter should be used with a mathematical expression depending on other Objects. For instance, if you have a Switch Object named Sustain in your interface, you can set the hold parameter of a Pad to Sustain.x so the Switch gets the ability to freeze the current lightness.

### Attributes

- **capture**: 0 or 1, capture off/on
- **color**: {integer*, integer*} - color off state, color on state
- **colors**: {integer*, integer*} - Off state color values for each pad
- **column**: 1 to 16 - number of columns
- **label**: 0 or 1 - label off/on
- **labels**: {Text, Text, ..} - vector of labels for multilabel use
- **multicolor**: 0 or 1 - Multicolor off/on
- **multilabel**: 0 or 1 - Multilabel off/on
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **row**: 1 to 16 - number of rows


### Additional Execution Modes

**onPadPressed(indices)**: Called when a pad is pressed, passing in a vector of the currently pressed indices
**onPadReleased(indice)**: Called when a pad is released, passing in a vectory of the currently pressed indices


## Range

The Range Object allows for setting a range of values by moving two handles along a continuous axis, useful for setting minimum and maximum thresholds.

### Expressions

- **x**: A list of the first handle’s value, ranging from 0 to 1.
- **x[1]**: The second handle’s value, ranging from 0 to 1.

### Properties

- **Name**: The name of the Range Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Color**: The color of the handles.
- **Orientation**: The orientation of the Range Object, can be set to Horizontal or Vertical.

### Behavior

- **Capture**: If Capture is checked, an Object will only react to cursors that were created inside its area. Even if the cursor later leaves the Object for another position, it will remain in control of the original Object until it is destroyed. When Capture is off, an Object will react to whatever cursor is present at any moment in its area.
- **Grid**: Snaps the handles to a grid defined by this parameter (value from 1 to 128).

### Attributes

- **capture**: 0 or 1, capture off/on
- **color**: integer - color of the handles
- **grid**: 1 to 128 - grid snapping value
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **orientation**: 0 or 1, Horizontal or Vertical

### Additional Execution Modes

None


## RingArea

The RingArea Object is a circular controller that allows for touch-based interaction within a ring-shaped area. It’s often used for XY control in a circular space.

### Expressions

- **x**: The X position of the touch point, ranging from -1 to 1.
- **y**: The Y position of the touch point, ranging from -1 to 1.
- **z**: The pressure or touch state. 0 means no touch, 1 means touch.

### Properties

- **Name**: The name of the RingArea Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Color**: The color of the ring.
- **Size**: Adjusts the thickness of the ring.
- **Shadow**: If checked, a shadow is displayed underneath the ring.

### Behavior

- **Capture**: If Capture is checked, the Object will only react to cursors that were created inside its area. Even if the cursor later leaves the Object for another position, it will remain in control of the original Object until it is destroyed. When Capture is off, the Object will react to whatever cursor is present at any moment in its area.
- **Grid**: Snaps the touch point to a grid defined by this parameter (value from 1 to 128).

### Attributes

- **capture**: 0 or 1, capture off/on
- **color**: integer - color of the ring
- **size**: integer - thickness of the ring
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **grid**: 1 to 128 - grid snapping value
- **shadow**: 0 or 1, shadow off/on


### Additional Execution Modes

None

## SignalScope

The SignalScope Object provides a real-time graphical representation of a signal, commonly used to visualize audio or other data streams.

### Expressions

- **x**: The signal values to be displayed on the scope.

### Properties

- **Name**: The name of the SignalScope Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Color**: The color of the signal waveform.
- **Background Color**: The color of the background area behind the waveform.
- **Thickness**: Adjusts the thickness of the signal line.
- **Resolution**: Defines the number of points displayed in the waveform.

### Behavior

- **Capture**: If Capture is checked, the Object will only react to cursors that were created inside its area. Even if the cursor later leaves the Object for another position, it will remain in control of the original Object until it is destroyed. When Capture is off, the Object will react to whatever cursor is present at any moment in its area.

### Attributes

- **capture**: 0 or 1, capture off/on
- **color**: integer - color of the signal waveform
- **thickness**: integer - thickness of the signal line
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **backgroundcolor**: integer - color of the background area
- **resolution**: integer - number of points displayed in the waveform

### Additional Execution Modes

None


## SurfaceLCD

The SurfaceLCD Object is a display component used to show text or numerical values on a virtual LCD screen.

### Expressions

- **value**: The text or numerical value displayed on the SurfaceLCD.

### Properties

- **Name**: The name of the SurfaceLCD Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Color**: The color of the text displayed on the LCD.
- **Background Color**: The color of the LCD background.
- **Font Size**: The size of the font used to display the text.
- **Font**: The type of font used for displaying the text (e.g., Arial, Times New Roman).

### Behavior

- **Auto Scale**: If checked, the font size will automatically adjust to fit the text within the LCD area.
- **Alignment**: Sets the alignment of the text within the LCD (e.g., left, center, right).

### Attributes

- **color**: integer - color of the text displayed
- **fontsize**: integer - size of the font
- **font**: string - type of font used
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **backgroundcolor**: integer - color of the LCD background
- **autoscale**: 0 or 1, auto scale off/on
- **alignment**: integer - text alignment (0 = left, 1 = center, 2 = right)


### Additional Execution Modes

None

## Switches

The Switches Object is a grid of buttons that can act as toggle switches, allowing for multiple on/off states. Each switch in the grid can be independently toggled on or off.

### Expressions

- **x**: Array of values representing the state of each switch (0 = off, 1 = on).
- **selected**: Array indicating which switch is currently selected.

### Properties

- **Name**: The name of the Switches Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Color**: The color of the switches when they are on.
- **Background Color**: The color of the switches when they are off.
- **Grid Size**: Defines the number of rows and columns in the grid.
- **Border Width**: Adjusts the width of the border around each switch.
- **Spacing**: Adjusts the spacing between switches in the grid.

### Behavior

- **Mode**: Defines the behavior of the switches (e.g., momentary, toggle).
- **Orientation**: Sets the orientation of the grid (e.g., horizontal, vertical).

### Attributes

- **x**: Array - state of each switch (0 = off, 1 = on)
- **selected**: Array - indicates which switch is currently selected
- **color**: integer - color of the switches when on
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **backgroundcolor**: integer - color of the switches when off
- **gridsize**: {rows, columns} - number of rows and columns in the grid
- **borderwidth**: integer - width of the border around each switch
- **spacing**: integer - spacing between switches in the grid
- **mode**: integer - switch mode (0 = momentary, 1 = toggle)
- **orientation**: integer - grid orientation (0 = horizontal, 1 = vertical)


### Additional Execution Modes

None


## Text

The Text Object displays a static string of text on the interface. It is primarily used for labeling or providing information within the interface.

### Expressions

- **value**: The text string displayed by the Text Object.

### Properties

- **Name**: The name of the Text Object, also used as its default OSC address.
- **Label**: If checked, the Object’s name is displayed in the Interface.
- **Color**: The color of the text.
- **Font Size**: The size of the font used to display the text.
- **Font**: The type of font used for displaying the text (e.g., Arial, Times New Roman).
- **Background Color**: The background color behind the text.
- **Horizontal Alignment**: Aligns the text horizontally (e.g., left, center, right).
- **Vertical Alignment**: Aligns the text vertically (e.g., top, middle, bottom).

### Behavior

- **Auto Size**: Automatically adjusts the size of the text to fit within the object’s bounds.
- **Word Wrap**: Enables or disables word wrapping if the text exceeds the width of the object.

### Attributes

- **value**: string - the text string displayed by the Text Object
- **color**: integer - color of the text
- **fontsize**: integer - size of the font
- **font**: string - type of font used
- **rect**: {X,Y,W,H} - object’s position and dimensions
- **backgroundcolor**: integer - background color behind the text
- **halign**: integer - horizontal alignment (0 = left, 1 = center, 2 = right)
- **valign**: integer - vertical alignment (0 = top, 1 = middle, 2 = bottom)
- **autosize**: 0 or 1 - auto size off/on
- **wordwrap**: 0 or 1 - word wrap off/on

