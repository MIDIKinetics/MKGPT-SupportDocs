# Lemur MIDI Programming Guide

## MIDI Setup

For MIDI setup instructions, see the Getting Started With Lemur guide. 

## Lemur Targets

Often when desigining a MIDI controller you will want to group related controls onto the same output. Use a *Lemur Target* to define a group, then connect that target to one of your Virtual or Physical MIDI hardware connections. Lemur comes with 8 Lemur Targets, numbered from 0-7.

To see how Lemur Targets are mapped to physical devices

1. Open the Lemur App on your mobile device
2. Tap the Cog-Wheel icon at the top right and go to More Settings...
3. Scroll down to MIDI Targets

    - Connections in the "From" column are inputs into your mobile device
    - Connections in the "To" column are outputs to the computer
    
There are 2 ways to define what Lemur Target MIDI messages are sent:

1. Through the MIDI Mapping Panel
2. Programmatically in code


## MIDI Mapping Panel

The MIDI Mapping Panel offers a simplified way to assign MIDI messages to UI widgets through the Lemur Editor. While useful for beginners or simple setups, advanced users should consider generating MIDI messages programmatically for greater control.

1. Click a UI widget in the project window
2. Open the MIDI Mapping Panel


### Object Target

Sets the default Lemur Target to send and receive MIDI. 

#### Target Cascading

You can set the Lemur Target to "Parent" or "Project" to inherit the target from higher up in the UI hierarchy.

- Project: Uses the Lemur Target defined at the project level.
- Parent: Uses the Lemur Target of the parent widget.
        
    
Example:

- Project: Target 0
    - Interface  
        - Container: Parent (Target 0)  
            - Fader: Parent (Target 0) 
        - Container: Target 1
            - Fader: Parent (Target 1) 
            - Fader: Project (Target 0)



Tip: For larger projects, use cascading by setting the top container's or project's' target and letting child widgets inherit it. This simplifies remapping the UI.

### Expression Menu

Selects the expression that triggers the MIDI message (e.g., a fader’s x expression, which changes when the fader moves).

### Target  Menu

Sets the Lemur Target for the selected expression.

### Message Menu

Sets the type of MIDI message to send for the selected expression:

- 80 - Note On
- 90 - Note Off
- A0 - Key Pressure
- B0 - Control Change
- C0 - Program Change
- D0 - Channel Pressure
- E0 - Pitch Bend
- F0 - System Exclusive
- F2 - Song Position
- F3 - Song Select
- F5 - Bus Select
- F6 - Tune Request
- F7 - Timing Tick
- FA - Start Song
- FB - Continue Song
- FC - Stop Song
- FE - Active Sensing
- FF - System Reset

### Additional Options

The available options depend on the selected message type. For instance, a Control Change message on a fader’s x expression might show the following:


- Controller 11 to 11

    Defines the MIDI controller range (e.g., Controller 11). For widgets like Pads, you may want to specify a range of controller values.
    
- value: x[0] 
    
    Specifies the expression to evaluate. For a Fader, this is typically x[0]; for a multi-dimensional Pads widget, it could be x[0..n] depending on how many pads are present.

- Condition: any

    Defines the condition under which the expression triggers a MIDI message. Conditions mirror those in script execution modes:
    - Any: Triggers on any change in the expression.
    - Rising from 0: Triggers when the expression value leaves 0.
    - Falling to 0: Triggers when the expression value returns to 0.
    - Reaches or Leaves 0: Triggers on either transition to or from 0.
    - Increasing: Triggers when the expression increases.
    - Decreasing: Triggers when the expression decreases.

- Scale 0 to 127

    Maps the expression’s range (e.g., 0.0–1.0) to MIDI values (e.g., 0–127).
    
- Channel 1 to 1

    Defines the MIDI channel range (e.g., Channel 1).

## Programmatic MIDI

### Programmatic MIDI Output

LemurLang provides a number of convenient functions to send common MIDI messages in code. 

**Important: For all channel voice messages, the `channel` argument is 1 based (valid channel ranges are 1 to 16). All other arguments are 0 based.** 

- `ctlout(target, ctrl, val, chan)`

    Sends a MIDI control change message out Lemur target `target` with data-byte 1 `ctrl` and data-byte 2 `val`, on MIDI channel `chan`.
    
- `noteout(target, note, vel, chan)`

    Sends a MIDI note message out Lemur target `target`, with data-byte 1 `note`, and data-byte 2 `vel`, on MIDI channel `chan`. Sending `vel` 0 sends a MIDI note-off message.
    
- `programchangeout(target, pgm, chan)`

    Sends a MIDI program change message out Lemur target `target`, with data-byte 1 `pgm`, on MIDI channel `chan`.
    
- `channelpressureout(target, pressure, chan)`

    Sends a MIDI channel pressure message out Lemur target `target`with data-byte 1 `pressure`, on MIDI channel `chan`.
    
- `polypressureout(target, note, pressure, chan)`

    Sends a MIDI poly pressure message out Lemur target `target` with data-byte 1 `note` and data-byte 2 `pressure` on MIDI channel `chan`.
    
- `pitchbendout(target, value, min, max, chan)`

    Sends a MIDI pitchbend message on Lemur target `target`, with value `value`, where `value` represents a value in the range of `min` and `max` on MIDI channel `chan`.
    
    Common ranges for `min` and `max`:
    
    - `min` = 0, `max` = 1 (center = 0.5)

        Useful for Lemur fader widgets who specify their position as a floating point number between 0 and 1. The center is at 0.5.
        
        Example:
        
        `pitchbendout(0, Fader.x, 0, 1, 1)`
    
    - `min` = -1, `max` = 1 (center = 0)
    - `min` = 0, `max` = 16383 (center = 8192)
    - `min` = -8192, `max` = 8191 (center = 0)

- `sysexout(target, databytes[])`

    Sends a MIDI sysex message out Lemur target `target` with the supplied `dataBytes`. 
    
    Note:
    - You do not need to specify the sysex-start or sysex-end data bytes. These will be added to the message before is sent.
    - Data bytes are currently limited to 128 values.

    Example:
    
    ```
    sysexout(0, {0, 0, 11, 12});
    ```

- `midiout(target, msg[])`

    Sends an arbitrary MIDI message out Lemur target `target` with the provided array of bytes `msg[]`. Use this method for more programmatic construction of MIDI data, or to construct a MIDI message that is not implemented above.

    
    Example: Sending MIDI Control Change 11 out Lemur Target 0 with the value of 120, on Channel 2:
    ```
    midiout(0, {0xB0 + 2, 11, 120});
    ```
    
    Example: Sending a long sysex message that needs to be split into multiple packets because Lemur arrays are limited to 256 elements:
    
    ```
    midiout(0, {0xF0}); // sysex start
    midiout(0, getPart1DataBytes()); 
    midiout(0, getPart2DataBytes());
    midiout(0, {0xF7}); // sysex end
    ```
    
### Programmatic MIDI Input

1. Create a script and set its execution mode to "On MIDI". The Lemur Editor will automatically modify the function signature to take in an argument of `MIDI_ARGS`, which will be an array of bytes to parse.

    ```
    onMIDI(MIDI_ARGS)
    ```
    
Next, above the code editor will be a series of drop-down menus and text fields to filter for specific MIDI messages:

2. Select the type of MIDI messsage  (e.g. note, control change)
3. Select the Lemur input target 
4. If applicable, enter a range of data byte 1 values, e.g. to listen for only MIDI controller 11, set both the min and max values to 11
5. If applicable, enter a range of MIDI channels (valid range 1...16)

*Note: You cannot listen to all MIDI message on all Lemur Targets from a single script. Create a new script for each type of message:*

```
onControlChange(MIDI_ARGS)
onNotes(MIDI_ARGS)
onSysex_target3(MIDI_ARGS)
etc.
```

#### Parsing `MIDI_ARGS`

*Important:*

The MIDI data array that comes in via `MIDI_ARGS` is formatted in a different way than their corresponding output functions described in Programmatic MIDI Output, and in a different order than the standard MIDI specification. **Most notably, the MIDI channel component is always 0 based (valid range 0...15), despite the filter fields specifying the range 1...16.**

This was implemented by the original manufacturers of the Jazzmutant Lemur MIDI Controller Hardware Device, and the reasons for this are unknown and not properly documented in the Lemur User Guide.

- 80 - Note Off 
    - `MIDI_ARGS[0]` = Note (data-byte 1)  
    - `MIDI_ARGS[1]` = Velocity (data-byte 2)  
    - `MIDI_ARGS[2]` = Channel (0...15)  

- 90 - Note On
    - `MIDI_ARGS[0]` = Note (data-byte 1)  
    - `MIDI_ARGS[1]` = Velocity (data-byte 2)  
    - `MIDI_ARGS[2]` = Channel (0...15)  

- A0 - Key Pressure  
    - `MIDI_ARGS[0]` = Note (data-byte 1)  
    - `MIDI_ARGS[1]` = Velocity (data-byte 2)  
    - `MIDI_ARGS[2]` = Channel (0...15)  
    
    *Note: Not all DAWs transmit the note component. `MIDI_ARGS[0]` will always be 0.*
    
- B0 - Control Change
    - `MIDI_ARGS[0]` = Controller Number (data-byte 1)  
    - `MIDI_ARGS[1]` = Controller Value (data-byte 2)  
    - `MIDI_ARGS[2]` = Channel (0...15)  
    
- C0 - Program Change  
   - `MIDI_ARGS[0]` = Program Number (data-byte 1)  
   - `MIDI_ARGS[1]` = Channel (0...15)  
   
- D0 - Channel Pressure  
    - `MIDI_ARGS[0]` = Pressure (data-byte 1)
    - `MIDI_ARGS[1]` = Channel (0...15)  

- E0 - Pitch Bend
    - `MIDI_ARGS[0]` = 14-bit pitchbend (0...16383, where 8192 = center) 
    - `MIDI_ARGS[1]` = Channel (0...15)  

- F0 - System Exclusive
    
    System exclusive message come in the following format:

    - The leading Sysex Start message is not included
    - The bytes before the last byte are the data bytes
    - The last byte is the Sysex End message (0xF7, or 247 in decimal)
    
    Example: A MIDI Sysex Message with data bytes 1, 2, 3
    
    `{1, 2, 3, 247}` 
    
    Example: Parsing a long sysex-message. Lemur arrays are limited to 256 values, so break up a single buffer into multiple sub-buffers.
    
    
    - MIDIWidget
        - `onLoad()`  
        - `onSysex(MIDI_ARGS)`  
        - `buffer0`  
        - `buffer1`  
        - `buffer2` 
        - `numBuffers`  
        - `parse()`  
        - `clear()`  
        - `tail`  
        - `dataByte(index)`  
    
    Code:
    
    ```
    onLoad() {
        // Execution Mode: "On Load"
        numBuffers = 3;
        clear();
    }
    
    clear() {
        decl i;
        for (i = 0; i < numBuffers; i++) {
            decl expression = 'buffer' + i;
            setexpression(getobject(), expression, fill(1, 0, 256));
        }
        tail = 0;
    }
    
    dataByte(index) {
        decl bufferIndex = floor(index/256);
        decl readIndex = index % 256;
        decl expression = 'buffer' + bufferIndex;
        decl bufferContent = getexpression(getobject(), expression);
        return bufferContent[readIndex];
    }
    
    onSysex(MIDI_ARGS) {
        decl i;
        for (i = 0; i < sizeof(MIDI_ARGS); i++) {
            if (MIDI_ARGS[i] == 247) { // Sysex end
                parse(); // Parse the completed sysex message
                clear(); // Clear the buffer 
            } else {
                // Calculate which buffer and index to write to
                decl bufferIndex = floor(tail/256);
                decl expression = 'buffer' + bufferIndex;
                decl writeIndex = tail % 256;
                
                // Get current buffer content
                decl this = getobject();
                decl bufferContent = getexpression(this, expression);
                
                // Store the MIDI data byte and update the buffer
                bufferContent[writeIndex] = MIDI_ARGS[i];
                setexpression(this, expression, bufferContent);
                
                // Increment the tail for the next byte
                tail++;
            }
        }
    }
    parse() {
        decl i;
        for (i = 0; i < tail; i++) {
            decl data = dataByte(i);
            // parse data
        }
    }
    ```
The remaining messages are not consistently implement by all DAWs. Consult the DAW user guide to see how they are used. This may require some experimentation.

- F2 - Song Position
- F3 - Song Select
- F5 - Bus Select
- F6 - Tune Request
- F7 - Timing Tick
- FA - Start Song
- FB - Continue Song
- FC - Stop Song
- FE - Active Sensing
- FF - System Reset


## Common Pitfalls 

### Connection Errors

The MIDI Kinetics support website has a complete list of troubleshooting steps. Please refer to this web page: 

https://support.midikinetics.com/knowledge-base/troubleshooting-midi-connections/

### Feedback Loops in Programmatic MIDI

Feedback loops occur when the output of a MIDI port is sent back to its input, causing a never-ending loop. This is a common source of bugs, lock-ups, and freezes in programmatic MIDI controls. (Connections made in the MIDI Mapping Panel automatically break feedback loops).

Example: 

A Fader sends a MIDI control change message when its `x` position changes, but also updates its `x` position when MIDI comes in, causing an infinite loop.

- Fader  
    - `onX()`  
    - `onMIDI(MIDI_ARGS)`  

Code:
```
onX() {
    // Execution Mode: On Expression
    // Expression: x
    // Condition: any
    
    // When `x` changes, sends MIDI CC11 
    // on Channel 1 on Target 0
    ctlout(0, 11, x * 127, 1);
}

onMIDI(MIDI_ARGS) {
    // Execution Mode: On MIDI
    // Message Type: B0 - Control Change
    // Target: 0
    // Data Byte 1: 11 to 11
    // Channel: 1 to 1
    
    // Updates `x`, which triggers `onX()`
    // causing an infinite loop
    x = MIDI_ARGS[1] / 127;
}

```


Solution: 

First check if the user's finger is on the fader by checking its `z` property.

```
onX() {
    // Only send MIDI if the fader is being
    // touched (z == 1)
    if (z) {
        ctlout(0, 11, x * 127, 1);
    }
}
```

### Misunderstanding Lemur Targets

A common challenge for new Lemur users is understanding how Lemur Targets work.

This is understandable, because when musicians use their DAW they are used to seeing all the virtual and physical MIDI ports available for connection.

Lemur, however, is a generic, cross-platform MIDI controller, and does not know in advance what connections will be available in a music studio. Unlike a DAW, where MIDI tracks are directly connected to physical devices, Lemur can't reference specific ports in code. For example, the following wouldn't work:

Not Possible:
```
ctlout('My USB MIDI Connection', 11, x * 127, 1)
```

Even if this were possible, changing studio hardware—such as adding a new MIDI interface—would break the project. Musicians are all too familiar with opening an old DAW project only to see numerous errors, "<!> Missing MIDI Port <!>", requiring time-consuming troubleshooting and updates. 


Lemur avoids this issue by using Lemur Targets, which act as generic MIDI groups. You can write your project code once, using Lemur Targets, and later map these targets to physical MIDI ports via More Settings > MIDI Targets in the Lemur App. This makes the project flexible and reduces the need to manually update connections or code when studio setups change.

```
// Send MIDI control change 11 on Target 0
// regardless of the physical port
ctlout(0, 11, x * 127, 1); 
```

While this adds some initial cognitive overhead, an effective analogy is to think of Lemur Targets like audio mix buses. Just as individual instrument tracks are routed to a mix bus and then to physical outputs, Lemur Targets route related controls to a single Lemur Target, which are then mapped to physical MIDI ports.
