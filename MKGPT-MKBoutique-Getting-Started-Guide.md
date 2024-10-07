# MKBoutique Controller Getting Started Guide

User guide for MKBoutique premium Lemur controllers. This guide contains instructions common to all MKBoutique Lemur controllers. For specific installation instructions, consult the user guide for your controller at support.midikinetics.com

## Installation

### 1. Download software from user account

The latest versions of our software are always available for download in your User Account at boutique.midikinetics.com
    
### 2. Download and install MKConnect

MKConnect is MIDI Kinetics’ own macOS/Windows connection app. It manages additional communications for MK products such as communication with our DAW plugins and license key servers.

Visit the MKConnect product page for download links.
    
### 3. Activate License Key

Each purchase of an MK Boutique Lemur controller includes 2 activations to run on 2 separate machines. Activate/deactivate a machine at any time to move activations to different machines.
    
1. In MKConnect, open the License Key tab.
2. Enter the license key you received with your purchase receipt. You will need an internet connection during the activation.
    
If you encounter an issue during activation, try to temporarily disable any firewalls or VPNs.
    
If for some reason you need to reset your activations, contact us and we will gladly reset them. You may need to do this, for example, if you do a clean OS install without first deactivating your license.
    
### 4. Open the project in the Lemur Editor

If your project ships as a .jzml file, just double click it and it will open in the Lemur Editor.

If your project ships as a .jzlib file, right-click anywhere in the Project window and select import. (You must create an interface first).

### 5. Transfer the projec to the Lemur App

1. Launch Lemur on your mobile device.
2. In the Lemur Editor, click the Connections triangle at the top-right corner.
3. Select your device from the list and click Connect. Your mobile device and your computer must be on the same mobile network.

### 6. Assign Lemur Targets

Every MK Boutique Lemur Controller requires 2 Lemur Targets to manage 2 independent MIDI connections:

1. A connection to your DAW
2. A connection to MKConnect

To change the Lemur Targers for your MK Controller, tap/swipe the MK logo and go to Prefs > Connections

New users can leave these at their default settings of 0 and 1. It only becomes necessary to change the Lemur Targets when combining controllers.

### 7. Connect MIDI to Targets

1. Tap the cog-wheel icon at the top-right of the Lemur App and go to More Settings...
2. Assign unique MIDI connections for each Target
3. Open MKConnect and assign the MIDI Connection connected to the MKConnect Target.
4. Open your DAW and asign the MIDI connections connected with the controller's main target. Every MK controller is specific to each DAW, so this step will vary. Consult the product manual for steps to integrate the controller with your DAW.

## Combining MKBoutique Lemur Controllers

It is possible to combine multiple MK Boutique Lemur controllers onto the same mobile device, or split up your installations across multiple devices. 

### Installing multiple controllers on the same device

All controllers can share the same MIDI connection to MKConnect. MKConnect knows how to share a single connection to a device and disambiguate between which controller it is talking to

1. Open the Lemur project file that contains the controller you want to combine.
2. Select its outermost project container and copy it to your clipboard.
3. Open your main project that will contain both controllers, and paste it.
4. Tap/Swipe the MK Logo in the first project and go to Prefs > Connections, and assign its Main and MKConnect Target.
5. Tap/Swipe the MK Logo in the second project and assign the MKConnect Target to the same MKConnect Target, assign its Main Target, and assign its MKConnect Target to the same Target as the first.


### Splitting installations across multiple devices

Each device must maintain its own connection to MKConnect. It is not possible to use the same MIDI connection between 2 hardware devices. 

## Example Routings

### Example 1: A single controller on a single device

#### Prefs > Connections

- Main Target In: 0
- Main Target Out: 0
- MKConnect Target In: 1
- MKConnect Target Out: 1

#### Lemur > More Settings

- MIDI 0 "From": Daemon Output 0
- MIDI 0 "To": Daemon Input 0
- MIDI 1 "From": Daemon Output 1
- MIDI 1 "To": Daemon Input 1

#### MKConnect Connections Tab

- Tablet 1 "To" : Daemon Output 1
- Tablet 1 "From" : Daemon Input 1

#### DAW

- Input : Daemon Input 0
- Output: Daemon Output 0

### Example 2: Two controllers on a single device

Both controllers can share the same connection to MKConnect.

#### Prefs > Connections

Controller 1 Preferences

- Main Target In: 0
- Main Target Out: 0
- MKConnect Target In: 2
- MKConnect Target Out: 2

Controller 2 Preferences

- Main Target In: 1
- Main Target Out: 1
- MKConnect Target In: 2
- MKConnect Target Out: 2

#### Lemur > More Settings

- MIDI 0 "From": Daemon Output 0
- MIDI 0 "To": Daemon Input 0
- MIDI 1 "From": Daemon Output 1
- MIDI 1 "To": Daemon Input 1
- MIDI 2 "From": Daemon Output 2
- MIDI 2 "To": Daemon Input 2

#### MKConnect Connections Tab

- Tablet 1 "To" : Daemon Output 2
- Tablet 1 "From" : Daemon Input 2

#### DAW

Controller 1

- Input : Daemon Input 0
- Output: Daemon Output 0

Controller 2

- Input : Daemon Input 1
- Output: Daemon Output 1


### Example 3: Two controllers on two devices

Both controllers need their own independent connections to MKConnect.

#### Prefs > Connections

Controller 1 Preferences

- Main Target In: 0
- Main Target Out: 0
- MKConnect Target In: 1
- MKConnect Target Out: 1

Controller 2 Preferences

- Main Target In: 0
- Main Target Out: 0
- MKConnect Target In: 1
- MKConnect Target Out: 1

#### Lemur > More Settings

Controller 1 Settings

- MIDI 0 "From": Daemon Output 0
- MIDI 0 "To": Daemon Input 0
- MIDI 1 "From": Daemon Output 1
- MIDI 1 "To": Daemon Input 1

Controller 2 Settings

- MIDI 0 "From": Daemon Output 2
- MIDI 0 "To": Daemon Input 2
- MIDI 1 "From": Daemon Output 3
- MIDI 1 "To": Daemon Input 3

#### MKConnect Connections Tab

- Tablet 1 "To" : Daemon Output 1
- Tablet 1 "From" : Daemon Input 1
- Tablet 2 "To" : Daemon Output 3
- Tablet 2 "From" : Daemon Input 3

#### DAW

Controller 1

- Input : Daemon Input 0
- Output: Daemon Output 0

Controller 2

- Input : Daemon Input 2
- Output: Daemon Output 2


## Transferring Settings to New Versions


All MK Boutique controllers contain a User folder that contains your settings. To transfer your settings to a new version, simply copy/paste it into the new version.

1. In the Lemur Editor, open your old project.
2. Locate the User Folder in the project navigator. Select it, and copy it (CMD/CTL + C).
3. Open the new version.
4. Select the existing User Folder, and delete it.
5. Select the outermost project container, and paste (CMD/CTL + V).
