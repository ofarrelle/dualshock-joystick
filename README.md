# dualshock-joystick
Pilot Google Earth Flight Simulator with a Playstation Dualshock 4 controller on Windows

Google Earth's built-in flight simulator is pretty neat but very hard to control without a joystick. Earth comes with preset configuration files for many legitimate airplane simulator joysticks, and interestingly for the XBox 360 controller, but not for any Playstation remotes. Here's how to use the Dualshock 4 to control your airplane (as a bonus, completely wirelessly).

## In-Simulator Controls

The configuration file [dualshock_4.ini](https://github.com/ofarrelle/dualshock-joystick/blob/master/dualshock_4.ini) sets up the controls as such:

Button/Gimbal             | Output                     |
|---------------------------|----------------------------|
| R1                        | Throttle up in 10% steps   |
| L1                        | Throttle down in 10% steps |
| Left joystick horizontal  | Rudder                     |
| Right joystick horizontal | Aileron                    |
| Right joystick vertical   | Elevator                   |
| Triangle                  | Flaps up in 50% steps      |
| X                         | Flaps down in 50% steps    |
| D-Pad                     | Look around                |

These controls are similar to using a Mode 2 transmitter to fly drones or RC airplanes, with the exception that throttle is moved to R1 and L1 to avoid the self-centering left gimbal.

## Setup

Download Google Earth Pro from https://www.google.com/earth/versions/#earth-pro. Flight Simulator is automatically included (at least it was back in 2020 when I downloaded it).

In the file system, navigate to `C:\Program Files\Google\Google Earth Pro\client\res\flightsim\controller`. Drop [dualshock_4.ini](https://github.com/ofarrelle/dualshock-joystick/blob/master/dualshock_4.ini) in this folder, alonside all the other `.ini` files that already exist.

Connect your Dualshock 4 to your PC via Bluetooth (I have not tested the wired method using DS4Windows). To do this, hold the PS and Share buttons on the Dualshock until the LED bar starts to flash. The Dualshock is now in pairing mode. Then on Windows, open Bluetooth Settings -> Add Bluetooth or other device -> Bluetooth, then find the wireless game controller.

From here you should be good to go! Boot up Google Earth, and go to Tools -> Enter Flight Simulator.

## Troubleshooting

1. No response to controls in the simulator? Check the name of the controller. Search for and launch `Set up USB game controllers` in the Windows Start menu. The only entry in the list should be the Bluetooth-connected Dualshock 4. Make sure it is called `Wireless Controller`. If it's not, then change the regex in line 64 of [dualshock_4.ini](https://github.com/ofarrelle/dualshock-joystick/blob/master/dualshock_4.ini) to match the name that appears in the Game Controllers list you just found.
2.  Wonky controls? Might be time to edit the configuation file. The button/gimbal maps I found are listed below in the **Development Guide**, yours may be different than mine.

## Development Guide

Google Earth interprets all possible inputs on the Dualshock in one of 3 categories

1. Axis (left and right joystick)
2. Button (buttons, bumbers, triggers)
3. Point of View (D-pad)

In the configuration file, all input values are defined as variables: axes `A0, A1, ... AN`, buttons `B0, B1, ... BN`, and POV input `P0` (there is likely more, but I haven't looked yet). The button/gimbal mapping is as follows.

| Button/Gimbal             | Corresponding Variable |
|---------------------------|------------------------|
| Left joystick horizontal  | A0                     |
| Left joystick vertical    | A1                     |
| Right joystick horizontal | A2                     |
| Right joystick vertical   | A5                     |
| R1                        | A3                     |
| L1                        | A4                     |
| Square                    | B0                     |
| X                         | B1                     |
| Circle                    | B2                     |
| Triangle                  | B3                     |
| L1                        | B4                     |
| R1                        | B5                     |
| L2                        | B6                     |
| R2                        | B7                     |
| Share                     | B8                     |

For example, when the left joystick is moved horizontally, the A0 variable changes value. Similarly when the Triangle button is pushed, the B3 variable registers an input. Variables P0 (and potentially P1, P2, P3) I have not experiemented with. Recall, these should be associated with the D-Pad. Also, R1 and L1 each map to both a button *and* and axis.

In case you ever want to add more functionality (ie. landing gear up/down), start with this button mapping. The functions used to define the effect's corresponding with these variables are laid out in [dualshock_4.ini](https://github.com/ofarrelle/dualshock-joystick/blob/master/dualshock_4.ini)

Happy flying!
