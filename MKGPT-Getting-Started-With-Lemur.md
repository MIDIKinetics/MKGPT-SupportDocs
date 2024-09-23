# Lemur Getting Started Guide

## Intro to Lemur

Lemur is an iOS app that allows musicians to design custom MIDI and OSC controllers for their mobile devices. Users can create interfaces with buttons, faders, toggles, and more. LemurLang, the built-in programming language, enables reactive programming for dynamic interfaces that respond to app state or MIDI/OSC input. Advanced features include the Canvas Widget for custom drawings and animations.

The Lemur Editor for macOS/Windows acts as both a WYSIWYG and code editor, synchronizing with the Lemur app in real time for instant feedback during design. Lemur supports 8 bidirectional MIDI ports, allowing connections to up to 8 devices simultaneously. Easy for beginners and powerful for professionals, Lemur has been a staple in music production studios for over 20 years. Lemur has even been used to create games!

*Note: Android support coming early 2025* 

## Requirements

- A 64 bit mobile device such as an iPhone or iPad running iOS 13 or later.
- A macOS or Windows computer running macOS 14 or later, or Windows 10 or later.
- A virtual or physical MIDI connection. See "Connecting MIDI" below.

## Installing Lemur

1. On your mobile device: Visit the App Store and purchase the Lemur App.
2. On your Mac/Windows computer: Go to www.midikinetics.com/Lemur and download the Lemur Desktop Software package. This package includes the Lemur Editor and the Lemur Daemon MIDI client for managing connections.

## Installing MIDI 

In order to send MIDI messages from your mobile device to your computer, you will need some way to connect MIDI to your computer. 

- macOS

    The Lemur Daemon will install 8 virtual MIDI ports for free, usable both wirelessly or through a thunderbolt/USB connection

- Windows: Requires third-party MIDI software or hardware.

    - Wireless: Use LoopBE or LoopBE30
    - Wired: Purchase a hardware device like the iConnectMIDI.

It is also possible connect Lemur to hardware MIDI gear, such as a MIDI-enabled piano or effects processor. These devices typically use 5-pin MIDI connections. For this, you’ll need a USB-to-MIDI hub, like an iConnect product. 

Using one of these hubs, it is also possible to simultaneously connect Lemur to a computer and to hardware MIDI gear. 

## Connecting MIDI

When designing a MIDI controller it's typical to group related controls onto a single MIDI connection. For example, you might have a mix-console emulator on one MIDI connection, and a virtual instrument controller on another. 

Use a *Lemur Target* to define a group. Lemur provides 8 Targets (numbered 0-7) for connecting up to 8 ports of bidirectional MIDI. Lemur Targets can then be mapped to physical or virtual MIDI connections.

To connect Lemur Targets to MIDI:

1. Open the Lemur App on your mobile device.
2. Tap the Cog-Wheel icon at the top right of the screen. On iPadOS, tap  "More Settings..."
3. Scroll down to MIDI Targets.
4. Tap one of the Targets to see a list the available physical and virtual MIDI ports discovered by Lemur.

    - The "From" column lists incoming connections to your mobile device
    - The "To" column lists outgoing connections to your computer


For more information on Lemur Targets, refer to the Lemur MIDI Programming Guide. 

## Connecting OSC

Similar to MIDI Targets, Lemur uses OSC Targets. On the same page as the MIDI Targets, you'll find a list of OSC Targets. Here, you can add the IP address of an OSC-enabled device.

## Using the Lemur Editor

The Lemur Editor for macOS/Windows is your main tool for designing controllers, offering both a WYSIWYG editor and code editor for writing LemurLang. 

**Important: The Lemur Editor does not have the ability to connect to MIDI. It is purely an interface builder. To use MIDI, transfer your project to the Lemur App. See below: "Connecting the Lemur Editor to the Lemur App.**

### Lemur Project File Types

Lemur projects are text files in a modified XML format, with one of 2 file extensions:

- `.jzml` 

    "Jazz Editor Markup Language". These are complete Lemur projects that can be opened in the Lemur Editor.

- `.jzlib`:

    "Jazz Editor Library." These are smaller widget libraries that can be exported or imported into existing projects.

### Connecting the Lemur Editor to the Lemur App

The Lemur Editor syncs with the Lemur app to provide real-time feedback as your design your controller. 

To connect the Lemur Editor to the Lemur App:

1. Launch the Lemur App on your mobile device.
2. At the top right of the Lemur Editor, click the triangle icon.
3. Select your mobile device in the list
    - Choose "Connect" to send the contents of the Lemur Editor to the Lemur App
    - Choose "Download" to send the contents of the Lemur App back to the Editor. 
    

If running Lemur on multiple devices, be sure to select the correct one. Devices can be identified from their public name or by IP address. 


**Important: Selecting either Connect or Download will overwrite the current contents of the destination. Be sure to save first.** 

If your device isn't listed, ensure both your computer and mobile device are on the same WiFi network. If necessary, particularly on Windows, you may need to use a static IP address—though this could indicate a deeper issue with your WiFi settings.

For further troubleshooting, visit the MIDI Kinetics support website or your manufacturer or Operating System's Wifi Settings Guide.

### Adding Widgets

- Drag widgets from the Object Palette Panel onto the canvas.
- Resize and position them as desired.
- Click on a widget to access its settings.

For more details, refer to the Lemur UI Programming Guide.

### Simulating Touches

To simulate touches while designing your controller, hold the "E" key on your keyboard.

Note: The state of controls in the Lemur Editor might differ from the state in the Lemur App. For example, if you tap a button in the Lemur Editor to highlight it, it might not appear highlighted in the Lemur App. This is normal. 

## Saving Projects

Lemur projects can be saved both to your computer and to the Lemur app. **It is highly recommended to keep backups of your project on both.**


Saving to Your Computer: 
    
- Click the Disk Icon at the top left of the Lemur Editor, or use the key command CMD+S (Save) or Shift+CMD+S (Save As).

Saving to Your mobile Device:
    
- **iPhone**: 
    - Tap the cog-wheel icon at the top-right of the Lemur App.  
    - Tap the project name under "Projects". 
    - Tap "Save As" at the top right.
    
- **iPadOS**:
    - Tap the cog-wheel icon at the top-right of the Lemur App.
    - Tap "Save Project"...

## Importing and Exporting jzlib Files

- To import a jzlib file, right-click anywhere in project canvas and select "Import".
- To export a jzlib file, right-click on the widget and select "Export".



## Where to Go From Here

Lemur is a feature-rich program with over 20 years of development. This guide was just a starting point. For more in-depth information, it’s highly recommended that you read the full Lemur User Guide, available for download on the MIDI Kinetics website. The guide includes tutorials, detailed feature explanations, and an introduction to scripting with LemurLang. Additionally, see the Lemur User Guide Addendum for a complete API reference for Lemur's powerful Canvas and Step Sequencer Widgets.

### Additional Resources:

- **YouTube**:  

    Fans of Lemur have created numerous video tutorials showcasing how they use Lemur in production and studio settings, as well as sharing project ideas.

- **Lemur Community Forum (coming soon)**:  

    MIDI Kinetics will soon reopen the Lemur Community Forum for users to ask questions and get programming help. Meanwhile, you can access the existing forum from Lemur's previous developers at [http://forum.liine.net/](http://forum.liine.net/).

- **ChatGPT**:  

    MIDI Kinetics has created a chatbot trained on Lemur's documentation. While not perfect, it can answer many questions.

- **Other Music-Related Forums**:  

    Lemur's popularity has led to discussions in various music production forums, such as vi-control.net, and forums for major DAWs like Ableton and Cubase.

- **MIDI Kinetics Support**:  

    If you're having technical issues or can't find the answers you need, visit the MIDI Kinetics support website. Note that MIDI Kinetics cannot provide general programming assistance. Computer programming is a deep topic, and good programming practices apply to LemurLang just as they do to any other programming language, such as JavaScript or C++. The ChatGPT chatbot is the best place to ask for programming help as it can not only write code for you, it can also teach you about programming in general.

- **MIDI Kinetics GitHub**:  

    MIDI Kinetics shares free-to-use modules on GitHub that you can incorporate into your Lemur projects. You’re welcome to request features, report bugs, or contribute your own features for consideration in public releases.
    
- **MKBoutique**:  

    MIDI Kinetics offers powerful pre-made Lemur projects for purchase at [boutique.midikinetics.com](https://boutique.midikinetics.com). These products are typically geared towards the professional film scoring community.


